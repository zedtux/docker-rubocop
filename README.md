# Rubocop in Docker for Atom

I have created this docker image in order to run [Rubocop](https://github.com/bbatsov/rubocop)
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
 docker run --rm -v $4:/app/.rubocop.yml -v /tmp:/tmp zedtux/rubocop $@
 ```
 6. Make it executable: `chmod +x /usr/local/bin/rubocop`

Now, in Atom, open a Ruby file and let the magic happening.

_Note: First time you'll run it, it will pull the image from the Internet which
could takes some time depending on your Internet connection_

### About this fork

This repository is a fork of the [BBC News specific config for Rubocop](https://github.com/BBC-News/rubocop-config).
Please have a look at their repository for more information.
