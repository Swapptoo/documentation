# Исправление проблем

## Проверка журнала

Чтобы узнать о проблеме, вам необходимо проверить файлы журнала ошибок.

#### Журналы ошибок EspoCRM

Журналы EspoCRM расположены в `<ESPOCRM_DIRECTORY> /logs/*.log` и содержат некоторую информацию об ошибке.

#### Журналы ошибок Apache

Для сервера Ubuntu журнал ошибок apache находится в `/var/log/apache2/error.log` и содержит всю информацию об ошибках. Расположение файлов журналов может отличаться в других системах.
## Проверьте системные требования (system requirements)

В разделе Администрирование > Системные требования. Важно установить все необходимые расширения.

## Запланированные действия не работают

#### Проблема №1: ваш crontab не настроен

1. Войдите в систему через SSH на свой сервер.

2. Настройте свой crontab, выполнив следующие [шаги](server-configuration.md#user-content-настройка-crontab)

Примечание. Crontab должен быть настроен под пользователем веб-сервера, например. `crontab -e -u www-data`.

3. Подождите некоторое время и проверьте Планировщик заданий, чтобы узнать, выполнены ли какие-либо задания (см. Журнал).

#### Проблема №2. Crontab настроен, но запланированные задания не работают

Чтобы убедиться, что при запуске cron нет ошибок, попробуйте запустить команду cron в терминале:

1. Войдите в систему через SSH на свой сервер.

2. Перейдите в каталог, где установлен EspoCRM. Например. для каталога `/var/www/html/espocrm` команда:

```bash
cd /var/www/html/espocrm
```

3. Запустите команду crontab:

```bash
php cron.php
```

Примечание. Он должен выполняться под пользователем веб-сервера. Если вы вошли в систему под учетной записью root, команда должна быть (например, для Ubuntu):

```bash
sudo -u www-data php cron.php
```

где `www-data` это сервер пользователя.

4. Если ошибок нет, проверьте Планировщик заданий, чтобы узнать, выполнено ли какое-либо задание (см. Журнал).

## Запуск перестройки из CLI (Running rebuild from CLI)

Иногда вам нужно запустить перестройку (rebuild) из интерфейса командной строки, когда приложение не загружается.

```bash
sudo -u www-data php cron.php
```

где `www-data` это пользователь веб-сервера.

## EspoCRM не загружается после обновления

Иногда это может происходить на некоторых общих хостах.

Проверьте разрешения файлов: /index.php /api/v1/index.php

Они должны быть 644. Если какой-либо из этих файлов имеет разрешение 664, вам необходимо изменить его на 644. Используйте панель управления хостинга или команду chmod.

```
chmod 644 /path/to/file
```
Дополнительная информация о файловых разрешениях: [здесь](server-configuration.md#user-content-требуемые-разрешения-для-систем-на-основе-unix).

## Ошибка MySQL: сервер запросил метод аутентификации, неизвестный клиенту

MySQL 8.0.4 изменил метод аутентификации по умолчанию на caching_sha2_password, который не поддерживается PHP. Эта проблема может быть решена с помощью этого [решения](server-configuration.md#user-content-поддержка-mysql-8).

## Электронные письма не доставляются

1. Убедитесь, что [cron](server-configuration.md#user-content-настройка-crontab) работает. Вы увидите уведомление об ошибке на главной странице администрирования, если cron не запущен.
2. Проверьте журнал EspoCRM (data/logs) и журналы сервера (server logs) на наличие ошибок.
3. Проверьте журнал (log) в разделе Администрирование > Планировщик заданий > Проверьте Personal Email Accounts. Убедитесь, что нет записей с статусом failed.
4. Проверьте журнал (log) в разделе Администрирование > Планировщик заданий > Проверьте Group Email Accounts. Убедитесь, что нет записей с статусом failed. 

## Включить режим отладки

Чтобы включить режим отладки, перейдите в установленный каталог EspoCRM, откройте файл `data/config.php` и измените значение:

```
'logger' => [
    ...
    'level' => 'WARNING',
    ...
]
```
to
```
'logger' => [
    ...
    'level' => 'DEBUG',
    ...
]
```
