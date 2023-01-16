# Introduction

The `File` class represents a file in the file system.
It implements the `Stringable` interface, which means that it can be used as a string and can be casted to a string.
It also gives you access to an API that you can see in the following.

## Usage

You can make a new `File` instance and use it like so:

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/root/home/user/filename.extension');
echo $file; // Output: '/root/home/user/filename.extension'

$file = new Path('/root/home/user/filename.extension');
echo $file; // Output: '/root/home/user/filename.extension'
```

Or you can use a `Path` instance:

```php
use PhpRepos\FileManager\Filesystem\File;
use PhpRepos\FileManager\Path;

$path = Path::from_string('/root/home/user/filename.extension');
$file = new File($path);
echo $file; // Output: '/root/home/user/filename.extension'
```

> **Note**
> For more information on the `Path` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/path-class)

The `File` class resolves the given string by using the `Resolver\realpath` function.

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/root/home/user/../project/file');
echo $file; // Output: '/root/home/project/file'
```

> **Note**
> For more information on the `Resolver` functions, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/resolver-functions)

## Methods

Here you can see a list of the available methods on the `File` class:

### exists

You can use the `exists` method checks if the given file exists on the filesystem.

```php
public function exists(): bool
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/home/user/file');

echo (int) $file->exists(); // Output: 1
echo (int) $file->delete()->exists(); // Output: 0
```

### leaf

You can use the `leaf` method to get the leaf of the file which is the filename of the file.

```php
public function leaf(): string
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

echo File::from_string('/home/user/filename')->leaf(); // Output: 'filename'
echo File::from_string('/home/user/project/filename.txt')->leaf(); // Output: 'filename.txt' 
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
use PhpRepos\FileManager\Filesystem\File;

echo File::from_string('/home/user/filename')->parent(); // Output: '/home/user'
echo File::from_string('/home/user/project/filename.txt')->parent(); // Output: '/home/user/project' 
```

### relocate

The `relocate` method returns a new `Path` instance by replacing the given origin with the given destination.

```php
public function relocate(string $origin, string $destination): Path
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = new File('/home/user/directory/filename');
$relocate = $file->relocate('/home/user/directory', '/home/user2/directory/../another-directory');
echo $relocate; // Output: '/home/user2/another-directory/filename' 
```

### sibling

The `sibling` method returns a new `Path` of the given path base on the path parent directory.

```php
public function sibling(string $file): Path
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = new File('/home/user/directory/filename');
echo $sibling_directory = $file->sibling('subdirectory'); // Output: /home/user/directory/subdirectory
echo $sibling_filename = $file->sibling('other-file.extension'); // Output: /home/user/directory/other-file.extension
```

### chmod

The `chmod` method sets the given permission as permission for the file.

```php
public function chmod(int $permission): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/home/user/file');
$file->chmod(0777);
echo $file->permission(); // Output: 0777
```

### content

The `content` method returns the content of the file as a string.

```php
public function content(): string
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/home/user/file');
$file->create('file content');
echo $file->content(); // Output: 'file content'
```

### create

The `create` method creates the file with the given content.
It sets the file permission as the given permission.
If permission does not pass, it sets the default permission which is 0664.

```php
public function create(string $content, int $permission = 0664): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/home/user/file');
$file->create('file content');
echo $file->content(); // Output: 'file content'
echo $file->permission(); // Output: 0664

$file = File::from_string('/home/user/file');
$file->create('file content', 0600);
echo $file->content(); // Output: 'file content'
echo $file->permission(); // Output: 0600
```

### delete

The `delete` method deletes the file.

```php
public function delete(): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/home/user/file');
$file->create('file content');
echo (int) $file->exists(); // Output: 1
$file->delete();
echo (int) $file->exists(); // Output: 0
```

### lines

The `lines` method return a `Generator` of file lines.

```php
public function lines(): \Generator
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/home/user/file');
$file->create('First line.' . PHP_EOL . 'Second line.');
$results = [];
foreach ($file->lines() as $n => $line) {
    $results[$n] = $line;
}
// Results: [0 => 'First line.' . PHP_EOL, 1 => 'Second line.']
```

### modify

The `modify` method modifies the file contents with the given content.

```php
public function modify(string $content): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/home/user/file');
$file->create('create content');
$file->modify('new content');
echo $file->content(); // Output: 'new content'
```

### permission

The `permission` method returns the file's permission.

```php
public function permission(): int
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/home/user/file');
$file->create('file content');
echo $file->permission(); // Output: 0664

$file = File::from_string('/home/user/file');
$file->create('file content', 0600);
echo $file->permission(); // Output: 0600
$file->chmod(0666);
echo $file->permission(); // Output: 0666
```

### preserve_copy

It copies the file to the given destination and sets the destination's permission as the file's permission.
It equals to `cp -P origin destination`.

```php
public function permission(): int
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\File;

$file = File::from_string('/home/user/file');
$file->create('file content');
$copied_file = $file->preserve_copy('/home/user/copied-file');
echo $copied_file->permission(); // Output: 0664

$file = File::from_string('/home/user/file');
$file->create('file content', 0600);
$copied_file = $file->preserve_copy('/home/user/copied-file');
echo $copied_file->permission(); // Output: 0600
```

