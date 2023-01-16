# Introduction

The `FilesystemTree` class extends Datatype `Tree` class.
You can use it to keep a filesystem tree.
It also gives you access to an API that you can see in the following.

> **Note**
> For more information on the `Tree` class, please read [its documentation](https://phpkg.com/packages/datatype/documentations/tree-class)

## Usage

You can use the `recursively` method on the `Directory` class to get a `FilesystemTree` 
or use the constructor and make it by yourself.

```php
use PhpRepos\FileManager\Filesystem\Directory;
use PhpRepos\FileManager\Filesystem\FilesystemTree;

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

## Methods

Here you can see a list of the available methods on the filesystem tree:

### files

It returns a collection of files in the tree, recursively, contains the hidden files.

```php
public function files(): FilesystemCollection
```

#### Example

```php
$root = Directory::from_string('/');
$root_file = $root->file('file');
$home = $root->subdirectory('home');
$file1 = $home->file('file1');
$symlink = $home->symlink('symlink');
$documents = $home->subdirectory('documents');
$document_file = $documents->file('file');

$tree = new FilesystemTree($root);
$tree->edge($root, $home)
    ->edge($root, $root_file)
    ->edge($home, $file1)
    ->edge($home, $symlink)
    ->edge($home, $documents)
    ->edge($documents, $document_file);

assert_true([
    $root_file,
    $file1,
    $document_file
] == $tree->files()->items());
```
