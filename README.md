# Node-Mod-Load

Lazy load modules and create consistent meta-modules

Working with a huge project means maintaining lots of moduls. Sometimes those modules form circular dependencies, which are not bad by default, but bring lots of problems with them (namely code which will not be executed).
In order to solve this problem, modules should be lazy-loaded, meaning they are only required when they are really needed in code.
That way even circular dependencies between modules will not be a problem as all code in a module can be read entirely.
This module will not solve circular calls. It will only allow circular dependencies and is no excuse for bad programming style and developer's errors!

In addition to regular modules, "meta-modules" can be added and used like every other module in the list of modules. Meta-modules are virtual modules, which are created during runtime.
They do not have a separate file for them and should just be a JS-Object which can be used like a module.

Node-Mod-Load can make use of JS Harmony's Proxy feature to only look up modules when they are needed instead of pre-fetching a file-list of available modules (without load them!).
Depending on the use-case, this might be more or less performant. You can disable this behavior by calling `enableLazyFetch(false)`.

This module was first created for SHPS, but then separated for easy use by everyone :)

### Version
1.0.0

### Installation
```sh
$ npm i node-mod-load
```

How To Use
----

You will first need to create a list of modules and meta-modules
```js
var libs = require('node-mod-load');

// Uncommenting the next line will disable lazy-fetching resulting in the module prefetching all available modules (without loading them)
// If the first parameter is true, lazy-fetch will be (re-)enabled!
libs.enableLazyFetch(false);

// This will add a single module or module-package to the list
// addPath will return a promise
libs.addPath('./some-module.js');
libs.addPath('./my-functionality');

// If you need addPath to be sync, just set the second parameter to true
libs.addPath('./somePackage', true);

// This will add all .js modules and all module-packages in the directory "./libs" to the list
// addDir used like this will return a Promise
libs.addDir('./libs');

// addDir can also be used as a sync function by adding a true as second parameter
libs.addDir('./modules', true);

// This will add a meta-module to the list
libs.addMeta('meta', { hellowWorld: () => { console.log('Hey there!'); } });

// The next uncommented line will make Node-Mod-Load flush the list of files, directories and meta-modules.
// Normally Node-Mod-Load would only work on that queue when a lib is requested for the first time if Harmony-Proxies are enabled
// If you do not enable Proxies or disable lazy-fetch, new entries will directly be worked on, so no flush required
libs.flush();
```

After building the list you can just require Node-Mod-Load anywhere and get the module from it
```js
var libs = require('node-mod-load').libs; // <- the object `libs` will include everything you added

var hellowFun = () => {
  libs.meta.hellowWorld();
};
```

Planned Features (TODO)
----

- remove-* methods
- handle modules with same name

Version History
----

License
----

MIT

**Free Software, Hell Yeah!**