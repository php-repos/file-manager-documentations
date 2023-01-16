# Introduction

The `Directory` namespace is part of the FileManager package.
Here you can see a list of included functions and their documentation.

## chmod

### Signature

```php
function chmod(string $path, int $permission): bool
```

### Definition

It changes the directory's permission.

> **Note**
> PHP's `umask` does not have any effect on this function.

### Examples

```php
use function PhpRepos\FileManager\Directory\chmod;

chmod($directory_path, 0774); // drwxrwxr--
chmod($directory_path, 0755); // drwxr-xr-x
chmod($directory_path, 0777); // drwxrwxrwx
```

## clean

### Signature

```php
function clean(string $path): void
```

### Definition

It cleans the given directory.
It equals to `rm -fR your-directory/*`.

### Examples

```php
use function PhpRepos\FileManager\Directory\clean;

clean('/root/home/user/project/directory'); // Equals to running rm -fR /root/home/user/project/directory/*
```

## delete

### Signature

```php
function delete(string $path): bool
```

### Definition

Attempts to remove the directory named by directory.
It equals to `rm your-directory`.
The directory must be empty, and the relevant permissions must permit this.
An E_WARNING level error will be generated on failure.

### Examples

```php
use function PhpRepos\FileManager\Directory\delete;

delete('/root/home/user/project/directory'); // Equals to running rm /root/home/user/project/directory
```

## delete_recursive

### Signature

```php
function delete_recursive(string $path): bool
```

### Definition

It deletes the given directory recursively.
It equals to `rm -fR your-directory`.

### Examples

```php
use function PhpRepos\FileManager\Directory\delete_recursive;

delete_recursive('/root/home/user/project/directory'); // Equals to running rm -fR /root/home/user/project/directory
```

## exists

### Signature

```php
function exists(string $path): bool
```

### Definition

It checks the given path and returns `true` if the given path exists, and it is a directory, otherwise returns `false`.

### Examples

```php
use function PhpRepos\FileManager\Directory\exists;

echo (int) exists('/root/home/user/project/directory'); // Output: 1 
echo (int) exists('/root/home/user/project/directory/sub-directory-not-exists'); // Output: 0 
echo (int) exists('/root/home/user/project/directory/filename.txt'); // Output: 0 
```

## exists_or_create

### Signature

```php
function exists_or_create(string $path): bool
```

### Definition

It checks the given path, if the directory exists, and is a directory, returns true.
If the directory does not exist, it makes the directory, and returns true.
Otherwise, it returns false.

### Examples

```php
use function PhpRepos\FileManager\Directory\exists_or_create;

echo (int) exists_or_create('/root/home/user/project/directory'); // Output: 1 
echo (int) exists_or_create('/root/home/user/project/directory/sub-directory-will-get-created'); // Output: 1  
echo (int) exists_or_create('/root/home/user/project/directory/filename.txt'); // Output: 0 
```

## is_empty

### Signature

```php
function is_empty(string $path): bool
```

### Definition

It returns `true` if the given directory is empty.

### Examples

```php
use function PhpRepos\FileManager\Directory\is_empty;

echo (int) is_empty('/root/home/user/project/directory'); // Output: 0 
echo (int) is_empty('/root/home/user/project/directory/sub-directory'); // Output: 1  
```

## ls

### Signature

```php
function ls(string $path): bool
```

### Definition

It returns a list of directory contents.
It excludes hidden contents.
It equals to `ls your-directory`.

### Examples

```php
use function PhpRepos\FileManager\Directory\ls;

ls('/root/home/user/project/directory'); // ['sub-directory'] 
ls('/root/home/user/project/directory/sub-directory'); // []  
```

## ls_all

### Signature

```php
function ls_all(string $path): bool
```

### Definition

It returns a list of directory contents, including the hidden ones.
It equals to `ls -a your-directory`.

### Examples

```php
use function PhpRepos\FileManager\Directory\ls_all;

ls_all('/root/home/user/project/directory'); // ['sub-directory', '.git'] 
ls_all('/root/home/user/project/directory/sub-directory'); // ['.gitignore']  
```

## make

### Signature

```php
function make(string $path, int $permission = 0775): bool
```

### Definition

It makes a directory in the given path with the given permission.
If no permission passes, the permission sets as 0775.

### Examples

```php
use function PhpRepos\FileManager\Directory\exists;
use function PhpRepos\FileManager\Directory\make;
 
make('/root/home/user/project/directory/the-directory-name-you-want-to-create');
echo (int) exists('/root/home/user/project/directory/the-directory-name-you-want-to-create'); // Output: 1
```

## make_recursive

### Signature

```php
function make_recursive(string $path, int $permission = 0775): bool
```

### Definition

It makes a directory recursively in the given path with the given permission.
If no permission passes, the permission sets as 0775.

### Examples

```php
use function PhpRepos\FileManager\Directory\exists;
use function PhpRepos\FileManager\Directory\make;
 
make('/root/home/user/project/not-exists/the-directory-name-you-want-to-create');
echo (int) exists('/root/home/user/project/not-exists/the-directory-name-you-want-to-create'); // Output 1  
```

## permission

### Signature

```php
function permission(string $path): int
```

### Definition

It returns the directory permission.

### Examples

```php
use function PhpRepos\FileManager\Directory\make;
use function PhpRepos\FileManager\Directory\permission;
 
make('/root/home/user/project/directory', 0777);
echo permission('/root/home/user/project/directory'); // Output 0777

make('/root/home/user/project/directory', 0755);
echo permission('/root/home/user/project/directory'); // Output 0755  
```

## preserve_copy

### Signature

```php
function preserve_copy(string $origin, string $destination): bool
```

### Definition

It preserves the permission from the given origin and makes the given destination directory with the same permission.
It equals to `cp -P origin destination`.

### Examples

```php
use function PhpRepos\FileManager\Directory\make;
use function PhpRepos\FileManager\Directory\preserve_copy;
use function PhpRepos\FileManager\Directory\permission;
 
make('/root/home/user/project/directory', 0777);
preserve_copy('/root/home/user/project/directory', '/root/home/user/project2/directory');
echo permission('/root/home/user/project2/directory'); // Output 0777  
```

## preserve_copy_recursive

### Signature

```php
function preserve_copy(string $origin, string $destination): bool
```

### Definition

It copies from the given origin to the given destination by preserving the origin content permissions.
It equals to `cp -RP origin destination`.

### Examples

```php
use function PhpRepos\FileManager\Directory\make;
use function PhpRepos\FileManager\Directory\preserve_copy;
use function PhpRepos\FileManager\Directory\permission;
 
make('/root/home/user/project/directory', 0777);
File\create('/root/home/user/project/directory/file.txt', 0666);
make('/root/home/user/project2/directory');
preserve_copy_recursive('/root/home/user/project/directory', '/root/home/user/project2/directory');
echo permission('/root/home/user/project2/directory/file.txt'); // Output: 0666  
```

## renew

### Signature

```php
function renew(string $path): void
```

### Definition

It makes the directory if it does not exist.
It cleans the directory by deleting its content when it exists.

### Examples

```php
use PhpRepos\FileManager\File;
use function PhpRepos\FileManager\Directory\exists;
use function PhpRepos\FileManager\Directory\renew;
use function PhpRepos\FileManager\Directory\preserve_copy;
use function PhpRepos\FileManager\Directory\permission;

echo (int) exists('/root/home/user/project/directory'); // Output 0
renew('/root/home/user/project/directory');
echo (int) exists('/root/home/user/project/directory'); // Output 1
File\create('/root/home/user/project/directory/file.txt');
renew('/root/home/user/project1/directory');
echo (int) File\exists('/root/home/user/project/directory/file.txt'); // Output 0
```

## renew_recursive

### Signature

```php
function renew_recursive(string $path): void
```

### Definition

It makes the directory recursively if not exists.
It cleans the directory by deleting its content when it exists.

### Examples

```php
use PhpRepos\FileManager\File;
use function PhpRepos\FileManager\Directory\exists;
use function PhpRepos\FileManager\Directory\renew;
use function PhpRepos\FileManager\Directory\preserve_copy;
use function PhpRepos\FileManager\Directory\permission;

echo (int) exists('/root/home/user/project/directory'); // Output 0
renew_recursive('/root/home/user/project/directory/subdirectory');
echo (int) exists('/root/home/user/project/directory/subdirectory'); // Output 1
File\create('/root/home/user/project/directory/subdirectory/file.txt');
renew_recursive('/root/home/user/project1/directory/subdirectory');
echo (int) File\exists('/root/home/user/project/directory/subdirectory/file.txt'); // Output 0
```

