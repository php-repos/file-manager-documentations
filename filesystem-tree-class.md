# Introduction

The `FilesystemTree` class extends Datatype `Tree` class.
You can use it to keep a filesystem tree.
It also gives you access to an API that you can see in the following.

> **Note**
> For more information on the `Tree` class, please read [its documentation](https://phpkg.com/packages/datatype/documentations/tree-class)

## Usage

You can use the `ls_recursively` function on the `Directory` namespace to get a `FilesystemTree` 
or use the constructor and make it by yourself.

```php
use PhpRepos\FileManager\Path;
use PhpRepos\FileManager\FilesystemTree;

$playground = Path::from_string(root() . 'Tests/PlayGround');
Directory\make($directory = $playground->append('directory'));
Directory\make($subdirectory = $playground->append('directory/subdirectory'));
File\create($file1 = $playground->append('directory/file1.txt'), '');
Symlink\link($symlink = $playground->append('directory/file1.txt'), $playground->append('directory/symlink'));
File\create($file2 = $playground->append('directory/subdirectory/file2.txt'), '');
File\create($file3 = $playground->append('directory/subdirectory/file3.txt'), '');
Directory\make($hidden_directory = $playground->append('directory/subdirectory/.hidden_directory'));
File\create($hidden_file = $playground->append('directory/subdirectory/.hidden_directory/.hidden_file'), '');

assert_true(
    [
        $directory,
        $file1,
        $subdirectory,
        $hidden_directory,
        $hidden_file,
        $file2,
        $file3,
        $symlink,
    ] == $tree->vertices()->items()
);

assert_true(
    [
        new Pair($directory, $file1),
        new Pair($directory, $subdirectory),
        new Pair($subdirectory, $hidden_directory),
        new Pair($hidden_directory, $hidden_file),
        new Pair($subdirectory, $file2),
        new Pair($subdirectory, $file3),
        new Pair($directory, $symlink),
    ] == $tree->edges()->items()
);
```
