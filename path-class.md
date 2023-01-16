# Introduction

The `Path` class extends Datatype `Text` abstract class.
You can use it to keep any path as a string and use its useful methods.
It also gives you access to an API that you can see in the following.

> **Note**
> For more information on the `Text` class, please read [its documentation](https://phpkg.com/packages/datatype/documentations/text-class)

## Usage

You can make a new `Path` instance and use it like so:

```php
use PhpRepos\FileManager\Path;

$path = Path::from_string('/root/home/user');
echo $path; // Output: '/root/home/user'

$path = new Path('/root/home/user');
echo $path; // Output: '/root/home/user'
```

The `Path` class resolves the given string by using the `Resolver\realpath` function.

> **Note**
> For more information on the `Resolver` functions, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/resolver-functions)

```php
use PhpRepos\FileManager\Path;

$path = Path::from_string('/root/home/user/../project');
echo $path; // Output: '/root/home/project'
```

## Methods

Here you can see a list of the available methods on the `Path` class:

### append

You can use the `append` method to append a substring to the end of your path.
It returns a new `Path` instance with the given substring added to the path.
The new value also gets validated.

```php
public function append(string $path): Path
```

#### Example

```php
use PhpRepos\FileManager\Path;

$path = Path::from_string('/home/user');

$project = $path->append('projects/awesome-project');
echo $project; // Output: /home/user/projects/awesome-project

$resolved_path = $path->append('projects/awesome-project/subdirectory/../filename.txt');
echo $resolved_path; // Output: /home/user/projects/awesome-project/filename.txt
```

### exists

You can use the `exists` method to check if the given path exists on the filesystem.

```php
public function exists(): bool
```

#### Example

```php
use PhpRepos\FileManager\Path;

$path = Path::from_string('/home/user');

echo (int) $path->exists(); // Output: 1
echo (int) $path->append('not-exists')->exists(); // Output: 0
```

### leaf

You can use the `leaf` method to get the leaf of the path.
If the path is the root, it will return the root.

```php
public function leaf(): string
```

#### Example

```php
use PhpRepos\FileManager\Path;

echo Path::from_string('/')->leaf(); // Output: '/'
echo Path::from_string('/home/user/project')->leaf(); // Output: 'project'
echo Path::from_string('/home/user/project/filename.txt')->leaf(); // Output: 'filename.txt' 
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
use PhpRepos\FileManager\Path;

echo Path::from_string('/home/user/project')->parent(); // Output: '/home/user'
echo Path::from_string('/home/user/project/filename.txt')->parent(); // Output: '/home/user/project' 
```

### relocate

The `relocate` method returns a new `Path` instance by replacing the given origin with the given destination.

```php
public function relocate(string $origin, string $destination): Path
```

#### Example

```php
use PhpRepos\FileManager\Path;

$path = new Path('/home/user/directory/filename');
$relocate = $path->relocate('/home/user/directory', '/home/user2/directory/../another-directory');
echo $relocate; // Output: '/home/user2/another-directory/filename' 
```

### sibling

The `sibling` method returns a new `Path` of the given path base on the path parent directory.

```php
public function sibling(string $path): Path
```

#### Example

```php
use PhpRepos\FileManager\Path;

$path = new Path('/home/user/directory/filename');
echo $sibling_directory = $path->sibling('subdirectory'); // Output: /home/user/directory/subdirectory
echo $sibling_filename = $path->sibling('other-file.extension'); // Output: /home/user/directory/other-file.extension
```

### as_file

The `as_file` method returns a new `File` instance of the path.

> **Note**
> For more information on the `File` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/file-class)

```php
public function as_file(): File
```

#### Example

```php
use PhpRepos\FileManager\Path;

$path = new Path('/home/user/directory/filename');
$file = $path->as_file();

echo (int) $file instanceof File; // Output: 1
echo $file; // Output: '/home/user/directory/filename'
```

### as_directory

The `as_directory` method returns a new `Directory` instance of the path.

> **Note**
> For more information on the `Directory` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/directory-class)

```php
public function as_directory(): Directory
```

#### Example

```php
use PhpRepos\FileManager\Path;

$path = new Path('/home/user/directory/subdirectory');
$subdirectory = $path->as_directory();

echo (int) $subdirectory instanceof Directory; // Output: 1
echo $subdirectory; // Output: '/home/user/directory/subdirectory'
```

### as_symlink

The `as_symlink` method returns a new `Symlink` instance of the path.

> **Note**
> For more information on the `Symlink` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/symlink-class)

```php
public function as_symlink(): Symlink
```

#### Example

```php
use PhpRepos\FileManager\Path;

$path = new Path('/home/user/directory/symlink');
$symlink = $path->as_symlink();

echo (int) $symlink instanceof Symlink; // Output: 1
echo $symlink; // Output: '/home/user/directory/symlink'
```
