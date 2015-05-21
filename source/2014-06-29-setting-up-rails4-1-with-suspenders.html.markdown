---
title: Setting up Rails 4.1 with Suspenders
date: 2014-06-29
tags: postgres sql, rbenv, ruby on rails, suspenders
---

Its dead simple to do. Just follow the instructions at Thoughtbot's Suspenders page:

https://github.com/thoughtbot/suspenders
You will need to be running Ruby 2.1.2 (as of this writing) so go through that upgrade process. While you are at it, install rbenv and ruby-build through their git clone methods. You will thank me later. By the way, if you need the list of Rubies you can install the command is:

`rbenv install -l`

We include a github repo and heroku setup with almost every project we work on. We also give our github project the same name as the app. So the combined suspenders line for us looks like:

`suspenders <app>  --heroku true --github <organization>/<app>`

Once we got through the install process we ran into two issue before we could create the db and really get going.

RuntimeError: Missing `secret_key_base` for 'development' environment, set this value in `config/secrets.yml`
PG::ConnectionBad: fe_sendauth: no password supplied
For the first issue, we added a nonsense line into 'config/secrets.yml'. This fixed it straight away.

development:
secret_key_base: 'blah'
For the second problem we just had to add our Postgres username and password into database.yml.

Once we got those set we were off and running with Suspenders.
