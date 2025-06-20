# Laravel Agora Token Generator - Development Guidelines

This document provides essential information for developers working on the Laravel Agora Token Generator package.

## Build/Configuration Instructions

### Installation

1. Install the package via Composer:
   ```bash
   composer require tomatophp/laravel-agora
   ```

2. The package will automatically register its service provider if you're using Laravel's package auto-discovery.

### Configuration

1. Publish the configuration file:
   ```bash
   php artisan vendor:publish --tag=laravel-agora-config
   ```

2. Configure your Agora credentials in your `.env` file:
   ```
   AGORA_APP_ID=your-app-id
   AGORA_APP_CERTIFICATE=your-app-certificate
   ```

## Testing Information

### Setting Up Tests

1. The package uses PHPUnit for testing. Ensure PHPUnit is installed in your Laravel application.

2. When testing this package within a Laravel application, you can use Laravel's testing utilities to mock the configuration:

   ```php
   // In your test
   Config::set('laravel-agora.agora.app_id', 'test-app-id');
   Config::set('laravel-agora.agora.app_certificate', 'test-app-certificate');
   ```

### Running Tests

To run the tests for this package within a Laravel application:

```bash
php artisan test --filter=AgoraTest
```

### Adding New Tests

1. Create test files in the `tests/Unit` or `tests/Feature` directories depending on the type of test.

2. For unit tests, extend `PHPUnit\Framework\TestCase`.

3. For feature tests that require Laravel's testing utilities, extend `Tests\TestCase`.

4. Example of a simple unit test:

```php
<?php

namespace Tests\Unit;

use PHPUnit\Framework\TestCase;
use TomatoPHP\LaravelAgora\Services\Agora;

class AgoraTest extends TestCase
{
    public function testTokenGeneration()
    {
        // Mock the config values
        // In a real Laravel application, use:
        // Config::set('laravel-agora.agora.app_id', 'test-app-id');
        // Config::set('laravel-agora.agora.app_certificate', 'test-app-certificate');
        
        // Create an instance of the Agora service
        $agora = Agora::make(1)
            ->channel('test-channel')
            ->uId('test-user')
            ->join(false)
            ->audioOnly(false);
        
        // Generate a token
        $token = $agora->token();
        
        // Assert that the token is a string and not empty
        $this->assertIsString($token);
        $this->assertNotEmpty($token);
    }
}
```

## Additional Development Information

### Code Style

This package follows the PSR-12 coding standard. Ensure your code adheres to this standard before submitting pull requests.

### Service Architecture

The package uses a fluent interface pattern for configuring and generating Agora tokens:

1. `Agora` - The main service class that provides a fluent interface for configuring token generation.
2. `RtcTokenBuilder` - Builds RTC tokens with specific privileges based on the role.
3. `AccessToken` - Handles the low-level token generation and validation.
4. `Message` - Manages the message content for token generation.

### Usage Examples

Basic token generation:

```php
use TomatoPHP\LaravelAgora\Services\Agora;

// Generate a token for a publisher
$token = Agora::make($userId)
    ->channel('channel-name')
    ->uId('user-id')
    ->join(false) // false for publisher, true for subscriber
    ->audioOnly(false) // false for video+audio, true for audio only
    ->token();
```

### Debugging

If you encounter issues with token generation, check the following:

1. Ensure your Agora credentials are correctly set in the configuration.
2. Verify that the channel name and user ID are valid.
3. Check the role and media type settings to ensure they match your requirements.

### Common Issues

1. **Token validation fails**: Ensure the app ID and app certificate match between token generation and validation.
2. **Token expires too quickly**: The default expiration is 24 hours. Modify the `Message` class if you need a different expiration time.