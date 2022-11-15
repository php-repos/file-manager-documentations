# Introduction

The `Resolver` namespace is part of the FileManager package.
Here you can see a list of included functions and their documentation.

## realpath

### Signature

```php
function realpath(string $path_string): string
```

### Definition

It resolves the path and returns an actual path of the given path string.
It does not need for the path to exist.

### Examples

```php
echo realpath('/user/home/./directory/filename.extension'); // Output: '/user/home/directory/filename.extension'
echo realpath('/user/home/./directory/another-directory/../filename.extension'); // Output: '/user/home/directory/filename.extension'
```

## root

### Signature

```php
function root(): string
```

### Definition

It returns the path of the current working directory.

### Examples

```php
// Assume you are running the index.php under /home/user/project/public directory
echo root(); // Output: /home/user/project/public/index.php
```
