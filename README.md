# Folder Watcher

Folder watcher is a simple Ruby script that can be set up to watch one or more folders (and all subfolders) on a Mac, and then run a shell command when something in there changes.

## Install

You don't need to know any Ruby to make use of this, and should be able to set this up with just a few shell commands. All Macs come with Ruby installed by default, so you don't even need to install it. Just download or clone the repository and follow these instructions.

This script uses `bundler` to manage external dependencies, so if you don't have it installed, get it with:

    gem install bundler

Then, inside the project directory, use this command to get all dependencies:

	bundle install

That's it. Depending on your system, you may need to add `sudo` before these. Try it without first.

## Configuration

Before you run the script, you need to tell it what folder(s) to watch, and what to do when a filesystem event is detected. All settings are contained in the `settings.yml` file. This provides the flexibility to watch multiple directories (with different actions for each directory), and keeps configuration away from code.

    test:
      path: /Users/Vickash/Desktop/test
      command: "osascript -e 'beep'"

This is a very straightforward [YAML](http://en.wikipedia.org/wiki/YAML) file, and should be easy to edit even if you're unfamiliar with the format. The three lines above constitute a single 'root-level' key. Each of these keys represents a different 'rule'; a directory to watch, and a command to execute when a change occurs in that directory.

The first line is just a name for the rule. You can use anything memorable, but each root-level key must have a unique name for the script to work properly. The second and third lines (indented by 2 spaces), are siblings, children of the parent, and correspond to the POSIX-style path being watched, and the shell command to run, respectively.

To add a new rule, copy-paste these 3 lines, rename the top level key to something unique, and adjust the path and command values to your needs. Keep in mind that, via the command value, you can call up different shells, shell scripts, another Ruby script, or even Applescript (as in my example), just as you would in a regular shell.

## Starting

Once you've put in your settings, open a shell to the project directory and type the following to start up the watcher:

    ruby folder_watcher.rb

This loads your settings, then starts and daemonizes the watcher process, freeing your shell. You'll need to restart this if you reboot.

## Stopping

From a shell in the project directory, type:

    kill -15 $(cat daemons.rb.pid)

You need to stop and restart the script each time you make a change to `settings.yml`.

## Notes

This script makes use of [rb-fsevent](https://github.com/thibaudgg/rb-fsevent/) for detecting filesystem events, and thus works on Macs only. For more info on customizing how the script responds to events (latency, deferring etc.), please see that project.

