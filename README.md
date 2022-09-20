# Flysystem adapter for Microsoft OneDrive
This package contains a Flysystem OneDrive adapter, which is operated with the Microsoft Graph API.
The adapter can also be used with the latest Laravel 9.x version.

## 1. Installation
You can install the package via composer:

`composer require justus/flysystem-onedrive`

## 2. Usage

### Laravel Usage
1. Add the following variable to the ``.env`` file

```dotenv
ONEDRIVE_ROOT=root/path
ONEDRIVE_ACCESS_TOKEN=fd6s7a98...
```

2. In the file ``config/filesystems.php``, please add the following code snippet in the disks section

```php
onedrive' => [
    'driver' => 'onedrive',
    'root' => env('ONEDRIVE_ROOT'),
    'access_token' => env('ONEDRIVE_ACCESS_TOKEN') //optional when on demand
],
```

3. Add the ``OneDriveAdapterServiceProvider`` in ``config/app.php``

```php
'providers' => [
    // ...
    Justus\FlysystemOneDrive\Providers\OneDriveAdapterServiceProvider::class,
    // ...
],
```

4. In code usage

There are two established approaches to using the package
- On demand: Recommended for use with a dynamic graph access token. (usage e. g. session('graph_access_token'))
```php
$disk = Storage::build([
    'driver' => config('filesystems.disks.onedrive.driver'),
    'root' => config('filesystems.disks.onedrive.root'),
    'use_path' => true,
    'access_token' => session('graph_access_token')
]);

$disk->makeDirectory('test');
```
- Default with Storage Facade: Recommended for use with a "fixed" graph access token.
```php
Storage::disk('onedrive')->makeDirectory('test');
```
### PHP Usage
Usage in default php usage without Laravel framework
```php
$graph = new Graph();
$graph->setAccessToken('fd6s7a98...');

$adapter = new OneDriveAdapter($graph, 'root/path', true);

$filesystem = new Filesystem($adapter);

$filesystem->createDirectory('test');
```

## 3. Changelog
Please see CHANGELOG for more information what has changed recently.

## 4. Testing
`$ composer test`

## 5. Security
If you discover any security related issues, please email jdonner@doerffler.com instead of using the issue tracker.

## 6. License
The MIT License (MIT). Please see License File for more information.
