---
layout: page
title: CI/CD Cheatsheets
permalink: /cheatsheets/ci-cd
date: 2021-08-04
last_modified_at: 2021-08-04
---

## Bitbucket Pipeline Rails

```yaml
image: ruby:3.0.1

pipelines:
  default:
    - step:
        name: Build
        services:
        - postgres
        script:
          - export DATABASE_URL=postgresql://test_user:test_user_password@localhost/pipelines
          - bundle install
          - bundle exec rake db:setup
          - bundle exec rake db:test:prepare
    - parallel:
      - step:
          name: Rspec
          script:
            - bundle exec rspec .
      - step:
          name: Lint Code
          script:
            - bundle exec rubocop
      - step:
          name: Rails Best Practices
          script:
            - bundle exec rails_best_practices -c config/rails_best_practices.yml

definitions:
  services:
    postgres:
      image: postgres
      environment:
        POSTGRES_DB: pipelines
        POSTGRES_USER: test_user
        POSTGRES_PASSWORD: test_user_password

```
