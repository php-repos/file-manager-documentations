# FileManager Package

## Introduction

`FileManager` Package has a set of useful functions and classes to work with files and directories.
You can use it to work on your project's filesystem.

## Installation

You can run the following command to install this package:

```shell
phpkg add https://github.com/php-repos/file-manager.git
```

## Functions

The `FileManager` package contains some namespaced functions to work with files and directories.
Here you can see a list of available functions.
For more information please read their documentation.

### Directory functions

The `Directory` namespace contains a set of functions to work on directories.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/directory-functions)

### File functions

The `File` namespace contains a set of functions to work on files.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/files-functions)

### Resolver functions

The `Resolver` namespace contains some functions to resolve paths like `realpath` and `root` functions.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/resolver-functions)

### Symlink functions

The `Symlink` namespace contains a set of functions to work on symlinks.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/symlink-functions)

### Json functions

The `Json` namespace contains a set of functions to work on JSON files like reading from and writing to a JSON file.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/json-functions)

## Classes

It also has some useful classes that you can use. Classes like `Path`, `Directory`, `File`, and `Symlink`.
Here you can see a list of available classes.
For more information please read their documentation.

### Path

The `Path` class can get used where you need to work on a path as a string.
It resolves any passed string by using the `realpath` function.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/path-class)

### Directory

The `Directory` class is a useful class when you need to deal with a directory and its subdirectories or files.
Making a directory, changing permissions, and deleting and copying the directory and its children are some examples.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/directory-class)

### File

The `File` class is a useful class when you need to work on a file.
Creating a file, changing permission, copying, or deleting are some methods you can use.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/file-class)

### Filename

The `Filename` class is a simple and useful class. It can be handy for keeping filenames as a variable.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/filename-class)

### Symlink

The `Symlink` class is a useful class when you need to work on a symlink.
Creating a symlink, changing permission, and deleting are some methods you can use.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/symlink-class)

### FilesystemCollection

The `FilesystemCollection` class extends the datatype `Collection` class and allows you to have a collection of filesystem objects.
These objects can be instances of `Directory`, `File`, and `Symlink`.
If you use the `ls` or `ls_all` methods on a `Directory` object, the return type will be a `FilesystemCollection`.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/filesystem-collection-class)

### FilesystemTree

The `FilesystemTree` class extends the datatype `Tree` class and allows you to have a tree of filesystem objects.
You can find some methods that are common use case when you want to work with a filesystem tree.
For more information, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/filesystem-tree-class)
