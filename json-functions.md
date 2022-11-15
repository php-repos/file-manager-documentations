# Introduction

The `Json` namespace is part of the FileManager package.
Here you can see a list of included functions and their documentation.

## to_array

### Signature

```php
function to_array(string $path): array
```

### Definition

It returns an array from the given JSON file.

### Examples

```php
File\create('file.json', json_encode(['foo' => 'bar']));
$result = Json\to_array('file.json');
// result: ['foo' => 'bar']
```

## write

### Signature

```php
function write(string $path, array $data): bool
```

### Definition

It creates a file in the given path with a JSON encoded content of the given data.

### Examples

```php
$arr = ['foo' => 'bar'];
Json\write('file.json', $arr);
echo File\content('file.json');
```

Output:

```shell
{
  "foo": "bar"
}
```
