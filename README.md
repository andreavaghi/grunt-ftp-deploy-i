# grunt-ftp-deploy

[![NPM version][npm-image]][npm-url] [![Build Status][travis-image]][travis-url]

This is a grunt task for code deployment over the ftp protocol. It is mostly a copy of grunt-ftp-deploy, but add the possibility of setting the auth credentials from environment variables. And when I say "mostly a copy," I mean I stole it all and added this feature. Including this readme, for now.

This is a [grunt](https://github.com/gruntjs/grunt) task for code deployment over the _ftp_ protocol.

These days _git_ is not only our goto code management tool but in many cases our deployment tool as well. But there are many cases where _git_ is not really fit for deployment:

* we deploy to servers with only _ftp_ access
* the production code is a result of a build process producing files that we do not necessarily track with _git_

This is why a _grunt_ task like this would be very useful.

For simplicity purposes this task avoids deleting any files and it is not trying to do any size or time stamp comparison. It simply transfers all the files (and folder structure) from your dev / build location to a location on your server.

## Getting Started

This plugin requires Grunt `~0.4.0`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-ftp-deploy --save-dev
```

and load the task:

```javascript
grunt.loadNpmTasks('grunt-ftp-deploy');
```

## Usage

To use this task you will need to include the following configuration in your _grunt_ file:

```javascript
'ftp-deploy': {
  build: {
    auth: {
      host: 'server.com',
      port: 21,
      authKey: 'key1'
    },
    src: 'path/to/source/folder',
    dest: '/path/to/destination/folder',
    exclusions: ['path/to/source/folder/**/.DS_Store', 'path/to/source/folder/**/Thumbs.db', 'path/to/dist/tmp']
  }
}
```

Please note that when defining paths for sources, destinations, exclusions e.t.c they need to be defined having the root of the project as a reference point.

The parameters in our configuration are:

* **host** - the name or the IP address of the server we are deploying to
* **port** - the port that the _ftp_ service is running on
* **authPath** - an optional path to a file with credentials that defaults to `.ftppass` in the project folder if not provided
* **authKey** - a key for looking up credentials saved in a file (see next section). If no value is defined, the `host` parameter will be used
* **src** - the source location, the local folder that we are transferring to the server
* **dest** - the destination location, the folder on the server we are deploying to
* **exclusions** - an optional parameter allowing us to exclude files and folders by utilizing grunt's support for [minimatch](https://github.com/isaacs/minimatch). The `matchBase` minimatch option is enabled, so `.git*` would match the path `/foo/bar/.gitignore`.
* **forceVerbose** - if set to `true` forces the output verbosity.

## Authentication parameters

Usernames and passwords can be stored in an optional JSON file (`.ftppass` in the project folder or optionaly defined in`authPath`). The credentials file should have the following format:

```javascript
{
  "key1": {
    "username": "username1",
    "password": "password1"
  },
  "key2": {
    "username": "username2",
    "password": "password2"
  }
}
```

This way we can save as many username / password combinations as we want and look them up by the `authKey` value defined in the _grunt_ config file where the rest of the target parameters are defined.

The task prompts for credentials that are not found in the credentials file and it prompts for all credentials if a credentials file does not exist.

**IMPORTANT**: make sure that the credentials file uses double quotes (which is the proper _JSON_ syntax) instead of single quotes for the names of the keys and the string values.

## Dependencies

This task is built by taking advantage of the great work of Sergi Mansilla and his [jsftp](https://github.com/sergi/jsftp) _node.js_ module and suited for the **0.4.x** branch of _grunt_.

[npm-url]: https://npmjs.org/package/grunt-ftp-deploy-i
[npm-image]: https://img.shields.io/npm/v/grunt-ftp-deploy-i.svg
[travis-url]: http://travis-ci.org/andreavaghi/grunt-ftp-deploy-i
[travis-image]: https://img.shields.io/travis/andreavaghi/grunt-ftp-deploy-i/master.svg
