# Introduction

The `Filename` class extends Datatype `Text` abstract class.
You can use it to keep any filename as a string and make sure the filename has been validated.
The filename should be at least one character.
If the filename starts with `.`, then it should at least be two characters.
If you pass an invalid value to the `Filename`, it throws an `InvalidArgumentException`.
It also gives you access to an API that you can see in the following.

> **Note**
> For more information on the `Text` class, please read [its documentation](https://saeghe.com/packages/datatype/documentations/text-class)

## Usage

You can pass the filename to the constructor:

```php
use Saeghe\FileManager\Filesystem\Filename;

$filename = new Filename('filename');
echo $filename; // Output: 'filename'
echo $filename->string(); // Output: 'filename'

$filename = new Filename('.filename');
echo $filename; // Output: '.filename'
echo $filename->string(); // Output: '.filename'
```
