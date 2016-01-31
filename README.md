Async hook manager built on top of the Q promise library.

Currently only CommonJS support.

## Basic usage

`npm install async-hook-manager`

```javascript

    // We're using Q to return promises to the hook manager.
    var Q = require('q');
    
    // Load up AHM.
    var AHM = require('async-hook-manager');

    // Create a new instance of AHM.
    var myHookManager = AHM();

    // Register an async hook.
    // The return value is the ID of the hook, which can later be used to
    // unregister the hook.
    var asyncHookId = myHookManager.registerHook(function () {

      // Create a new promise.
      var hookPromise = Q.defer();
      
      // Do some aync stuff...
      example.fireTransitionalAnimation(function () {
        
        // On animation complete.
        hookPromise.resolve();
      });

      return hookPromise.promise;
    });

    // Register a sync hook.
    var syncHookId = myHookManager.registerHook(function () {

      // Do something synchronously here...
      example.updateSomeDataSynchronously();

      return true;
    });

    // Make a call via the hook manager instance.
    // This will call all of the registered hooks, and wait for all to respond
    // before firing its callback.
    myHookManager.makeCall().then(function () {

      // Do something that has to wait for all of the hooks to return.
      example.updateTheCurrentRoute();

    });

    // Unregister a hook that is no longer required.
    myHookManager.unregisterHook(syncHookId);

```

## API instance Methods

### Register a new hook
`.registerHook(hookCallback[, reference])`

### Unregister a hook
`.unregisterHook(hookId)`

### Unregister all hooks
'.unregisterAllHooks()'

### Get all currently registered hooks
`.getAllRegisteredHooks()`

### Make a call
`.makeCall([options])`

### Get the ID of the last call.
`.getLastCallId()`
