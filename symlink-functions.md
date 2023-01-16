# Introduction

The `Symlink` namespace provides functions to work with symbolic links in the file system.
It also gives you access to an API that you can see in the following.

## delete

### Signature

```php
function delete(string $path): bool
```

### Definition

This function deletes the symbolic link specified by path. It returns `true` on success and `false` on failure.

### Examples

```php
use function PhpRepos\FileManager\Symlink\delete;

$file = Path::from_string(root() . 'Tests/PlayGround/LinkSource');
File\create($file, 'file content');
$link = $file->parent()->append('symlink');
link($file->as_file(), $link);
assert_true(delete($link));
assert_true($file->exists());
assert_false($link->exists());
```

## exists

### Signature

```php
function exists(string $path): bool
```

### Definition

This function checks if the given path is a symbolic link. It returns a boolean value, `true` if the path is a symbolic link, `false` otherwise.

### Examples

```php
use function PhpRepos\FileManager\Symlink\exists;

$file = Path::from_string(root() . 'Tests/PlayGround/LinkSource');
create($file, 'file content');
$link = $file->parent()->append('symlink');
assert_false(exists($link));

link($file, $link);
assert_true(exists($link));
```

## link

### Signature

```php
function link(string $source_path, string $link_path): bool
```

### Definition

This function creates a symbolic link at the specified link_path, pointing to the specified source_path. 
It returns a boolean value, `true` on success and `false` on failure.

### Examples

```php
use function PhpRepos\FileManager\Symlink\link;

$file = Path::from_string(root() . 'Tests/PlayGround/LinkSource');
create($file, 'file content');
$link = $file->parent()->append('symlink');
assert_true(link($file, $link));
assert_true($file->string() === readlink($link));
```

## target

### Signature

```php
function target(string $path): string
```

### Definition

This function returns the target of the symbolic link specified by path.

### Examples

```php
use function PhpRepos\FileManager\Symlink\target;

$file = Path::from_string(root() . 'Tests/PlayGround/LinkSource');
create($file, 'file content');
$link = $file->parent()->append('symlink');
link($file, $link);
assert_true($file->string() === target($link));
```
