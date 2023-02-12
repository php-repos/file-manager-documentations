# Introduction

The `FilesystemCollection` class extends the `Collection` datatype class.
If you use the `ls` or `ls_all` functions on a `Directory` namespace, the return type will be a `FilesystemCollection`.
It also gives you access to an API that you can see in the following.

> **Note**
> For more information on the `Collection` class, please read [its documentation](https://phpkg.com/packages/datatype/documentations/collection-class)

## Usage

You can use the constructor to make a new `FilesystemCollection`.

```php
use PhpRepos\FileManager\FilesystemCollection;

$directory = Path::from_string('/home/user');
$file = Path::from_string('/home/user/file');
$symlink = Path::from_string('/home/user/symlink');

$collection = new FilesystemCollection([$directory, $file, $symlink]);
$collection->put($new_file);
$collection->push($other_file)->push($new_symlink);
$files = $collection->filter(function (Path $item) {
    return File\exists($item);
});
```

When you use the `ls` or `ls_all` functions on the `Directory` namespace, the return type is a `FilesystemCollection`:

> **Note**
> For more information on the `Directory` namespace, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/directory-functions)

```php
use PhpRepos\FileManager\FilesystemCollection;
use PhpRepos\FileManager\Path;
use function PhpRepos\FileManager\Directory\ls;
use function PhpRepos\FileManager\Directory\ls_all;

$directory = Path::from_string('/home/user');

$collection = ls($directory);
$collection = ls_all($directory);
```
