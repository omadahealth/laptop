Laptop
======

Laptop is a script to set up an OS X laptop for web development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

Pre-installation steps
----------------------

This script assumes you have gone through the steps outlined in the
[engineering onboarding](https://wiki.omadahealth.net/doku.php?id=engineering:onboarding) wiki
and have set up your Github profile with your ssh keys before running.

Install
-------

Download, review, then execute the script:

```sh
curl -o ~/mac --remote-name https://raw.githubusercontent.com/omadahealth/laptop/master/mac
less ~/mac
sudo sh ~/mac 2>&1 | tee ~/laptop.log
```

Once the script is done, it's a good idea to quit and relaunch Terminal.

Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/omadahealth/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

What it sets up
---------------

Mac OS X tools:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Dotfiles:

* [Omada's Dotfiles] to standardize the dev experience between machines

[Omada's Dotfiles]: https://github.com/omadahealth/dotfiles

Unix tools:

* [Git] for version control
* [OpenSSL] for Transport Layer Security (TLS)
* [Coreutils] provides the GNU version of the stat command

[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[Coreutils]: http://www.gnu.org/software/coreutils/coreutils.html


Image tools:

* [ImageMagick] for cropping and resizing images

[ImageMagick]: http://www.imagemagick.org/script/index.php

Programming languages and configuration:

* [Bundler] for managing Ruby libraries
* [Node.js] and [NPM], for running apps and installing JavaScript packages
* [RVM] for managing versions of Ruby
* [Ruby] stable for writing general-purpose code

Databases:

* [Postgres] for storing relational data
* [Redis] for storing key-value data

[Postgres]: http://www.postgresql.org/
[Redis]: http://redis.io/

It should take less than 15 minutes to install (depends on your machine).

Customize your team's setup in the project specific files
------------------------------

There are project specific files in the `/projects` folder.

If you want to share setup commands between your whole team, add them in there and create a pull request to update them in the repo.

The team files are pulled in when running the laptop.local script.

You can declare which teams' files you'd like to include by updating this line:
`declare -a PROJECTS=("orange" "green" "blue" "red")`

When naming new team files, please keep to the style of `teamname.local`.

Customize in `~/.laptop.local`
------------------------------

Your ~/.laptop.local is run at the end of the mac script.
Put your personal customizations there.
This repo already contains a .laptop.local you can use to get started.
Either grab it from the repo or you can download it to your home directory using the command below.

```sh
curl -o ~/mac --remote-name https://raw.githubusercontent.com/omadahealth/laptop/master/.laptop.local
```

Optional tools currently in `laptop.local`
If you want to install these, uncomment them from the `laptop.local` file.

* [Qt] for headless JavaScript testing via Capybara Webkit
* [PhantomJS] for JavaScript testing
* [Watchman] watches files and records, or triggers actions, when they change
* [GPG] for management of PGP keys

[Qt]: http://qt-project.org/
[PhantomJS]: http://phantomjs.org/
[Watchman]: https://github.com/facebook/watchman
[GPG]: https://www.gnupg.org/

For example:

```sh
#!/bin/sh

brew_cask_install_or_upgrade "dockertoolbox"
brew_install_or_upgrade "go"
brew_install_or_upgrade "ngrok"
brew_install_or_upgrade "watch"

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the [wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.

Contributing
------------

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck].

```sh
brew install shellcheck
```

[ShellCheck]: http://www.shellcheck.net/about.html

License
-------

Laptop is © 2011-2016 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE
