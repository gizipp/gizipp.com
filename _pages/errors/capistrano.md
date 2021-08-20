---
layout: page
title: Capistrano Errors
permalink: /errors/capistrano/
date: 2021-08-20
last_modified_at: 2021-08-20
---

### Capistrano can't deploy errors : /usr/bin/rbenv/bin/rbenv: Not a directory

```sh
cap aborted!
SSHKit::Runner::ExecuteError: Exception while executing on host 34.135.248.213: Exception while executing on host 34.135.248.213: bundle exit status: 126
bundle stdout: bash: /usr/bin/rbenv/bin/rbenv: Not a directory
bundle stderr: Nothing written


Caused by:
SSHKit::Runner::ExecuteError: Exception while executing on host 34.135.248.213: bundle exit status: 126
bundle stdout: bash: /usr/bin/rbenv/bin/rbenv: Not a directory
bundle stderr: Nothing written


Caused by:
SSHKit::Command::Failed: bundle exit status: 126
bundle stdout: bash: /usr/bin/rbenv/bin/rbenv: Not a directory
bundle stderr: Nothing written

Tasks: TOP => deploy:initial
(See full trace by running task with --trace)
The deploy has failed with an error: Exception while executing on host 34.135.248.213: Exception while executing on host 34.135.248.213: bundle exit status: 126
bundle stdout: bash: /usr/bin/rbenv/bin/rbenv: Not a directory
bundle stderr: Nothing written
```

- On me `/usr/bin/rbenv/bin/rbenv` seems fishy 😅
- Go to server and check your `rbenv` path by `which rbenv` on me is `/usr/bin/rbenv`
- Add to your `deploy.rb` e.g like this

```sh
set :rbenv_custom_path, "/usr/bin/rbenv"
```

### Capistrono cap aborted! NotImplementedError: OpenSSH keys only supported if ED25519 is available

```
cap aborted!
NotImplementedError: OpenSSH keys only supported if ED25519 is available
net-ssh requires the following gems for ed25519 support:
 * ed25519 (>= 1.2, < 2.0)
 * bcrypt_pbkdf (>= 1.0, < 2.0)
See https://github.com/net-ssh/net-ssh/issues/565 for more information
Gem::MissingSpecError : "Could not find 'ed25519' (~> 1.2) among 589 total gem(s)
Checked in 'GEM_PATH=/Users/gizipp/.gem/ruby/3.0.0:/Users/gizipp/.rbenv/versions/3.0.1/lib/ruby/gems/3.0.0' , execute `gem env` for more information"

Tasks: TOP => deploy:check => git:check => git:wrapper
(See full trace by running task with --trace)
The deploy has failed with an error: OpenSSH keys only supported if ED25519 is available
net-ssh requires the following gems for ed25519 support:
 * ed25519 (>= 1.2, < 2.0)
 * bcrypt_pbkdf (>= 1.0, < 2.0)
See https://github.com/net-ssh/net-ssh/issues/565 for more information
Gem::MissingSpecError : "Could not find 'ed25519' (~> 1.2) among 589 total gem(s)
Checked in 'GEM_PATH=/Users/gizipp/.gem/ruby/3.0.0:/Users/gizipp/.rbenv/versions/3.0.1/lib/ruby/gems/3.0.0' , execute `gem env` for more information"
```

- Of course need to ssh-add [your key] again but [link](https://stackoverflow.com/a/58835804/6469234) somehow it is the private key(?)
- Remove pub
```
set :ssh_options, { ... , keys: %w(~/.ssh/id_rsa.pub) } # from with .pub
set :ssh_options, { ... , keys: %w(~/.ssh/id_rsa) } # to without .pub
```
- Add ssh key again the private NOT the pub `ssh-add ~/.ssh/id_rsa` without .pub
