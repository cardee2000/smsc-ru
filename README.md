# SmscRu notifications channel for Laravel

SmscRu notifications channel for Laravel 5.3+, including Laravel 13+.

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
- [Русский](#русский)


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

If you discover any security-related issues, please open a GitHub issue instead of using email.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

- [Sergei Gruzdov](https://github.com/cardee2000)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

---

## Русский

# Канал уведомлений SmscRu для Laravel

Канал уведомлений SmscRu для Laravel 5.3+, включая Laravel 13+.

Пространство имён `NotificationChannels\SmscRu` сохранено для совместимости.

Пакет упрощает отправку уведомлений через [smsc.ru](https://smsc.ru) (СМС‑Центр) в Laravel 5.3+.

## Установка

Установите пакет через Composer:

```bash
composer require cardee/smsc-ru
```

Соответствие версий:

| Пакет | Laravel |
|---|---|
| `^3.0` | 5.1–7 |
| `^3.1` / `^3.2` | 5.1–8 |
| `^3.3` | 5.1–9 |
| `^3.4` | 5.1–10 |
| `^3.5` | 5.1–11 |
| `^3.6` | 5.1–12 |
| `^3.7` | 5.1–13 |

Сервис-провайдер подключается автоматически. При необходимости можно указать его вручную:

```php
// config/app.php
'providers' => [
    ...
    NotificationChannels\SmscRu\SmscRuServiceProvider::class,
],
```

### Настройка сервиса SmscRu

Добавьте логин SmscRu, секретный ключ (хеш пароля) и имя отправителя по умолчанию (или номер телефона) в `config/services.php`:

```php
// config/services.php
...
'smscru' => [
    'login'  => env('SMSCRU_LOGIN'),
    'secret' => env('SMSCRU_SECRET'),
    'sender' => 'John_Doe',
    'extra'  => [
        // любые другие параметры API
        // 'tinyurl' => 1
    ],
],
...
```

> Если нужен хост, отличный от `smsc.ru`, укажите его **со слэшем в конце**.

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

## Использование

Укажите канал в методе `via()` уведомления:

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

В модели-получателе добавьте метод `routeNotificationForSmscru()`, который возвращает номер телефона или массив номеров.

```php
public function routeNotificationForSmscru()
{
    return $this->phone;
}
```

### Доступные методы

`from()`: задаёт имя или номер отправителя.

`content()`: задаёт текст уведомления.

`sendAt()`: задаёт время отложенной отправки.

## Changelog

См. [CHANGELOG](CHANGELOG.md).

## Тесты

``` bash
$ composer test
```

## Безопасность

О проблемах безопасности создавайте issue в GitHub, а не пишите на email.

## Участие в разработке

См. [CONTRIBUTING](CONTRIBUTING.md).

## Авторы

- [Sergei Gruzdov](https://github.com/cardee2000)

## Лицензия

The MIT License (MIT). Подробнее — в [License File](LICENSE.md).
