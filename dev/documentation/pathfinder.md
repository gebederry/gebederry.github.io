## How to (re)create those artifacts?
### Preparation of Node modules for use in browsers

* currently only one Node module is used by the pathfinder code: geojson-path-finder
* the tool browserify is used to create a browser loadable file from Node modules
    * current form of use: turn all module exports into browser globals
* browserify bridges the gap between Node and browser; some issues are:
    * globals like “window”
    * npm used to install Node modules, i.e. dependencies
    * code dependencies are loaded with “require()”
* the current (set of) file(s) can be generated with the following setup
    * nvm
        * a tool to install and manage diffent Node versions
        * Node versions end up under user home directory
        * great side effect: even “global” Node module installs require no “root” any more
        * used version 0.35.3 at time of writing
        * very likely not required: just get Node this way or any other
    * Node
        * the JS environment for “servers” and anywhere else (without browser)
        * v12.16.2
        * was almost latest LTS at the time of writing
        * probably anything in that area works equally fine
    * npm
        * Node’s package manager
        * will be installed with Node (always, right?), but can be updated individually
        * at least nvm brings Node with npm
        * have version 6.14.4 here
    * package.json, package-lock.json
        * npm reads (and writes) these files
        * hard to tell for sure which command is right
            * over time npm changed what does what exactly
            * documentation mediocre... just like jquery... haha
        * package-lock.json
            * describes exact versions of packages
                * of whatever node package is installed...
                * ...unless something forbode npm to record it here
            * required to reinstall packages with same version
                * hard to do otherwise
                * especially as even a single package often requires a multitude of packages itself
            * package.json still required to work with package-lock.json at all
        * package.json
            * records requested packages and (usually) what version range satisfies the/this project
            * with the right npm command, npm will install
                * packages as registered in package.json
                * ...and their dependencies
                * exactly as recorded in package-lock.json, ...
                * ...unless those versions conflict with what package.json wants...
                * ...then it silently does something else(?)
    * with all tools prepared, there are these options to install the required packages
        * `npm ci`
            * supposed to be faster than `npm install` if done for the first time
            * otherwise slower than `npm install`, because it deletes all node_modules at start
            * complains if it can’t install package.json according to package-lock.json
        * `npm install`
            * sneakily changes package-lock.json if package.json can not be satisfied otherwise
                * I would like an option to complain at least
                * check back with git to see if npm did change it and consider investigating
    * with the packages installed...
        * `npx browserify --require geojson-path-finder --standalone GeoJSONPathFinder --outfile geojson-path-finder.js`
        * which means: loading geojson-path-finder.js in the browser ...
        * ... is like `global.GeoJSONPathFinder = require('geojson-path-finder')` in Node