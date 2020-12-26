---
title: Record of "Build Your Own Bundler"
tags: []
categories:
    - []
---

the original [video](https://www.youtube.com/watch?v=Gc9-7PBqOC8&feature=youtu.be)

## Things you may need to know before to start:

1. What is AST(Abstract Syntax Tree).
2. what is Babel and how to use it.
3. Have some basic knowledge with JavaScript and Node.js

Let's get started

Including the file system plugins. Make a function call `createAsset`, which use your entry file as param. Get content by `fs.readFIleSync()`.

```javascript JavaScript
const fs = requre('fs')

function createAsset(filename) {
  const content = fs.readFIleSync(filename,'utf-8',callback)
  console.log(content)
}

createAsset('./exapmle/entry.js')
```
