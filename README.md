# Check, Check, It

use checklists, like a boss

CheckCheckIt is a ruby library that exposes the command line program `check` and companion web service that makes the process of going through a checklist easy to sync across multiple people.

Right now everything begins at the command line and a directory of checklists.

A "checklist" is just a text file.
Every line that starts with a dash '-' is a step.
Everything beneath a step is that step's body or description.

## Installation

    $ gem install checkcheckit

## Usage

    # list your checklists
    $ check list
    # Checklists
    personal
      groceries
    work
      deploy

    # it's all text
    $ cat ~/checkcheckit/personal/groceries.md
    - bacon
    - eggs
    - coffee
    - chicken apple sausage
    - avocados

    # start a list at the command line and keep it there
    $ check start groceries
    |.......| Step 1: bacon
    Check: <enter>

    |+......| Step 2: eggs
    Check: ^C
    Goodbye!

    # start a list, open it in your browser, and skip the CLI interaction
    $ check start groceries --live --web-only --open
    $ check start groceries --live --no-cli -O
    Live at URL: http://checkcheckit.herokuapp.com/4f24b9d933d5467ec913461b8da3f952dbe724cb

### `list` the checklists

`checkcheckit` assumes a home directory of ~/checkcheckit

In that directory are folders for your organizations, groups, etc.

In those folders are your checklists.

    $ check list
    # Checklists
    heroku
      todo
    personal
      todo
    vault
      deploy

### `start` a checklist

You can go through a checklist by running `check start ` and then the checklist name.

If there are multiple checklists with the same name use the format `folder/checklist`.

When you iterate through a checklist you can just type "enter", "y", or "+" to confirm a step and "no" or "-" to
fail one.

You can use the `--notes` flag to enter optional notes.

For example:

    $ check start deploy --notes
    |.......| Step 1: Pull everything from git
      > git pull origin
    Check: <enter>
    Notes: <enter>

    |+......| Step 2: Make sure there are no uncommitted changes
      > `git status`
    Check: <n>
    Notes: <enter>

    |+-.....| Step 3: Diff master with heroku/master
      Make sure the change you want to push are what you're pushing
      > git fetch heroku
      > git diff heroku/master | $EDITOR
    Check: <y>
    Notes: <enter>

    |+-+....| Step 4: Run the test suite
    Check: failures!
    Notes: <enter>

### Live mode

This is fun.

`check start <listname> --live` will create an _interactive_ companion URL on the web.

This URL is websockets-enabled and communicates with the command line.
This means command line 'checks' get pushed to the web.  Once a list is on the web you can
disconnect the command line and continue finishing it (with others).

    $ check start deploy --live
    Live at URL: http://checkcheckit.herokuapp.com/4f24b9d933d5467ec913461b8da3f952dbe724cb
    Websocket refused connection - using POST
    |........| Step 1: Make sure there are no uncommitted changes
      > `git status`
    Check:

    |+.......| Step 2: Pull everything from git
      > `git pull`
    Check: ^C
    bye

During that console session the web UI would be interactively crossing items off the list:
<img height="400px" src="http://f.cl.ly/items/1h3V0L1a1p1a062I2X3f/Screen%20Shot%202012-12-16%20at%209.37.56%20PM.png" />

### Email
Specify an email (or a comma-separated list) on the command line via the `--email` flag and
the address(es) will receive an email with a link to a web version of the checklist.


    $ check start deploy --email bob@work.com,steve@work.com
    Live at URL: http://checkcheckit.herokuapp.com/4f24b9d933d5467ec913461b8da3f952dbe724cb
    Websocket refused connection - using POST
    |........| Step 1: Make sure there are no uncommitted changes
      > `git status`
    Check: ^C
    bye

## TODO

- resume a run locally from URL
- push notes to web
- emit pass/fail and colorize
- post to campfire

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
