---
title: Elixir, Postgres, RabbitMQ Architecture
published: true
description: A look at system design utilizing elixir, phoenix, RabbitMQ, PostgreSQL, and other elixir/phoenix features
categories:
- software engineering
tags: 
- upskill 
- advice
- web development
- web developer
---
# Getting up and running
1. `sudo apt update`
2. `sudo apt upgrade`
3. `apt install curl git`
4.  add this to your ~/.bashrc 
```
. "$HOME/.asdf/asdf.sh"
. "$HOME/.asdf/completions/asdf.bash"
```
5. run `source ~/.bashrc`
6. `asdf plugin add erlang`
7. `asdf plugin add elixir`
8. `asdf install erlang 26.1.2`
9. `asdf install elixir 1.16.0-rc.0-otp-26`
10. `asdf local erlang 26.1.2`
11. `asdf local elixir 1.16.0-rc.0-otp-26`
** For setting up postgres I have added WSL2 instructions, https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-database. your system may be slightly different for installing postgres.
12. ` mix archive.install hex phx_new`
13. `mix phx.new my_golf --binary-id --no-html`
14. `cd my_golf`
15. `mix ecto.create`
16. `mix phx.server`

://TODO