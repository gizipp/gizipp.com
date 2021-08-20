---
layout: page
title: Rbenv Cheatsheets
permalink: /cheatsheets/rbenv
date: 2021-08-20
last_modified_at: 2021-08-20
---

### Install spesific version

```sh
rbenv install 3.0.2
```

### Set version

```sh
rbenv global 3.0.2
rbenv local 3.0.2
```

### Verbose install

```sh
rbenv install --verbose 3.0.2
```

### Show list you could install

```sh
rbenv install --list
```

### Pull if ruby version not exist (using ruby build)

```sh
git -C "$(rbenv root)"/plugins/ruby-build pull
```

Can't pull? Install ruby build first https://github.com/rbenv/ruby-build


