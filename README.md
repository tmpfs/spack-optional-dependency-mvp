# CommonJS Optional Dependency

This repository reproduces an issue with attempting to dynamically `require()` an optional dependency that cannot be installed on Linux.

To test this you must be running a Linux distro so that the `fsevents` module fails to install.

Then run `npx spack` and `spack` yields this error:

```
(node:13558) UnhandledPromiseRejectionWarning: Error: load_transformed failed

Caused by:
    0: failed to analyze module
    1: failed to resolve fsevents from node_modules/chokidar/lib/fsevents-handler.js
    2: not found
(Use `node --trace-warnings ...` to show where the warning was created)
(node:13558) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). To terminate the node process on unhandled promise rejection, use the CLI flag `--unhandled-rejections=strict` (see https://nodejs.org/api/cli.html#cli_unhandled_rejections_mode). (rejection id: 1)
(node:13558) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
```

---

The [code that generates this problem](https://github.com/paulmillr/chokidar/blob/master/lib/fsevents-handler.js#L7-L12) is:

```javascript
let fsevents;
try {
  fsevents = require('fsevents');
} catch (error) {
  if (process.env.CHOKIDAR_PRINT_FSEVENTS_REQUIRE_ERROR) console.error(error);
}
```

