# Introduction

The `Filename` class extends Datatype `Text` class.
You can use it to keep any filename as a string.
It also gives you access to an API that you can see in the following.

> **Note**
> For more information on the `Text` class, please read [its documentation](https://phpkg.com/packages/datatype/documentations/text-class)

## Usage

You can pass the filename to the constructor:

```php
use PhpRepos\FileManager\Filesystem\Filename;

$filename = new Filename('filename');
echo $filename; // Output: 'filename'
echo $filename->string(); // Output: 'filename'

$filename = new Filename('.filename');
echo $filename; // Output: '.filename'
echo $filename->string(); // Output: '.filename'
```

## Methods

Here you can see a list of the available methods on the filename:

### directory

It returns a `Directory` object by appending the filename to the given root directory.

> **Note**
> For more information on the `Directory` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/directory-class)

```php
public function directory(Directory $root): Directory
```

#### Example

```php
$filename = new Filename('subdirectory');
$result = $filename->directory(Directory::from_string('/home/user'));

assert_true($result instanceof Directory);
assert_true('/home/user/subdirectory' === $result->path->string());
```

### file

It returns a `File` object by appending the filename to the given root directory.

> **Note**
> For more information on the `File` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/file-class)

```php
public function file(Directory $root): File
```

#### Example

```php
$filename = new Filename('filename');
$result = $filename->file(Directory::from_string('/home/user'));

assert_true($result instanceof File);
assert_true('/home/user/filename' === $result->path->string());
```

### symlink

It returns a `Symlink` object by appending the filename to the given root directory.

> **Note**
> For more information on the `Symlink` class, please read [its documentation](https://phpkg.com/packages/file-manager/documentations/symlink-class)

```php
public function symlink(Directory $root): Symlink
```

#### Example

```php
$filename = new Filename('symlink');
$result = $filename->symlink(Directory::from_string('/home/user'));

assert_true($result instanceof Symlink);
assert_true('/home/user/symlink' === $result->path->string());
```
