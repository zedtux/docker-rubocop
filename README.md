# Rubocop in Docker for Atom

I have created this [docker image](https://hub.docker.com/r/zedtux/docker-rubocop/) in order to run [Rubocop](https://github.com/bbatsov/rubocop)
from the [Atom](https://atom.io/) IDE.

## Installation

Follow the following steps in order to get rubocop running in Atom using Docker:

 1. Install Docker
 2. Install Atom
 3. Install the `linter` and `linter-rubocop` atom packages
 4. Open the `linter-rubocop` settings and in 'Additional Arguments' put `-c .rubocop.yml`
 5. Create the following file `/usr/local/bin/rubocop`:

 ```
 #!/bin/bash
 
 # Remove invalid arguments send by linter-rubocop
 CMD_ARGS=""
 for arg in $@
 do
   # -s stands for --stdin which prevent rubocop from working
   [[ "$arg" == "-s" ]] && continue
 
   CMD_ARGS="$CMD_ARGS $arg"
 done
 
 docker run --rm -v /home:/home/ --workdir / zedtux/docker-rubocop:0.34.2 $CMD_ARGS
 ```
 6. Make it executable: `chmod +x /usr/local/bin/rubocop`

Now, in Atom, open a Ruby file and let the magic happening.

_Note: First time you'll run it, it will pull the image from the Internet which
could takes some time depending on your Internet connection_

_Note 2: You can find all the available tags [here](https://hub.docker.com/r/zedtux/docker-rubocop/tags/)._

## Upgrade Rubocop

The goal is to not install things on your machine, so how to upgrade rubocop without Ruby installed on your machine ?

```
$ docker run --rm \
             -v `pwd`:/app \
             -w /app \
             ruby \
             sh -c 'gem install bundler && bundle update'
 
```

This command will mount your current folder in a `ruby` container, set the working directory (flag `-w`) to that folder in the container and finally execute (`sh -c`) the installation of Bundler and then run the command `bundle update`.

The `Gemfile.lock` is updated, you can commit it and build a new image of docker-rubocop.

### About this fork

This repository is a fork of the [BBC News specific config for Rubocop](https://github.com/BBC-News/rubocop-config).
Please have a look at their repository for more information.
