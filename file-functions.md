# Introduction

The `File` namespace in the `FileManager` package provides a set of functions to perform operations on files.
Here you can see a list of included functions and their documentation.

## chmod

### Signature

```php
function chmod(string $path, int $permission): bool
```

### Definition

The `chmod` function changes the permissions of a file or directory at the specified path.

> **Note**
> PHP's `umask` does not have any effect on this function.

### Examples

```php
use function PhpRepos\FileManager\File\chmod;

chmod($filepath, 0666); // drw-rw-rw-
chmod($filepath, 0600); // drw-------
chmod($filepath, 0660); // drwxrwx---
```

## content

### Signature

```php
function content(string $path): string
```

### Definition

The `content` function returns the content of a file at the specified path.
It equals `cat path`.

### Examples

```php
use function PhpRepos\FileManager\File\content;

content('/path-to-file');
```

## copy

### Signature

```php
function copy(string $origin, string $destination): bool
```

### Definition

It copies the given origin file to the given destination file.
It equals to `cp origin destination`.

### Examples

```php
use function PhpRepos\FileManager\File\copy;

copy('/path-to-origin', '/path-to-destination');
```

## create

### Signature

```php
function create(string $path, string $content, ?int $permission = 0664): bool
```

### Definition

It creates the given path file and put the given content as its content.
The permission of the file sets as the given permission, if not passed, the permission sets as 0664.

### Examples

```php
use function PhpRepos\FileManager\File\create;

create('/path-to-file', 'hello world', 0666);
```

## delete

### Signature

```php
function delete(string $path): bool
```

### Definition

It deletes the file in the given path.
It equals to `rm path-to-file`.

### Examples

```php
use function PhpRepos\FileManager\File\delete;

delete('/path-to-file');
```

## exists

### Signature

```php
function exists(string $path): bool
```

### Definition

It returns `true` if the given path exists, and it is a file. Otherwise, it returns `false`.

### Examples

```php
use function PhpRepos\FileManager\File\create;
use function PhpRepos\FileManager\File\exists;

echo (int) exists('/path-to-file'); // Output: 0
create('/path-to-file', '');
echo (int) exists('/path-to-file'); // Output: 1
```

## lines

### Signature

```php
function lines(string $path): \Generator
```

### Definition

It returns a generator of the file's lines.

### Examples

```php
use function PhpRepos\FileManager\File\create;
use function PhpRepos\FileManager\File\lines;

create('path-to-file', 'First line.' . PHP_EOL . 'Second line.');
$results = [];
foreach (lines($file) as $n => $line) {
    $results[$n] = $line;
}
// Results: [0 => 'First line.' . PHP_EOL, 1 => 'Second line.']
```

## modify

### Signature

```php
function modify(string $path, string $content): bool
```

### Definition

It modifies the content of the file in the given path with the given content.

### Examples

```php
use function PhpRepos\FileManager\File\create;
use function PhpRepos\FileManager\File\content;
use function PhpRepos\FileManager\File\modify;

create('path-to-file', 'create content');
modify('path-to-file', 'modified content');
echo content('path-to-file'); // Output: 'modified content'
```

## move

### Signature

```php
function move(string $origin, string $destination): bool
```

### Definition

It moves the given origin file to the given destination.
It equals to `mv origin destination`.

### Examples

```php
use function PhpRepos\FileManager\File\move;

move('path-to-file', 'path-to-destination');
```

## permission

### Signature

```php
function permission(string $path): int
```

### Definition

It returns the file permission.

### Examples

```php
use function PhpRepos\FileManager\File\create;
use function PhpRepos\FileManager\File\permission;
 
create('/root/home/file', '', 0666);
echo permission('/root/home/file'); // Output 0666

create('/root/home/file', '', 0600);
echo permission('/root/home/file'); // Output 0600  
```

## preserve_copy

### Signature

```php
function preserve_copy(string $origin, string $destination): bool
```

### Definition

It copies the given origin to the given destination and sets the destination's permission as the origin's permission.
It equals to `cp -P origin destination`.

### Examples

```php
use function PhpRepos\FileManager\File\create;
use function PhpRepos\FileManager\File\permission;
use function PhpRepos\FileManager\File\preserve_copy;
 
create('/root/home/file1', '', 0666);
preserve_copy('/root/home/file1', '/root/home/file2');
echo permission('/root/home/file2'); // Output 0666  
```
