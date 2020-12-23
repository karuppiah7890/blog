---
title: "Optionally Running Services in Docker Compose (v3 and v2)"
date: 2020-12-23T15:34:35+05:30
---

Today I learned a new thing when someone asked something -

"We have two services in docker compose file. How to run one service optionally
based on some feature toggle like environment variable or command line
option? Is it possible to do this in a single Docker Compose file?"

## TLDR

One way is to control the replica count. If replica count is 0, then it means
no instance of the service is running. How to do this?

- If you are using version v3.x Docker Compose file format - you can use
  `<service>.deploy.replicas` field to control the replica count, along with
  environment variable based variable substitution in Docker Compose yaml file
  for the feature toggle.
- If you are using version >= v2.2 Docker Compose file format - you can use
  `<service>.scale` field to control the replica count, along with environment
  variable based variable substitution in Docker Compose yaml file for the
  feature toggle. I haven't tried this one personally though.

I couldn't find a solution for v1.x as there doesn't seem to be a way to control
replica count using the Docker Compose file spec according to the official docs

## Longer Version

The alternate solutions / options that were already researched by the Original
Poster (OP) were -

1. To have two separate docker compose files - one for each service and run one
   of them optionally
2. To have two separate docker compose files, where one contains both the
   services and other contains only service. And decide which compose to take.

I haven't tried out the above options though. But both options look straight
forward and simple to implement.

But the OP was more interested in a solution where only one Docker Compose file
is enough to solve this.

Note: Before going into the post, note that I use the latest version of Docker
and Docker Compose in this case. Due to this I can use the latest Docker
Compose file format which is `3.9` as of this writing. If you can't use v3.x
Docker Compose file format, it's okay, I'll post the related field names and
doc links for versions >= v2.2, but I can't test them so try it out on your own
:) And like I mentioned, I don't have any solution for version v1.x

Now, I was curious about this problem and started digging the docs to see if the
Docker Compose file reference has any features / solutions for this

https://docs.docker.com/compose/compose-file/compose-file-v3

> For v2
> https://docs.docker.com/compose/compose-file/compose-file-v2

I noticed and realized that there's a field called `replicas` and got curious.
I was like "what if we could use replica count as 0?"

https://docs.docker.com/compose/compose-file/compose-file-v3/#replicas

> For v2
> https://docs.docker.com/compose/compose-file/compose-file-v2/#scale

I also learned recently from another colleague that Docker Compose file has
some support for environment variables and substituting them in the file. But I
had never used it. I started looking for that and found it

https://docs.docker.com/compose/compose-file/compose-file-v3/#variable-substitution

> For v2
> https://docs.docker.com/compose/compose-file/compose-file-v2/#variable-substitution

I started meddling with a sample docker compose file. The below one runs a
simple redis

```yaml
version: "3.9"
services:
  redis:
    image: redis:alpine
    deploy:
      replicas: $REPLICAS
```

```bash
$ docker-compose up
WARNING: The REPLICAS variable is not set. Defaulting to a blank string.
ERROR: Error while attempting to convert service.redis.deploy.replicas to appropriate type: "" is not a valid integer
```

Interesting. Now, with replica count as 0 using the `REPLICAS` environment
variable

```bash
$ REPLICAS=0 docker-compose up
Creating network "docker-compose-demo_default" with the default driver
Attaching to

$ docker-compose ps
ERROR: Error while attempting to convert service.redis.deploy.replicas to appropriate type: "" is not a valid integer

$ REPLICAS=0 docker-compose ps
Name   Command   State   Ports
------------------------------
```

It worked! It didn't run the redis! Now let's try running the redis by
turning on the feature toggle, that is, the environment variable

```bash
$ REPLICAS=1 docker-compose up
Creating network "docker-compose-demo_default" with the default driver
Creating docker-compose-demo_redis_1 ... done
Attaching to docker-compose-demo_redis_1
redis_1  | 1:C 23 Dec 2020 10:10:53.384 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis_1  | 1:C 23 Dec 2020 10:10:53.384 # Redis version=6.0.9, bits=64, commit=00000000, modified=0, pid=1, just started
redis_1  | 1:C 23 Dec 2020 10:10:53.384 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis_1  | 1:M 23 Dec 2020 10:10:53.386 * Running mode=standalone, port=6379.
redis_1  | 1:M 23 Dec 2020 10:10:53.387 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
redis_1  | 1:M 23 Dec 2020 10:10:53.387 # Server initialized
redis_1  | 1:M 23 Dec 2020 10:10:53.390 * Ready to accept connections
```

So it worked! With one more example service, it would look like this

```yaml
version: "3.9"
services:
  redis:
    image: redis:alpine
    deploy:
      replicas: $REDIS_REPLICAS
  postgres:
    image: postgres:13-alpine
    environment:
      POSTGRES_PASSWORD: password
```

So, we are trying to run both postgres and redis

```bash
$ REDIS_REPLICAS=0 docker-compose up
Recreating docker-compose-demo_postgres_1 ... done
Attaching to docker-compose-demo_postgres_1
postgres_1  | The files belonging to this database system will be owned by user "postgres".
postgres_1  | This user must also own the server process.
postgres_1  |
postgres_1  | The database cluster will be initialized with locale "en_US.utf8".
postgres_1  | The default database encoding has accordingly been set to "UTF8".
postgres_1  | The default text search configuration will be set to "english".
postgres_1  |
postgres_1  | Data page checksums are disabled.
postgres_1  |
postgres_1  | fixing permissions on existing directory /var/lib/postgresql/data ... ok
postgres_1  | creating subdirectories ... ok
postgres_1  | selecting dynamic shared memory implementation ... posix
postgres_1  | selecting default max_connections ... 100
...
...
```

Let's try detached mode this time

```bash
$ REDIS_REPLICAS=0 docker-compose up -d
Recreating docker-compose-demo_postgres_1 ... done

$ REDIS_REPLICAS=0 docker-compose ps
             Name                           Command              State    Ports
---------------------------------------------------------------------------------
docker-compose-demo_postgres_1   docker-entrypoint.sh postgres   Up      5432/tcp
```

Let's try toggling on the redis service to run it

```bash
$ REDIS_REPLICAS=0 docker-compose down
Stopping docker-compose-demo_postgres_1 ... done
Removing docker-compose-demo_postgres_1 ... done
Removing network docker-compose-demo_default

$ REDIS_REPLICAS=1 docker-compose up -d
Creating network "docker-compose-demo_default" with the default driver
Creating docker-compose-demo_redis_1 ... done
Creating docker-compose-demo_postgres_1 ... done

$ REDIS_REPLICAS=1 docker-compose ps
             Name                           Command               State    Ports
----------------------------------------------------------------------------------
docker-compose-demo_postgres_1   docker-entrypoint.sh postgres    Up      5432/tcp
docker-compose-demo_redis_1      docker-entrypoint.sh redis ...   Up      6379/tcp
```

Interesting! It works ;)

There was also another thing we wanted to try. What if we need to use
`depends_on` feature based on the service that is being toggled on and off?
That's possible too :)

```yaml
version: "3.9"
services:
  redis:
    image: redis:alpine
    deploy:
      replicas: $REDIS_REPLICAS
  postgres:
    image: postgres:13-alpine
    environment:
      POSTGRES_PASSWORD: password
    depends_on:
      - redis
```

Cleaning up stuff first

```bash
$ REDIS_REPLICAS=1 docker-compose down
Stopping docker-compose-demo_postgres_1 ... done
Stopping docker-compose-demo_redis_1    ... done
Removing docker-compose-demo_postgres_1 ... done
Removing docker-compose-demo_redis_1    ... done
Removing network docker-compose-demo_default
```

Now bringing up postgres alone without redis. But postgres depends on redis
according to the spec file

```bash
$ REDIS_REPLICAS=0 docker-compose up -d
Creating network "docker-compose-demo_default" with the default driver
Creating docker-compose-demo_postgres_1 ... done

$ REDIS_REPLICAS=0 docker-compose ps
             Name                           Command              State    Ports
---------------------------------------------------------------------------------
docker-compose-demo_postgres_1   docker-entrypoint.sh postgres   Up      5432/tcp
```

All good! It still works!

Now let's cleanup

```bash
$ REDIS_REPLICAS=0 docker-compose down
Stopping docker-compose-demo_postgres_1 ... done
Removing docker-compose-demo_postgres_1 ... done
Removing network docker-compose-demo_default
```

Let's try to toggle on the redis and try things out!

```bash

$ REDIS_REPLICAS=1 docker-compose up -d
Creating network "docker-compose-demo_default" with the default driver
Creating docker-compose-demo_redis_1 ... done
Creating docker-compose-demo_postgres_1 ... done
$ REDIS_REPLICAS=1 docker-compose ps
             Name                           Command               State    Ports
----------------------------------------------------------------------------------
docker-compose-demo_postgres_1   docker-entrypoint.sh postgres    Up      5432/tcp
docker-compose-demo_redis_1      docker-entrypoint.sh redis ...   Up      6379/tcp
```

All works! Final cleanup

```
$ REDIS_REPLICAS=1 docker-compose down
Stopping docker-compose-demo_postgres_1 ... done
Stopping docker-compose-demo_redis_1    ... done
Removing docker-compose-demo_postgres_1 ... done
Removing docker-compose-demo_redis_1    ... done
Removing network docker-compose-demo_default
```

So, that's one way you can optionally run services in Docker Compose, with just
a single Docker Compose file.

If you have other ways, do mention in the comments :)
