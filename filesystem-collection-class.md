# Introduction

The `FilesystemCollection` class extends the `Collection` datatype class.
If you use the `ls` or `ls_all` methods on a `Directory` object, the return type will be a `FilesystemCollection`.
It also gives you access to an API that you can see in the following.

> **Note**
> For more information on the `Collection` class, please read [its documentation](https://phpkg.com/packages/datatype/documentations/collection-class)

## Usage

You can use the constructor to make a new `FilesystemCollection`.

```php
use PhpRepos\FileManager\Filesystem\FilesystemCollection;

$directory = Directory::from_string('/home/user');
$file = File::from_string('/home/user/file');
$symlink = Symlink::from_string('/home/user/symlink');

$collection = new FilesystemCollection([$directory, $file, $symlink]);
$collection->put($new_file);
$collection->push($other_file)->push($new_symlink);
$files = $collection->filter(function (Directory|File|Symlink $item) {
    return $item instanceof File;
});
```

When you use the `ls` or `ls_all` methods on the `Directory` class, the return type is a `FilesystemCollection`:

> **Note**
> For more information on the `Directory` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/directory-class)

```php
use PhpRepos\FileManager\Filesystem\FilesystemCollection;
use PhpRepos\FileManager\Filesystem\Directory;

$directory = Directory::from_string('/home/user');

$collection = $directory->ls();
$collection = $directory->ls_all();
```
