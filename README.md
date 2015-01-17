# HAM
Ham is the amateur radio operator for your dokku apps.  It's both the missing Dokku remote client and a wrapper around common commands to simplify iterative development of dokku applications.

Save server and app configuration locally to take the pain out of `dokku` commands and (optionally) push work-in-progress code without needing to `git commit`.

The working directory `push` feature should be used with development dokku servers (i.e. running locally in vagrant) rather than out in the real world.

## Installation

Requires `git`, `ssh`, `bash`, and a server running [dokku](http://progrium.viewdocs.io/dokku/index).

```shell
> # Download the script
> git clone https://github.com/amcgee/ham
> # Add it to your path, i.e.
> echo "PATH=`pwd`/ham/bin:\$PATH" >> ~/.bash_profile
> # Profit
```

## Usage

```
USAGE:
  ham <command>

<command>:
  init <url> <app>          Create the specified application and save it to a
                              local .hamrc file
  check                     Check that the connected dokku application exists.
  push                      Push the current working directory to the connected
                              dokku application (doesn't affect local git repo)
  open                      Open the url of this app in a web browser.

  dokku <args...>           Connect to the remote dokku instance and run the
                              specified command verbatim.
  <cmd> <args...>           Connect to the remote dokku instance and run the
                              specified command, with $HAMAPP as the first arg.

Configuration is loaded in the following order:
  1. Command-line arguments (for 'init' and 'connect' only)
  2. Environment variables 'HAMURL' and 'HAMAPP'
  3. The local .hamrc file
  4. The global ~/.hamrc file
```

Here's a simple example (after setting up [dokku](http://progrium.viewdocs.io/dokku) in a local [vagrant](https://www.vagrantup.com/) box at dokku.me):

<iframe src="http://showterm.io/7b5f8d42ba021511e627e" width="640" height="480"></iframe>

```shell
> git clone https://github.com/heroku/node-js-sample
...
> cd node-js-sample

> # Configure ham to talk to a dokku server and use the 'sample' app
> # Create 'sample' if it doesn't already exist
> ham init dokku@dokku.me sample
...

> # Push the app (no need to set up a git remote)
> ham push
...

> # Test it out!
> # (those are backticks, not single quotes)
> curl `ham url`
Hello world!

> # Modify something locally but don't commit it in git
> sed -i '' 's/World/Dokku/g' index.js

> # Push the app again - the local changes will be pushed
> ham push
...

> # Check our work
> curl `ham url`
Hello dokku!

> # Easily read the application logs
> ham logs
...

> # Run a general dokku command on the remote server
> ham dokku apps
=== My Apps
sample

> # Over
```

[See it in action with showterm.io!](http://showterm.io/fa9984f1813bebf937c24#fast)

Please Enjoy responsibly.

## License and Contributions
[MIT](http://opensource.org/licenses/MIT) licensed, contributions welcome.  Use it, love it, improve it.
