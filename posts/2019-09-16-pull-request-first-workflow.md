---
title: Improve Your Code Review
permalink: improve-your-code-review
date: 2019-09-16 17:18
---

> Pending code reviews represent blocked threads of execution.

## The Importance of Reviewing Pull Requests

Being locked by code review, an engineer can't make progress at all, or can only make small progress at the cost of frequent context switching and increased memory allocation.

## Criteria for Being a Good Reviewer

1. Put code review at high priority.
2. Shorten your event-loop, put time to review pull request whenever possible.
3. Don't put code review time all together. Split to short periods of time is more effective.
4. Be thorough, each and every time.
5. Respond to comments timely.

## Criteria for Being a Good Submitter

1. Make sure the code error free (code should compile) and warning free (including Swiftlint warnings).
2. Make sure you've done your cleanup (e.g. naming, file structure, comments, temporary prints/variables).
3. Review the PR yourself before submitting.
4. Considering including screenshots/gif if necessary.
5. Respond to comments timely.

Reference: 
1. http://khanlou.com/2019/07/continuous-integration/
2. https://medium.com/@9len/on-code-review-16ea85f7c585

