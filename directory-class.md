# Introduction

The `Directory` class in the `FileManager` package is a class that represents a directory in the file system.
It implements the `Stringable` interface, which means it can be converted to a string.
The `Directory` class provides methods for working with the directory's location in the file system.
The class has several methods to interact with the file system, such as creating, 
deleting, and listing the files and subdirectories in the directory.
The class also has methods to work with permissions and copying the directory.
Additionally, the class has a `recursively()` method which returns a `FilesystemTree` object,
which is a tree representation of the directory with all its subdirectories and files.
It also gives you access to an API that you can see in the following.

> **Note**
> For more information on the `Text` class, please read [its documentation](https://phpkg.com/packages/datatype/documentations/text-class)
 
> **Note**
> For more information on the `Tree` class, please read [its documentation](https://phpkg.com/packages/datatype/documentations/tree-class)

## Usage

You can make a new `Directory` instance and use it like so:

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/root/home/user');
echo $directory; // Output: '/root/home/user'

$directory = new Directory(new Path('/root/home/user'));
echo $directory; // Output: '/root/home/user'
```

The `Directory` class resolves the given string by using the `Resolver\realpath` function.

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/root/home/user/../project');
echo $directory; // Output: '/root/home/project'
```

## Methods

Here you can see a list of the available methods on the `Directory` class:

### append

You can use the `append` method to append a substring to the end of your path.
It returns a new `Path` instance with the given substring added to the path.
The new value also gets validated.

> **Note**
> For more information on the `Path` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/path-class)

```php
public function append(string $directory): Path
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user');

$project = $directory->append('projects/awesome-project');
echo $project; // Output: /home/user/projects/awesome-project

$resolved_path = $directory->append('projects/awesome-project/subdirectory/../filename.txt');
echo $resolved_path; // Output: /home/user/projects/awesome-project/filename.txt
```

### exists

You can use the `exists` method checks if the given directory exists and is a directory.

```php
public function exists(): bool
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user');

echo (int) $directory->exists(); // Output: 1
echo (int) $directory->append('not-exists')->exists(); // Output: 0
```

### leaf

You can use the `leaf` method to get the leaf of the directory, that is the name of the directory.
If the directory is the root, it will return the root.

```php
public function leaf(): string
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

echo Directory::from_string('/')->leaf(); // Output: '/'
echo Directory::from_string('/home/user/project')->leaf(); // Output: 'project' 
```

### parent

The `parent` method returns an instance of the `Directory` class from the current path's parent directory.

```php
public function parent(): Directory
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

echo Directory::from_string('/home/user/project')->parent(); // Output: '/home/user' 
```

### relocate

The `relocate` method returns a new `Path` instance by replacing the given origin with the given destination.

```php
public function relocate(string $origin, string $destination): Path
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory/subdirectory');
$relocate = $directory->relocate('/home/user/directory', '/home/user2/directory/../another-directory');
echo $relocate; // Output: '/home/user2/another-directory/subdirectory' 
```

### sibling

The `sibling` method returns a new `Path` of the given path base on the path parent directory.

```php
public function sibling(string $directory): Path
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory/filename');
echo $sibling_directory = $directory->sibling('subdirectory'); // Output: /home/user/directory/subdirectory
echo $sibling_filename = $directory->sibling('other-file.extension'); // Output: /home/user/directory/other-file.extension
```

### clean

It removes all files and directories within a directory, but leaves the directory itself.

```php
public function exists(): bool
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string(root() . 'MainDirectory');
$directory->file('file.txt')->create('test content');
$directory->subdirectory('Subdirectories')->make_recursive();
$directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->make_recursive();
$directory->subdirectory('Subdirectories')->file('file.txt')->create('test content');
$directory->clean();
assert_true($directory->exists());
assert_true($directory->ls_all()->items() === []);
```

### chmod

The `chmod` method sets the given permission as permission for the directory.

```php
public function chmod(int $permission): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory');
$directory->chmod(0777);
echo $directory->permission(); // Output: 0777
```

### delete

The `delete` method tries to delete the directory.

```php
public function delete(): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory');
echo (int) $directory->exists(); // Output: 1
$directory->delete();
echo (int) $directory->exists(); // Output: 0
```

### delete_recursive

The `delete_recursive` method deletes the directory recursively.

```php
public function delete_recursive(): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

// Directory has some contents
$directory = Directory::from_string('/home/user/directory');
$directory->delete_recursive();
echo (int) $directory->exists(); // Output: 0
```

### exists_or_create

The `exists_or_create` method checks to see if the directory exists.
If exists, returns true, if not, it makes the directory and returns true.

```php
public function exists_or_create(): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

// Directory has some contents
$directory = Directory::from_string('/home/user/directory');
echo (int) $directory->exists(); // Output: 0
$directory->exists_or_create();
echo (int) $directory->exists(); // Output: 1
$directory->exists_or_create();
echo (int) $directory->exists(); // Output: 1
```

### file

The `file` method returns a new `File` instance of the given path under the directory.

> **Note**
> For more information on the `File` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/file-class)

```php
public function file(string $directory): File
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory');
$file = $directory->file('filename')
echo (int) $file instanceof File; // Output: 1
echo $file; // Output: '/home/user/directory/filename'
```

### item

The `item` method checks the given path.
If the given path, based on the current directory, is a directory, it returns a `Directory` instance of the path.
If the given path, based on the current directory, is a symlink, it returns a `Symlink` instance of the path.
If the given path, is based on the current directory, otherwise it returns a `File` instance.

> **Note**
> For more information on the `File` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/file-class)
> For more information on the `Symlink` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/symlink-class)

```php
public function item(string $directory): Directory|File|Symlink
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory');

$subdirectory = $directory->item('subdirectory');
echo (int) $subdirectory instanceof Directory; // Output: 1

$symlink = $directory->item('symlink');
echo (int) $symlink instanceof Symlink; // Output: 1

$file = $directory->item('file');
echo (int) $file instanceof File; // Output: 1
```

### ls

The `ls` method returns a `FilesystemCollection` instance containing all the items in the directory.

> **Note**
> For more information on the `FilesystemCollection` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/filesystem-collection-class)

```php
public function ls(): FilesystemCollection
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory');
$collection = $directory->ls();
foreach ($collection as $item) {
    $item->exists();
}
```

### ls_all

The `ls_all` method returns a `FilesystemCollection` instance containing all the items in the directory, including hidden files.

> **Note**
> For more information on the `FilesystemCollection` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/filesystem-collection-class)

```php
public function ls_all(): FilesystemCollection
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory');
$collection = $directory->ls_all();
foreach ($collection as $item) {
    $item->exists();
}
```

### make

The `make` method makes the directory on the filesystem.
It sets the given permission as the permission on the directory.
If permission does not passed, it sets the default 0775 as the permission.

```php
public function make(int $permission = 0775): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory');
echo (int) $directory->exists(); // Output: 0
$directory->make(0777);
echo (int) $directory->exists(); // Output: 1
echo $directory->permission(); // Output: 0777
```

### make_recursive

The `make_recursive` method makes the directory recursively on the filesystem.
It sets the given permission as the permission on the directory.
If permission does not passed, it sets the default 0775 as the permission.

```php
public function make_recursive(int $permission = 0775): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory/subdirectory');
echo (int) $directory->parent()->exists(); // Output: 0
echo (int) $directory->exists(); // Output: 0
$directory->make(0777);
echo (int) $directory->parent()->exists(); // Output: 1
echo (int) $directory->exists(); // Output: 1
echo $directory->permission(); // Output: 0777
```

### permission

The `permission` returns the directory's permission.

```php
public function permission(): int
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory');
$directory->make(0777);
echo $directory->permission(); // Output: 0777

$directory = Directory::from_string('/home/user/another-directory');
$directory->make(0755);
echo $directory->permission(); // Output: 0755
```

### preserve_copy

It preserves the permission from the given origin and makes the given destination directory with the same permission.
It equals to `cp -P origin destination`.

```php
public function preserve_copy(Directory $destination): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/root/home/user/project/directory');
$directory->make(0777);
$other_directory = $directory->preserve_copy('/root/home/user/project2/directory'); 
echo $other_directory->permission(); // Output 0777
```

### recursively

The `recursively` method returns a `FilesystemTree` object, which represents the directory tree starting from the current directory object.
The tree is constructed by recursively traversing the directory structure and adding each directory and file as vertices to the tree,
and the parent-child relationship as edges.
The current directory object is set as the `root` of the tree.
The returned tree object can be used to access the entire directory structure, including all subdirectories and files.

> **Note**
> For more information on the `FilesystemTree` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/filesystem-tree-class)

```php
public function recursively(): FilesystemTree
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string(root() . 'PlayGround');
$directory->file('.hidden')->create('');
$directory->subdirectory('Subdirectories')->make_recursive();
$directory->subdirectory('Subdirectories')->file('file1.txt')->create('');
$directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->make_recursive();
$directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->symlink('symlink1')
    ->link($directory->subdirectory('Subdirectories')->file('file1.txt'));
$directory->subdirectory('Subdirectories')->subdirectory('subdirectory2')->make_recursive();
$directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3')->make_recursive();
$directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3')->file('file2.txt')->create('');
$directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3')->symlink('symlink2')
    ->link($directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3')->file('file2.txt'));
    
$results = $directory->recursively();

assert_true([
    $directory,
    $directory->file('.hidden'),
    $directory->subdirectory('Subdirectories'),
    $directory->subdirectory('Subdirectories')->file('file1.txt'),
    $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1'),
    $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3'),
    $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3')->file('file2.txt'),
    $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3')->symlink('symlink2'),
    $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->symlink('symlink1'),
    $directory->subdirectory('Subdirectories')->subdirectory('subdirectory2'),
] == $results->vertices()->items());

assert_true([
    new Pair($directory, $directory->file('.hidden')),
    new Pair($directory, $directory->subdirectory('Subdirectories')),
    new Pair($directory->subdirectory('Subdirectories'), $directory->subdirectory('Subdirectories')->file('file1.txt')),
    new Pair($directory->subdirectory('Subdirectories'), $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')),
    new Pair(
        $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1'),
        $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3')
    ),
    new Pair(
        $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3'),
        $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3')->file('file2.txt')
    ),
    new Pair(
        $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3'),
        $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->subdirectory('subdirectory3')->symlink('symlink2')
    ),
    new Pair(
        $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1'),
        $directory->subdirectory('Subdirectories')->subdirectory('subdirectory1')->symlink('symlink1')
    ),
    new Pair($directory->subdirectory('Subdirectories'), $directory->subdirectory('Subdirectories')->subdirectory('subdirectory2')),
] == $results->edges()->items());
```

### renew

It makes the directory if does not exist.
It cleans the directory by deleting its content when it exists.

```php
public function renew(): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/root/home/user/project/directory');
echo (int) $directory->exists(); // Output 0
$directory->renew();
echo (int) $directory->exists(); // Output 1
File\create('/root/home/user/project/directory/file.txt');
$directory->renew();
echo (int) File\exists('/root/home/user/project/directory/file.txt'); // Output 0
```

### renew_recursive

It makes the directory recursively if not exists.
It cleans the directory by deleting its content when it exists.

```php
public function renew_recursive(): self
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/root/home/user/project/directory/subdirectory');
echo (int) $directory->parent()->exists(); // Output 0
echo (int) $directory->exists(); // Output 0
$directory->renew_recursive();
echo (int) $directory->parent()->exists(); // Output 1
echo (int) $directory->exists(); // Output 1
File\create('/root/home/user/project/directory/subdirectory/file.txt');
$directory->renew_recursive();
echo (int) File\exists('/root/home/user/project/directory/subdirectory/file.txt'); // Output 0
```

### subdirectory

It returns a new instance of the subdirectory base on the current directory.

```php
public function subdirectory(string $directory): static
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/root/home/user/directory');
$result = $directory->subdirectory('Subdirectory');
echo $result; // Output: '/root/home/user/directory/Subdirectory'
```

### symlink

The `symlink` method returns a new `Symlink` instance of the given path under the directory.

> **Note**
> For more information on the `Symlink` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/symlink-class)

```php
public function symlink(string $directory): Symlink
```

#### Example

```php
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user/directory');
$symlink = $directory->symlink('symlink')
echo (int) $symlink instanceof Symlink; // Output: 1
echo $symlink; // Output: '/home/user/directory/symlink'
```
