I've noticed a couple of issues with the way the compiler resolves the output directory structure. I've attached a minimal project that can be used to reproduce these issues, it's setup as an NPM package, so after downloading and extracting just run `npm install` to install the latest nightly build of TypeScript (support for filenames in the `--project` compiler option was added very recently).

Issue 1
======
From the package root run
```
node ./node_modules/typescript/bin/tsc --project src/good-tsconfig.json
```
Note that the output directory structure is (as expected):
```
lib/
  first/
    file1.js
  second/
    file2.js
```
Now run
```
node ./node_modules/typescript/bin/tsc --project src/bad-tsconfig.json
```
Note that the output directory structure is missing the subdir `second`:
```
lib2/
  file2.js
```
While the expected output directory structure is:


```
lib2/
  second/
    file2.js
```

Issue 2
======
I'm not sure if this is a bug or just user error, but it can be reproduced starting with TypeScript 1.6.2 (and possibly earlier, that's just the oldest version I've tried). Run
```
node ./node_modules/typescript/bin/tsc --project src/second
```
Note that the output directory structure is:
```
lib3/
  second/
    first/
      file1.js
    second/
      file2.js
```
While the expected output directory structure is:
```
lib3/
  first/
    file1.js
  second/
    file2.js
```
