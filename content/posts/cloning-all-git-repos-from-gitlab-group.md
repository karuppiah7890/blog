---
title: "Cloning All Git Repos From GitLab Group"
date: 2020-04-19T07:35:13+05:30
---

## tldr;

```
$ curl -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
"https://<gitlab-url>/api/v4/groups/<group-id>/projects?per_page=100" | jq \
'map({ id: .id, name: .name, ssh_url: .ssh_url_to_repo, http_url: .http_url_to_repo })'

$ cat projects.json | jq ".[].http_url" -r | xargs -I {} git clone {}
$ # OR
$ cat projects.json | jq ".[].ssh_url" -r | xargs -I {} git clone {}
```

## Longer version

I have joined a new project. My use case was to clone all the repositories
in our team's GitLab group. Ideally I'm expecting a feature in
[gg](https://github.com/thecasualcoder/gg/) for this use case. Actually I
promised the authors, my friends, that I would help with support for GitLab
and such features 🙈 Anyways, I think I'll do that someday! :)

Back to the problem I had at hand. So, today morning I decided to use my
cool and simple automation skills to just clone away all the repositories
in our GitLab group. So, this is what I did - 

### Got my GitLab access token

You can check the docs on how to get it 
[here](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html). 
We need to choose scope of token based on the need - in this case, the need is
to use the API to list all the repositories or projects in a GitLab group. You
will need `read_api` for this, but that's only present in a very new version,
for old versions, like in my case, we need to use `api`, which has both read And
write access. Write access is not needed in our use case, so, play safe just
saying :P

### Listed and stored the repositories information I needed

Now I used the API and listed the repositories / projects like this using
[list group projects api](https://docs.gitlab.com/ee/api/groups.html#list-a-groups-projects)

```
$ curl -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
"https://<gitlab-url>/api/v4/groups/<group-id>/projects?per_page=100"
```

Make sure `GITLAB_TOKEN` has your personal access token stored in it. `gitlab-url`
is the GitLab instance URL - based on if it's a public instance or private
instance that you host yourself. `group-id` you can get by checking the web UI
of the GitLab Group's page whose projects you want to clone. GitLab shows the
ID saying `Group ID: abc`. Or else you will need to use
[list groups api](https://docs.gitlab.com/ee/api/groups.html#list-groups) and
find out the ID from the response with the name of the group.

Now that I got the project info, I'm going to just get the info I need, which
in this case is the url to the repo which I can use to clone. I'm also going
to simply get the id and name of the repo too. I'm using
[`jq`](https://stedolan.github.io/jq/) for this purpose like this

```
$ curl -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
"https://<gitlab-url>/api/v4/groups/<group-id>/projects?per_page=100" | jq \
'map({ id: .id, name: .name, ssh_url: .ssh_url_to_repo, http_url: .http_url_to_repo })'
```

I have got both the urls - ssh and http. This way you could use whatever you
want. Now, let's write this information into a JSON file!

```
$ curl -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
"https://<gitlab-url>/api/v4/groups/<group-id>/projects?per_page=100" | jq \
'map({ id: .id, name: .name, ssh_url: .ssh_url_to_repo, http_url: .http_url_to_repo })' > projects.json
```

Note: You can also use `jq` to filter out possible repos you might not need, for
example any repo with the word `test` in it. `jq` has exceptional processing
capabilities for processing JSON! :) In my case I wanted to clone all, so I
didn't do anything of that sort here

### Cloned the repostories

Now I just used some more automation to clone the repositories like this, since
I used HTTP URL to clone git repositories

```
$ cat projects.json | jq ".[].http_url" -r | xargs -I {} git clone {}
```

For SSH URL, you could do this

```
$ cat projects.json | jq ".[].ssh_url" -r | xargs -I {} git clone {}
```

Also, note that `git clone` will not clone to an existing non-empty directory,
so you don't have to worry about overwriting any existing files for that matter!

---

So that was what I did to clone all the repositories in our GitLab group to
my local machine! :) Have any questions? Shoot away! :D

