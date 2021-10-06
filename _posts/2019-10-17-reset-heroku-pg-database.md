---
layout: post
title: How to Reset Heroku Postgres Database and Why It Keep Fails
description: Dropping database production is not your usual practice.
categories: heroku rails
image: 3
last_modified_at: 2021-10-07
---

Dropping database production is not your usual practice. Surprisingly, it is not straightforward for it's for reason. Learn the hard way.

## Too Long, Didn't Read

Run this one-liner command.

```bash
heroku restart && heroku pg:reset DATABASE && heroku run rake db:migrate
```

Or

- Step 1: `heroku restart` (make sure it has no process run)
- Step 2: `heroku pg:reset DATABASE` (no need to change the `DATABASE`)
- Step 3: `heroku run rake db:migrate`
- Step 4: `heroku run rake db:seed` (if you have seed)

***

## Too Long, Read Anyway

I have Rails app, so it should be as simple as...

```bash
cd my_rails_app && heroku pg:reset DATABASE
```

```bash
rake db:reset
```

But, the reality is a little harder.

I have a Rails 4 app, using Postgres 9.4, Heroku Cedar stack. There are two databases: one is the production database, another one is for staging. Generally, this is a good practice to have a staging environment that is equivalent to production. So that I should have a staging database on Heroku.

When pushing to staging, I want to reset the database to ensure it's exactly the same as production. So I run the command above. It should be like:

```bash

heroku pg:reset DATABASE

```

But it just not works.

It certainly works locally. It is not in local development. It is live in the production environment, even only the staging server.

### The story behind of reset

I have a staging server (running in a production environment) and it was simply deployed in Heroku. No reason behind it. It was quite easy to deploy and I also Heroku provide almost-production-running-like app without hassle with as simple as command `heroku create`

Heroku is chosen because Heroku simply provides an almost-production-running-like app without hassle. One day I need to reset the database because Heroku only provides 10.000 free rows.

### Rails that prevents data wipeout.

When I run `heroku run rake db:drop` on heroku CLI, It will given this kind error messages.

```
ActiveRecord::ProtectedEnvironmentError: You are attempting to run a destructive action against your 'production' database.
If you are sure you want to continue, run the same command with the environment variable:
DISABLE_DATABASE_ENVIRONMENT_CHECK=1
```

Okay, I get it. See [Rails commits about prevent destructive action on production databaset][3], it's handy feature :

> Preventing destructive action on production database.

Let's run `heroku run rake db:drop DISABLE_DATABASE_ENVIRONMENT_CHECK=1` and try my luck.

And what happens is another errors.

```
Database 'des74ei48mf9s6' does not exist
```

Not sure for this one.

### Make sure it has no process run.

```
heroku restart
```

### Heroku apps do not have permission to drop and create databases.

It seems like not all Rake features are supported on Heroku. Heroku apps do not have permission to drop and create databases.

Heroku doesn't allow users from using rake db:reset, rake db:drop and rake db:create command. They only allow heroku pg:reset and rake db:migrate commands.

The following is a list of known limitations see: [Heroku Limitations][2]

## Summary

Dropping the production database is not your usual performance. Despite the non-straightforward step, it is made to ensure and double-check your action.

Are you sure you really want to destroy the production database?

Thanks for reading.

***

###### Reference

- [Gist reset Heroku Postgres database][1]
- [Heroku Limitations when running rake commands][2]
- [Rails commits about prevent destructive action on production database][3]

[1]: https://go.gizipp.com/https://gist.github.com/zulhfreelancer/ea140d8ef9292fa9165e  "Gist"
[2]: https://go.gizipp.com/https://devcenter.heroku.com/articles/rake#limitations       "Heroku"
[3]: https://go.gizipp.com/https://github.com/rails/rails/pull/22967/commits            "Rails"
