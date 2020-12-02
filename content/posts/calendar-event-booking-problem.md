---
title: "Calendar Event Booking Problem"
date: 2020-12-02T07:46:52+05:30
draft: true
---

This is a coding problem that has been inspired from the Google Calendar System.
It has a feature to show suggested times for the calendar event you are trying
to book

![gcal-suggested-times-feature](/blog/img/calendar-event-booking-problem/gcal-suggested-times-feature.png "gcal-suggested-times-feature")

Let me explain the problem first and then we can go into the solution

## Problem

Given the calendar data of N people, where the data tells you if a person is
busy at a particular time slot, find time slot for a meeting for a given
duration and within the work hour timings. The meeting happens within a day, so the duration of the meeting is less than 24 hrs.

Input - We get the data for a day for the N people, meeting duration, work hour
timings

Output - Find time slot for the meeting

If no time slot can be found for the meeting, which can accommodate everyone's
calendar, give that as result.

Extension to the problem - find a time slot where we can accommodate enough
number of people if not all. The minimum number of people needs to be more than
max(2, N/2) - which means that, at least half the people should be able to
attend

## Input, Output

The problem does not go into the specifics of how the input exactly looks like
and how the output should look like. This is a deliberate thing. But let's just
take a stab at how we can have it - though sometimes it might be based on how
we want to implement it and what we can implement. Let's go with an assumption
for now

It can look something like this

```bash
Meeting duration (in minutes): 45

Working Hours
Start: 09:00
End: 18:30

How many people are going to attend this meeting? 2

Provide the busy time slots of the people in 24hr time format

Person Name: Karuppiah
Slot 1 start: 09:00
Slot 1 end: 09:30
More? [y/n] n

Person Name: Alex
Slot 1 start: 09:30
Slot 1 end: 10:00
More? [y/n] n

Free time slots available for meeting are:
* 10:00 to 18:30
```

In the above sample execution, we see that we get the input from the user in a
particular format. We get times in particular format, like minutes for duration
and 24 hr timing for slots.

We are not asking to give total number of busy time slots. One benefit to this
is, the user doesn't have to count or remember the count of the busy time slots, they can just say what the busy time slots are.

The person name is just to avoid saying "person 1", "person 2" etc while asking
input and have some personalization to it. But according to the problem, we
don't have any use for names or person index (1,2 etc) until we go into the
extension where we need to see who all can attend the meeting for the given
time slots.

For the final output, we show the free time slots available for the meeting and
the slot can be bigger than the meeting itself. We can also suggest timings like
Google, by finding slots within that big slot for the meeting duration but there
can be many slots actually, for example between 10:00 and 18:30 for a 45 minutes
meeting. But if we want, we can do a simple suggestion(s) apart from telling
what is/are the free time slot(s) available, like

```
Free time slots available for meeting are:
* 10:00 to 18:30

Suggested meeting times:
* 10:00 to 10:45
* 10:45 to 11:30
...
```

For now, let's go with the simple output to start with and then add more
features later. Let's get into the solution mode now. Also, the above is a very
simple example with two people and just one busy time slot for each. In reality,
there can be a lot of people with lot of busy time slots.

## Assumptions

We assume that the input given by the user is all good and show output based on
that.

We assume that everyone attending the meeting are in the same timezone, though in a real world scenario this is not necessarily always true. Due to this
assumption, all input related to timings will not have timezone factor to it
and no date factor to it too, and will be considered as all happening in the
same day and same timezone for all. This assumption is to keep our input and
processing simple, as the problem statement does not talk much about this part
of the problem. But it's a good extension to have for the problem :)

## Solution

Let's start with solving the problem for just 2 people, instead of N. This is to
just simplify the problem to solve it iteratively.

Now, think that you are solving the problem manually. What would you do? You
have the calendar data of 2 people. You know the time slots of when they are
busy, within a given day, and also their work hour timings - so they can't have
meetings outside that time, because they have a life to live :)

Let's visualize like how Google Calendar would show us the Calendar schedule
(date) of two people

![karuppiah-and-alex-calendars](/blog/img/calendar-event-booking-problem/karuppiah-and-alex-calendars.png "karuppiah-and-alex-calendars")

As you can see, I'm a very a busy person compared to Alex :P :P

Now, how would you find a slot for a meeting? Let's say the meeting is for an
hour. Let's assume our work hours is 09:00 to 16:00

![karuppiah-and-alex-calendars-common-free-time-slot](/blog/img/calendar-event-booking-problem/karuppiah-and-alex-calendars-common-free-time-slot.png "karuppiah-and-alex-calendars-common-free-time-slot")

You would look for those gaps, right? Gaps to put in your extra meeting. These
gaps are the key to our problem.

If you think about it, this gap has some features to it. This gap should be
available for all the people in the meeting. Let's start with 2, so for the
two people in the meeting.

When we say available, what we mean is - the gap should Not be a busy time slot.
So, this gap is the inverse of the busy time slot - a free or non-busy time
slot, where the person is free to take up meetings, or possibly free as we just
have the calendar data, but don't know if the person is really available 😅
Anyways.
