# dirtree2json
Simple, flexible lib to convert a directory tree into a json.

# Installation
```
npm install dirtree2json --save
```

# API
## Usage
``` javascript
var dirtree2json = require('dirtree2json');
var result = dirtree2json.dirTojson(path [,options]);
```

## Options

| Option | Type | Default | Description |
|------|:----:|:-------:|-----------|
|attributeName.id | string | "id" | overwrites the name of the 'id' attribute|
|attributeName.absolutePath | string | "absolutePath" | overwrites the name of the 'absolute path' attribute|
|attributeName.child | string | "child" | overwrites the name of the 'children' attribute|
|attributeName.creationTime | string | "creationTime" | overwrites the name of the 'creation time' attribute|
|attributeName.dirFlag | string | "isDir" | overwrites the name of the 'is directory flag' attribute|
|attributeName.extension | string | "ext" | overwrites the name of the 'extension' attribute|
|attributeName.fileName | string | "name" | overwrites the name of the 'file name' attribute|
|attributeName.modificationTime | string | "modificationTime" | overwrites the name of the 'modification time' attribute|
|attributeName.path | string | "path" | overwrites the name of the 'path' attribute|
|attributeName.size | string | "size" | overwrites the name of the 'size' attribute|
|attributeName.state | string | "state" | overwrites the name of the 'state' attribute|
|excludeEmptyFolders | boolean | false | exclude all folders without a file. This is also excludes folders with just empty folders|
|filter.fileExtension | regex or string | null | filter files by file extension name|
|filter.fileName | regex or string | null | filter files by name|
|filter.folderName | regex or string | null | filter folders by name|
|includeId | boolean | false | generate uuid for every node |
|includeAbsolutePath | boolean | false | include the absolute path of the folder/file |
|includeCreationTime | boolean | false | include the creation time of the folder/file|
|includeDirFlag | boolean | true | include is directory flag|
|includeExtension | boolean | true | include the extension of the file|
|includeModificationTime | boolean | false | include the modification time of the folder/file|
|includeSize | boolean | false | include the size of the folder/file|
|rootName | string | name of the relative path | overwrites the name of the root folder|
|rootPath | string | relative path of the folder | overwrites the path attribute of the  the root|
|state | string | state of node | custom states can be kept in here i.e. expanded: true |

## Examples
All examples use this folder structure
```
testDir
├── dir1
│   ├── css1.css
│   ├── file1.txt
│   ├── file2.txt
│   ├── index.html
│   └── js1.js
├── dir2
│   ├── file1.txt
│   └── file2.txt
├── dir3
└── file1.txt
```
### Example 1: Include size, extension and exclude isDir flag
``` javascript
var dirtree2json = require('dirtree2json');

var path = __dirname + '/test/testDir';
var options = {
    includeSize: true,
    includeDirFlag: false,
    includeExtension: true
};

var result = dirtree2json.dirTojson(path, options);
```
Result:
``` javascript
{
  "ext": "",
  "path": "testDir",
  "name": "testDir",
  "child": [
    {
      "ext": "",
      "path": "testDir/dir1",
      "name": "dir1",
      "child": [
        {
          "ext": "css",
          "path": "testDir/dir1/css1.css",
          "name": "css1.css",
          "size": 0
        },
        {
          "ext": "txt",
          "path": "testDir/dir1/file1.txt",
          "name": "file1.txt",
          "size": 10
        },
        {
          "ext": "txt",
          "path": "testDir/dir1/file2.txt",
          "name": "file2.txt",
          "size": 11
        },
        {
          "ext": "html",
          "path": "testDir/dir1/index.html",
          "name": "index.html",
          "size": 0
        },
        {
          "ext": "js",
          "path": "testDir/dir1/js1.js",
          "name": "js1.js",
          "size": 0
        }
      ],
      "size": 21
    },
    {
      "ext": "",
      "path": "testDir/dir2",
      "name": "dir2",
      "child": [
        {
          "ext": "txt",
          "path": "testDir/dir2/file1.txt",
          "name": "file1.txt",
          "size": 10
        },
        {
          "ext": "txt",
          "path": "testDir/dir2/file2.txt",
          "name": "file2.txt",
          "size": 11
        }
      ],
      "size": 21
    },
    {
      "ext": "",
      "path": "testDir/dir3",
      "name": "dir3",
      "child": [],
      "size": 0
    },
    {
      "ext": "txt",
      "path": "testDir/file1.txt",
      "name": "file1.txt",
      "size": 13
    }
  ],
  "size": 55
}
```

### Example 2: Override the root path and name
``` javascript
var dirtree2json = require('dirtree2json');

var path = __dirname + '/test/testDir';
var options = {
    rootPath: ".",
    rootName: "root"
};

var result = dirtree2json.dirTojson(path, options);
```
Result:
``` javascript
{
  "ext": "",
  "isDir": true,
  "path": ".",
  "name": "root",
  "child": [
    {
      "ext": "",
      "path": "./dir1",
      "name": "dir1",
      "isDir": true,
      "child": [...]
    },
    ...
  ]
}
```

### Example 3: Filter by file name and exclude empty folders
``` javascript
var dirtree2json = require('dirtree2json');

var path = __dirname + '/test/testDir';
var options = {
    filter: {
        fileExtension: /html/i
    },
    excludeEmptyFolders: true
};

var result = dirtree2json.dirTojson(path, options);
```
Result:
``` javascript
{
  "ext": "",
  "isDir": true,
  "path": "testDir",
  "name": "testDir",
  "child": [
    {
      "ext": "",
      "path": "testDir/dir1",
      "name": "dir1",
      "isDir": true,
      "child": [
        {
          "ext": "html",
          "path": "testDir/dir1/index.html",
          "name": "index.html",
          "isDir": false
        }
      ]
    }
  ]
}
```

### Example 4: Override attribute names
``` javascript
var dirtree2json = require('dirtree2json');

var path = __dirname + '/test/testDir';
var options = {
    attributeName: {
        path: "relativePath",
        fileName: "folderFileName",
        extension: "extension",
        dirFlag: "isFolder",
        child: "children"
    }
};

var result = dirtree2json.dirTojson(path, options);
```
Result:
``` javascript
{
  "extension": "",
  "isFolder": true,
  "relativePath": "testDir",
  "folderFileName": "testDir",
  "children": [
    {
      "extension": "",
      "relativePath": "testDir/dir1",
      "folderFileName": "dir1",
      "isFolder": true,
      "children": [
        {
          "extension": "css",
          "relativePath": "testDir/dir1/css1.css",
          "folderFileName": "css1.css",
          "isFolder": false
        },
        ...
      ]
    },
    ...
  ]
}

```

# CLI
## Install
npm install -g dirtree2json
## Usage
dirtree2json [options] <directory-path>

```
Options
  -h, --help        Print this usage guide
  -p, --prettify    Enable json pretty print
  -j, --json file   Json file with dirtree2json configuration. The configuration json is the same of the API
```
