# Laravel Agora Token Generator

Agora Token Generator With easy to use Service Class for Laravel applications.

## Requirements

- PHP 8.0.2 or higher
- Laravel 8.x, 9.x, 10.x, 11.x, or 12.x

## Installation

### From Packagist

```bash
composer require cyberdeep/laravel-agora-token-generator:^1.0
```

If you encounter stability issues, you can explicitly allow dev stability:

```bash
composer require cyberdeep/laravel-agora-token-generator:^1.0 --with-all-dependencies
```

### Local Installation

For instructions on how to install this package locally during development, please
see [LOCAL_INSTALLATION.md](LOCAL_INSTALLATION.md).

## Configuration

After installing the package, publish the configuration file:

```bash
php artisan vendor:publish --tag=laravel-agora-token-generator-config
```

Then, add your Agora credentials to your `.env` file:

```
AGORA_APP_ID=your-app-id
AGORA_APP_CERTIFICATE=your-app-certificate
```

## Usage

```php
use CyberDeep\LaravelAgoraTokenGenerator\Services\Agora;

// Generate a token for a publisher
$token = Agora::make($userId)
    ->channel('channel-name')
    ->uId('user-id')
    ->join(false) // false for publisher, true for subscriber
    ->audioOnly(false) // false for video+audio, true for audio only
    ->token();
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Security

Please see [SECURITY](SECURITY.md) for more information about security.

## Credits

- [Fady Mondy](mailto:info@3x1.io)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
