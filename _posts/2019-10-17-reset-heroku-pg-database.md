---
layout: post
title: How to Reset Heroku Postgres Database and Why It Keep Fails
description: Dropping database production is not your usual practice.
categories: heroku rails
image: 3
last_modified_at: 2020-06-04T00:00:00+00:00
---

Dropping database production is not your usual practice. Surprisingly, it is not straightforward for it's for reason.

## Too Long, Didn't Read

Run this one-liner command.

```bash
heroku restart && heroku pg:reset DATABASE && heroku run rake db:migrate
```

Or

- Step 1: `heroku restart`
- Step 2: `heroku pg:reset DATABASE` (no need to change the `DATABASE`)
- Step 3: `heroku run rake db:migrate`
- Step 4: `heroku run rake db:seed` (if you have seed)

***

## Too Long, Read Anyway

I have Rails app, so it should be as simple as...

```bash
rake db:reset
```

But it just not works.

It certainly works in local. It not in local development. It is live on production environment, even only staging server.

### The story behind of reset

I have staging server (running in production environment) and it was simply deployed in Heroku. No reason behind it. It was quite easy to deploy and I also Heroku provide almost-production-running-like app without hassle with as simple as command `heroku create`

Heroku is chosen because Heroku simply provide almost-production-running-like app without hassle. One day I need to reset the database because Heroku only provide 10.000 free rows.

### Rails that prevents data wipeout.

Handy feature : Preventing destructive action on production database. See https://github.com/rails/rails/pull/22967/commits


When I run `heroku run rake db:drop` on heroku CLI, It will given this kind error messages.

`ActiveRecord::ProtectedEnvironmentError: You are attempting to run a destructive action against your 'production' database.
If you are sure you want to continue, run the same command with the environment variable:
DISABLE_DATABASE_ENVIRONMENT_CHECK=1`

Okay, I get it. Let's run `heroku run rake db:drop DISABLE_DATABASE_ENVIRONMENT_CHECK=1` and try my luck. And what happens is another errors.

`Database 'des74ei48mf9s6' does not exist`


It seems like not all Rake features are supported on Heroku. Heroku apps do not have permission to drop and create databases. It is


Heroku doesn't allow users from using rake db:reset, rake db:drop and rake db:create command. They only allow heroku pg:reset and rake db:migrate commands.


The following is a list of known limitations: https://devcenter.heroku.com/articles/rake


### Heroku apps do not have permission to drop and create databases.


### Make sure it has no process run.

```heroku restart```

## Summary

Dropping production database is not your usual perform. Despite of non-straightforward step.
