Inboxen
=======

[![Build Status](https://travis-ci.org/Inboxen/Inboxen.svg?branch=master)](https://travis-ci.org/Inboxen/Inboxen)
[![Test coverage](http://codecov.io/github/Inboxen/Inboxen/coverage.svg?branch=master)](http://codecov.io/github/Inboxen/Inboxen?branch=master)

This is the complete system with everything you need to set up Inboxen. Please
use the "deploy" branch if you wish to use this in production - "master"
sometimes breaks!

Join us in our IRC channel! We're in the #inboxen channel on
[MegNet](https://www.megworld.co.uk/irc/)

See also: <https://etherpad.mozilla.org/inboxen>

Developing
----------

You'll need the following tools:

* Git
* Python (we strongly recommend you use virtualenv too)
* NodeJS
* Sass
* [EditorConfig](http://editorconfig.org/) *(optional)*

This project comes with a `.editorconfig` file - we recommend installing it to
avoid things like mixing tabs/spaces or accidently saving files with DOS-style
newlines.

Set yourself up with a virtual environment and run the following:

```
git clone https://github.com/Inboxen/Inboxen.git
cd Inboxen
pip install -r requirements-dev.txt
mkdir node_modules
npm update
```

When you've made your changes, remember to run `flake8` against Python files
you've changed (and `jshint` on JS files) and run unit tests. To run the tests,
do the following:

```
DB=sqlite python manage.py test --settings=inboxen.tests.settings
```

Committing and Branching
------------------------

### Branching

[Vincent Driessen's branching
model](http://nvie.com/posts/a-successful-git-branching-model/) best describes
how *should* do things.

There are some differences however:
* Our `master` is really Driessen's `develop`, we don't have an equivalent to
  Driessen's `master`
* `deploy` is our only release branch

Commits on `deploy` **must** be signed with a GPG key. This is important for
the future.

### Commit messages

You should follow the pattern of "summary, gap, details, gap, issue references"

For example:

```
Blah blah thing

Fixes this thing, changes how we should do something else

fix #345
touch #234
```

Deploying
---------

You'll need the following tools:

* Git
* Python (we strongly recommend you use virtualenv too)
* NodeJS
* Sass

Set yourself up with a virtual environment and run the following:

```
git clone -b deploy https://github.com/Inboxen/Inboxen.git
cd Inboxen
pip install -r requirements.txt
mkdir node_modules
npm update
```

After this has completed, see the next section on minimum configuration. Also,
`settings.py` is well commented and explains what various configuration options
do.

We tag our deployments (you should too) - we use signed annotated tags (`git
tag -as deplpy-YYYYMMDD`). The tag should contain a changelog since the last
deploy tag. This is particularly useful for rollbacks and keeps a record of
deployments that's separate from git history.

### settings.ini

At the very least, this file should contain the following:

```
[general]
secret_key = some_random_string
```

Where `some_random_string` is a long (at least a length of 50) string,
containing random characters.

### Webserver

The WSGI script can be found at `inboxen/wsgi.py`

There is also `inboxen/wsgi_admin.py` - this enables the admin interface on
`/admin`. It is **highly** recommended that you protect it from the outside
world.  Solutions such as a VPN are probably the easiest for your staff to use.

### Static Files

Static files are collected into `./static_content/`. You should configure your
webserver to point the URL `/static/` to this folder.

Remember to run `python manage.py collectstatic`!
