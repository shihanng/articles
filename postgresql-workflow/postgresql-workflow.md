---
title: Improve your PostgreSQL Workflows with vim-tmux-runner
published: false
description: This article contains a small demo showing how to use Vim to execute SQL queries.
tags: productivity, tutorial, vim, tmux
---

There are many client applications for working with databases (PostgreSQL, etc.) available in the market.  Some of my colleagues use [JetBrains's DataGrip](https://www.jetbrains.com/datagrip/), [Postico](https://eggerapps.at/postico/), [DBeaver](https://dbeaver.io/), etc. While I do not work much directly with DBs on daily basis, I do sometimes find myself experimenting on local PostgreSQL instance via [`pgcli`](https://www.pgcli.com/) before "transferring" the queries into application code. During this process, one feature that I would really like to have from those GUI clients is the ability
execute selected/highlighted commands directly from the editor and get the result directly on screen. I use Vim as my main editor. So it would be nice if I can perform the same thing describe above
within Vim.

It turns out this can be achieve with combination of Vim and [tmux](https://github.com/tmux/tmux) and the **vim-tmux-runner** plugin. I've included a small demo in this article to show how that can be done with the tools mentioned before.

{% github christoomey/vim-tmux-runner %}

### DB Preparation

For demo purposes we are going to setup a sample PostgreSQL database in a Docker container. If you already have a DB to play with, feel free to skip this part.

```sh
docker run -d --name postgres_demo -p 5432:5432 --rm postgres:11.6
echo "CREATE DATABASE pagila;" | psql -h localhost -p 5432 -U postgres
curl -s https://raw.githubusercontent.com/devrimgunduz/pagila/68b7bab066a18988694a8b698533c3c507f7b133/pagila-schema.sql | psql -h localhost -p 5432 -U postgres pagila -f -
curl -s https://raw.githubusercontent.com/devrimgunduz/pagila/68b7bab066a18988694a8b698533c3c507f7b133/pagila-data.sql | psql -h localhost -p 5432 -U postgres pagila -f -
```

### Vim configurations

First install the `christoomey/vim-tmux-runner` plugin with your favorite Vim plugin manager. Then, for my setup, I've included the following in my vimrc:

```vim
let g:VtrUseVtrMaps = 1
let g:VtrStripLeadingWhitespace = 1
let g:VtrClearEmptyLines = 1
```

### DEMO

1. This workflow requires a **tmux** session, so go ahead and start up one.
2. Then we launch **Vim** within that tmux session.
3. In **Vim**, create a new tmux pane (referred as "runner" in the plugin) using the `:VtrOpenRunner` command.
4. Run **pgcli** in the newly created pane and connect to the DB (`pagila`) created above.
    ```sh
    pgcli -h localhost -p 5432 -u postgres pagila
    ```
5. Turn on [**Multi-line Mode**](https://www.pgcli.com/multi-line) in **pgcli** by pressing the `<F3>` key. This means that our PostgreSQL commands in Vim need to end with semi-colon `;`.

{% asciinema 293390 %}

Once we have **pgcli** running on the runner pane, we can write down our SQL query in Vim then select the query in [Visual mode](http://vimdoc.sourceforge.net/htmldoc/visual.html).

```sql
SELECT film_id, title FROM film LIMIT 5;
```

Finally use the binding `<Leader>sl` to send the selected SQL commands to the runner. The binding (enable via `g:VtrUseVtrMaps` mentioned above) executes command `:VtrSendLinesToRunner`.

{% asciinema 293801 %}

Often, we might be in a state where we already have **pgcli** opened but not attached to Vim as the runner. In this situation, if we do `:VtrSendLinesToRunner`, we will get `VTR: No runner pane attached.` error. For this case, we can use `:VtrAttachToPane` command to attach the pane as runner then continue with our workflow.

{% asciinema 293803 %}

This concludes the article about me indulging myself with another "How to do X in Vim?" Of course, usage of [**vim-tmux-runner**](https://github.com/christoomey/vim-tmux-runner) is not limited to PostgreSQL but you can do the same on other REPL shell e.g. [IRB](https://en.wikipedia.org/wiki/Interactive_Ruby_Shell), [IPython](https://ipython.org/index.html), etc. Hope this article gives you some ideas of how to integrate this plugin into your workflow. Thanks for reading :heart:.

### Cleanup :wastebasket:

If you want to clean up the demo DB created above, simply do:

```sh
docker stop postgres_demo
```
