backblaze-b2 is the SDK for working with Backblaze's B2 storage service.

Install
Via Composer

$ composer require mwebdev/backblaze-b2

Usage
use BackblazeB2\Client;
use BackblazeB2\Bucket;

$options = ['auth_timeout_seconds' => seconds];

$client = new Client('accountId', 'applicationKey', $options);
$options is optional. If omitted, the default timeout is 12 hours. The timeout allows for a long lived Client object so that the authorization token does not expire.

ApplicationKey is not supported yet, please use MasterKey only
Returns a bucket details
$bucket = $client->createBucket([
    'BucketName' => 'my-special-bucket',
    'BucketType' => Bucket::TYPE_PRIVATE // or TYPE_PUBLIC
]);
Change the bucket Type
$updatedBucket = $client->updateBucket([
    'BucketId' => $bucket->getId(),
    'BucketType' => Bucket::TYPE_PUBLIC
]);
List all buckets
$buckets = $client->listBuckets();
Delete a bucket
$client->deleteBucket([
    'BucketId' => 'YOUR_BUCKET_ID'
]);
File Upload
$file = $client->upload([
    'BucketName' => 'my-special-bucket',
    'FileName' => 'path/to/upload/to',
    'Body' => 'I am the file content'

    // The file content can also be provided via a resource.
    // 'Body' => fopen('/path/to/input', 'r')
]);
File Download
$fileContent = $client->download([
    'FileId' => $file->getId()

    // Can also identify the file via bucket and path:
    // 'BucketName' => 'my-special-bucket',
    // 'FileName' => 'path/to/file'

    // Can also save directly to a location on disk. This will cause download() to not return file content.
    // 'SaveAs' => '/path/to/save/location'
]);
File Copy
$copyOfFile = $client->copy([
    'BucketName' => $bucketName,
    'FileName'   => $path,
    'SaveAs'     => $newPath,

    // Can also supply BucketId instead of BucketName
    // Optional are DestinationBucketName or DestinationBucketId
]);
File Delete
$fileDelete = $client->deleteFile([
    'FileId' => $file->getId()

    // Can also identify the file via bucket and path:
    // 'BucketName' => 'my-special-bucket',
    // 'FileName' => 'path/to/file'
]);
List all files
$fileList = $client->listFiles([
    'BucketId' => 'YOUR_BUCKET_ID'
]);
Change log
Please see CHANGELOG for more information what has changed recently.

Testing
$ vendor/bin/phpunit
Contributing
Please see CONTRIBUTING and CONDUCT for details.

Security
If you discover any security related issues, please email atonujekemena@gmail.com instead of using the issue tracker.

Credits
All Contributors
License
The MIT License (MIT). Please see License File for more information.



Laravel usage
## Install

Via Composer

``` bash
composer require mwebdev/backblaze-b2
```
In your app.php config file add to the list of service providers:

``` php
\Gliterd\BackblazeB2\BackblazeB2ServiceProvider::class,
```
Add the following to your filesystems.php config file in the disks section:

``` php
'b2' => [
    'driver'         => 'b2',
    'accountId'      => '',
    'applicationKey' => '',
    'bucketName'     => '',
],
```

## Usage

Just use it as you normally would use the Storage facade.

``` php
\Storage::disk('b2')->put('filename.txt', 'My important content');
```
and
``` php
\Storage::disk('b2')->get('filename.txt')
```
