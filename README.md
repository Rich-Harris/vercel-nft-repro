# @vercel/nft node built-ins repro

Running `nodeFileTrace` on an app that imports Node built-ins with the `node:` prefix (which is the preferred way to import built-ins as of Node 18) causes a warning to be emitted:

```js
import fs from 'node:fs';
```

```
warnings: Set(1) {
  Error: Failed to resolve dependency node:fs:
  Cannot find module 'node:fs' loaded from [dir]/src/index.js
      at [dir]/node_modules/@vercel/nft/out/node-file-trace.js:304:39
      at async Promise.all (index 0)
      at async Job.emitDependency ([dir]/node_modules/@vercel/nft/out/node-file-trace.js:290:9)
      at async Promise.all (index 0)
      at async nodeFileTrace ([dir]/node_modules/@vercel/nft/out/node-file-trace.js:32:5)
      at async file://[dir]/index.js:3:13
```

Removing the `node:` prefix fixes it, but that's not always an option since it could be in library code.

## Reproduction

Clone this repo, then `npm install && node index.js`.