# Introduction

The `Symlink` class represents a symlink in the file system.
It implements the `Stringable` interface, which means that it can be used as a string and can be casted to a string.
It also gives you access to an API that you can see in the following.

## Usage

You can make a new `Symlink` instance and use it like so:

```php
use PhpRepos\FileManager\Filesystem\Symlink;

$symlink = Symlink::from_string('/root/home/user/symlink');
echo $symlink; // Output: '/root/home/user/symlink'

$symlink = new Symlink('/root/home/user/symlink');
echo $symlink; // Output: '/root/home/user/symlink'
```

Or you can use a `Path` instance:

```php
use PhpRepos\FileManager\Filesystem\Symlink;
use PhpRepos\FileManager\Path;

$path = Path::from_string('/root/home/user/symlink');
$symlink = new Symlink($path);
echo $symlink; // Output: '/root/home/user/symlink'
```

> **Note**
> For more information on the `Path` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/path-class)

The `Symlink` class resolves the given string by using the `Resolver\realpath` function.

```php
use PhpRepos\FileManager\Filesystem\Symlink;

$symlink = Symlink::from_string('/root/home/user/../project/symlink');
echo $symlink; // Output: '/root/home/project/symlink'
```

> **Note**
> For more information on the `Resolver` functions, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/resolver-functions)

## Methods

Here you can see a list of the available methods on the `Symlink` class:

### exists

You can use the `exists` method to check if the given symlink exists on the filesystem.

```php
public function exists(): bool
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Symlink;

$symlink = Symlink::from_string('/home/user/symlink');

echo (int) $symlink->exists(); // Output: 1
echo (int) $symlink->delete()->exists(); // Output: 0
```

### leaf

You can use the `leaf` method to get the leaf of the symlink which is the filename of the symlink.

```php
public function leaf(): string
```

#### Example

```php
use Phprepos\FileManager\Filesystem\Symlink;

echo Symlink::from_string('/home/user/project')->leaf(); // Output: 'project'
echo Symlink::from_string('/home/user/project/filename.txt')->leaf(); // Output: 'filename.txt' 
```

### parent

The `parent` method returns an instance of the `Directory` class from the current path's parent directory.

> **Note**
> For more information on the `Directory` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/directory-class)

```php
public function parent(): Directory
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Symlink;

echo Symlink::from_string('/home/user/symlink')->parent(); // Output: '/home/user'
echo Symlink::from_string('/home/user/project/symlink')->parent(); // Output: '/home/user/project' 
```

### relocate

The `relocate` method returns a new `Path` instance by replacing the given origin with the given destination.

```php
public function relocate(string $origin, string $destination): Path
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Symlink;

$symlink = new Symlink('/home/user/directory/symlink');
$relocate = $symlink->relocate('/home/user/directory', '/home/user2/directory/../another-directory');
echo $relocate; // Output: '/home/user2/another-directory/symlink' 
```

### sibling

The `sibling` method returns a new `Path` of the given path base on the path parent directory.

```php
public function sibling(string $symlink): Path
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Symlink;

$symlink = new Symlink('/home/user/directory/symlink');
echo $sibling_directory = $symlink->sibling('subdirectory'); // Output: /home/user/directory/subdirectory
echo $sibling_filename = $symlink->sibling('other-file.extension'); // Output: /home/user/directory/other-file.extension
```

### delete

The `delete` deletes the symlink from the filesystem.

```php
public function delete(): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Symlink;

$symlink = new Symlink('/home/user/directory/symlink');
echo (int) $symlink->exists(); // Output: 1
$symlink->delete();
echo (int) $symlink->exists(); // Output: 0
```

### exists

The `exists` returns true if the symlink exists on the filesystem and is a link.
Otherwise, it returns false.

```php
public function exists(): bool
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Symlink;

$symlink = new Symlink('/home/user/directory/symlink');
echo (int) $symlink->exists(); // Output: 0
$symlink->link(File::from_string('/home/user/file'));
echo (int) $symlink->exists(); // Output: 1
```

### link

The `link` method creates the symlink on the filesystem and links it to the given file.

```php
public function link(File $file): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Symlink;

$symlink = new Symlink('/home/user/directory/symlink');
echo (int) $symlink->exists(); // Output: 0
$symlink->link(File::from_string('/home/user/file'));
echo (int) $symlink->exists(); // Output: 1
```
