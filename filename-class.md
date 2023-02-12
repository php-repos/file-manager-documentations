# Introduction

The `Filename` class extends Datatype `Text` class.
You can use it to keep any filename as a string.
It also gives you access to an API that you can see in the following.

> **Note**
> For more information on the `Text` class, please read [its documentation](https://phpkg.com/packages/datatype/documentations/text-class)

## Usage

You can pass the filename to the constructor:

```php
use PhpRepos\FileManager\Filename;

$filename = new Filename('filename');
echo $filename; // Output: 'filename'
echo $filename->string(); // Output: 'filename'

$filename = new Filename('.filename');
echo $filename; // Output: '.filename'
echo $filename->string(); // Output: '.filename'
```
