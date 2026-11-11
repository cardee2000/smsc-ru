# SmscRu notifications channel for Laravel

SmscRu notifications channel for Laravel 5.3+ under the `cardee` vendor, including Laravel 13+.

Namespace `NotificationChannels\SmscRu` is preserved for drop-in use.

[![Latest Version on Packagist](https://img.shields.io/packagist/v/cardee/smsc-ru.svg?style=flat-square)](https://packagist.org/packages/cardee/smsc-ru)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Total Downloads](https://img.shields.io/packagist/dt/cardee/smsc-ru.svg?style=flat-square)](https://packagist.org/packages/cardee/smsc-ru)

This package makes it easy to send notifications using [smsc.ru](https://smsc.ru) (aka СМС–Центр) with Laravel 5.3+.

## Contents

- [Installation](#installation)
    - [Setting up the SmscRu service](#setting-up-the-SmscRu-service)
- [Usage](#usage)
    - [Available Message methods](#available-methods)
- [Changelog](#changelog)
- [Testing](#testing)
- [Security](#security)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)


## Installation

Install this package with Composer:

```bash
composer require cardee/smsc-ru
```

Version guide:

| Package | Laravel |
|---|---|
| `^3.0` | 5.1–7 |
| `^3.1` / `^3.2` | 5.1–8 |
| `^3.3` | 5.1–9 |
| `^3.4` | 5.1–10 |
| `^3.5` | 5.1–11 |
| `^3.6` | 5.1–12 |
| `^3.7` | 5.1–13 |


The service provider gets loaded automatically. Or you can do this manually:
```php
// config/app.php
'providers' => [
    ...
    NotificationChannels\SmscRu\SmscRuServiceProvider::class,
],
```

### Setting up the SmscRu service

Add your SmscRu login, secret key (hashed password) and default sender name (or phone number) to your `config/services.php`:

```php
// config/services.php
...
'smscru' => [
    'login'  => env('SMSCRU_LOGIN'),
    'secret' => env('SMSCRU_SECRET'),
    'sender' => 'John_Doe',
    'extra'  => [
        // any other API parameters
        // 'tinyurl' => 1
    ],
],
...
```

> If you want use other host than `smsc.ru`, you MUST set custom host WITH trailing slash.

```
// .env
...
SMSCRU_HOST=http://www1.smsc.kz/
...
```

```php
// config/services.php
...
'smscru' => [
    ...
    'host' => env('SMSCRU_HOST'),
    ...
],
...
```

## Usage

You can use the channel in your `via()` method inside the notification:

```php
use Illuminate\Notifications\Notification;
use NotificationChannels\SmscRu\SmscRuMessage;
use NotificationChannels\SmscRu\SmscRuChannel;

class AccountApproved extends Notification
{
    public function via($notifiable)
    {
        return [SmscRuChannel::class];
    }

    public function toSmscRu($notifiable)
    {
        return SmscRuMessage::create("Task #{$notifiable->id} is complete!");
    }
}
```

In your notifiable model, make sure to include a `routeNotificationForSmscru()` method, which returns a phone number
or an array of phone numbers.

```php
public function routeNotificationForSmscru()
{
    return $this->phone;
}
```

### Available methods

`from()`: Sets the sender's name or phone number.

`content()`: Set a content of the notification message.

`sendAt()`: Set a time for scheduling the notification message.

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Security

If you discover any security related issues, please email cardee@mifesta.com instead of using the issue tracker.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

- [Sergei Gruzdov](https://github.com/cardee2000)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
