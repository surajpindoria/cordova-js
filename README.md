A unified JavaScript layer for Callback projects.

# Project Structure

    callback-js
      |
      |-build/
      | Will contain any build modules (currently nothing here as it is all hacked
      | into the JakeFile)
      |
      |-lib
      |  |-bootstrap.js
      |  | Code to bootstrap the Callback platform, inject APIs and fire events
      |  |
      |  |-builder.js
      |  | Injects in our classes onto navigator (or wherever else is needed)
      |  |
      |  |-channel.js
      |  | A pub/sub implementation to handle custom framework events 
      |  |
      |  |-hijacks.js
      |  | We need to hijack `add/removeEventListener` - this is where
      |  | we do it.
      |  |
      |  |-phonegap.js
      |  | Common phonegap stuff such as callback handling. 
      |  | 
      |  |-utils.js
      |  | General purpose JS utility stuff: closures, uuids, object
      |  | cloning.
      |  |
      |  |-exec/
      |  | Will contain the platform specific definitaions of the exec method. 
      |  | Thinking of maybe renaming/repurposing this for any other platform
      |  | specific quirks.
      |  |
      |  |-platform/
      |  | Definitions of each platform that help us describe where
      |  | and what to put on the window object, and what to run to
      |  | initialize the platform. Each file is a JSON object with the
      |  | following properties:
      |  | 
      |  |  {
      |  |    id:"atari", // a string representing the platform id
      |  |    initialize:function(){}, // function to run any platform-specific initialization
      |  |    objects:{ // properties of this object define globals that get either injected onto `window` or mixed into existing `window` objects.
      |  |      PhoneGap:{
      |  |        path:"phonegap", // a requirejs-compatible path to the .js file to use for the object.
      |  |        children:{ // properties of this object are added to the parent object, i.e. in this example the defined children will be added to the `PhoneGap` global.
      |  |          path:"phonegap/somedir/other" // specify either a path to the .js file to use for the child
      |  |        }
      |  |      }
      |  |    }
      |  |  }
      |  |
      |  |-plugin/
      |  | All API definitions as plugins.

# Building

Just make sure you have `node`, `npm` and `jake` installed and run:

    jake

It will build into the `./pkg` folder.

# Testing

Launch `./test/index.html` to run the suite of tests for each module / plugin.

Coming soon, nodeJS running of tasks!

Final testing should always be done with https://github.com/callback/callback-test

# Integration

## Callback

Build the .js file and drop it in as a replacement for phonegap.js!

## Ripple

Load this in Ripple to play with it. You will have to use the phonegap
prototype branch to better simulate the phone environment and use this
javascript rather than Ripples emulated code.

    git clone git@github.com:blackberry-webworks/Ripple-UI.git
    git checkout winnie.the.pooh
    ./configure
    jake

and then load the upacked extension in chrome in the pkg/chromium folder.
Use the phonegap.proto platform in ripple.

# Adding a New Platform

FILL THIS OUT YO!

# TODO / Hacking / Contributing

- figure out the bootstrap
- docs: adding new platform, `require('path')` pathing transformation
  from `./build/packager.js`
- think more about what it means to run in node
- think about whether to select and load the platform specific modules at
  runtime or at buildtime.
- 3rd party plugins could be interesting. Need a little bit more thought about how these will fit into the system. I am thinking a package.json type file to handle per plugin.