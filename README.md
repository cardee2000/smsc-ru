# Канал уведомлений SmscRu для Laravel

Канал уведомлений SmscRu для Laravel 5.3+, включая Laravel 13+.

Пространство имён `NotificationChannels\SmscRu` сохранено для совместимости.

[![Latest Version on Packagist](https://img.shields.io/packagist/v/cardee/smsc-ru.svg?style=flat-square)](https://packagist.org/packages/cardee/smsc-ru)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Total Downloads](https://img.shields.io/packagist/dt/cardee/smsc-ru.svg?style=flat-square)](https://packagist.org/packages/cardee/smsc-ru)

Пакет упрощает отправку уведомлений через [smsc.ru](https://smsc.ru) (СМС‑Центр) в Laravel 5.3+.

## Содержание

- [Установка](#установка)
    - [Настройка сервиса SmscRu](#настройка-сервиса-smscru)
- [Использование](#использование)
    - [Доступные методы сообщения](#доступные-методы)
- [Changelog](#changelog)
- [Тесты](#тесты)
- [Безопасность](#безопасность)
- [Участие в разработке](#участие-в-разработке)
- [Авторы](#авторы)
- [Лицензия](#лицензия)


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

В модели-получателе добавьте метод `routeNotificationForSmscru()`, который возвращает номер телефона
или массив номеров.

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

О проблемах безопасности пишите на cardee@mifesta.com, а не через issue tracker.

## Участие в разработке

См. [CONTRIBUTING](CONTRIBUTING.md).

## Авторы

- [Sergei Gruzdov](https://github.com/cardee2000)

## Лицензия

The MIT License (MIT). Подробнее — в [License File](LICENSE.md).
