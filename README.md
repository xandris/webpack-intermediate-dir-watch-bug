# Webpack `--watch` intermediate directory bug

To reproduce:

```bash
$ npm ci
$ npm start
```

Then edit `src/entry/index.js`:

```javascript
require('../utils');

console.log('entry here');
```

Comment the first line and save it:

```javascript
// require('../utils');

console.log('entry here');
```

The project will recompile correctly.

Finally, uncomment the first line and save it. The project will not recompile.

I understand that Webpack watches directories, not individual files, to save on
resources, and it uses a reference count on these directory watchers to remove
them when no longer needed.  I believe this problem occurs because the refcount
on `src/` goes to zero because `src/utils.js` was no longer referenced. The
refcount doesn't seem to take into account the watch on `src/entry/`.
