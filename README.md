# Forge Moderation System - Документация

## Содержание
- [О системе](#о-системе)
- [Установка](#установка)
- [Конфигурация](#конфигурация)
- [Команды](#команды)
  - [Команда Ban](#команда-ban)
  - [Команда Warn](#команда-warn)
  - [Команда Demorgan](#команда-demorgan)
  - [Команда CC (CheckCheat)](#команда-cc-checkcheat)
- [Права доступа](#права-доступа)
- [База данных](#база-данных)
- [Интеграция с Discord](#интеграция-с-discord)
- [Устранение неполадок](#устранение-неполадок)
- [FAQ](#faq)

## О системе

Forge Moderation System - это комплексная система модерации для серверов Unturned, предоставляющая администраторам полный набор инструментов для эффективного управления игроками и борьбы с нарушителями. Система включает в себя:

- **Система банов** с поддержкой временных и перманентных банов
- **Система предупреждений** с автоматическими наказаниями
- **Система деморгана** (тюрьма) для временной изоляции нарушителей
- **Система проверки на читы** с возможностью вызова игрока на проверку
- **Защита от обхода банов** через отслеживание HWID и IP
- **Автоматические наказания** на основе количества предупреждений
- **Ведение истории наказаний** для каждого игрока

## Установка

1. Скачайте последнюю версию Forge Moderation System из [официального репозитория](https://github.com/forge-moderation-system).
2. Распакуйте архив в директорию `Rocket/Plugins` на вашем сервере Unturned.
3. Перезапустите сервер для создания конфигурационных файлов.
4. Настройте конфигурацию в файле `Rocket/Plugins/ForgeModeration/Config.xml`.

## Конфигурация

После первого запуска плагина создается файл конфигурации `Config.xml`. Вот основные параметры, которые можно настроить:

```xml
<EnableGlobalNotifications>true</EnableGlobalNotifications> <!-- Включение/выключение глобальных уведомлений о наказаниях -->
<DefaultWarnDuration>604800</DefaultWarnDuration> <!-- Стандартная длительность предупреждения в секундах (604800 = 7 дней) -->
<EnableWarnNotifications>true</EnableWarnNotifications> <!-- Уведомлять игроков о получении предупреждения -->
<EnableAutoActions>true</EnableAutoActions> <!-- Включить автоматические наказания за накопление предупреждений -->

<!-- Автоматические наказания за предупреждения -->
<WarnPunishments>
  <Punishment warnsCount="3" type="Ban" Duration="86400">
    <Message>Вы были автоматически забанены из-за накопления 3 предупреждений.</Message>
    <LogToConsole>true</LogToConsole>
  </Punishment>
  <!-- Можно добавить другие наказания -->
</WarnPunishments>

<!-- Настройки деморгана -->
<DemorganPosition>0,0,0</DemorganPosition> <!-- Координаты деморгана, необходимо установить через команду -->
<DemorganRadius>15.0</DemorganRadius> <!-- Радиус деморгана в метрах -->
<DefaultDemorganDuration>1800</DefaultDemorganDuration> <!-- Стандартная длительность деморгана в секундах (1800 = 30 минут) -->
<EnableDemorganNotifications>true</EnableDemorganNotifications> <!-- Уведомлять игроков о помещении в деморган -->
<EnableDemorganTeleport>true</EnableDemorganTeleport> <!-- Телепортировать игроков в деморган -->

<!-- Настройки проверки на читы -->
<CheckCheatsEffectID>20000</CheckCheatsEffectID> <!-- ID UI эффекта для проверки на читы -->
<CheckCheatsEffectKey>20000</CheckCheatsEffectKey> <!-- Ключ UI эффекта для проверки на читы -->
<CheckCheatsTimeoutSeconds>300</CheckCheatsTimeoutSeconds> <!-- Время на ответ игрока при проверке (5 минут) -->
<CheckCheatsBanDuration>43200</CheckCheatsBanDuration> <!-- Длительность бана за отказ от проверки (12 часов) -->
<CheckCheatsBanReason>Отказ от проверки на читы</CheckCheatsBanReason> <!-- Причина бана за отказ от проверки -->
```

### Настройка точки деморгана

Для работы функции деморгана необходимо установить точку деморгана через команду:
```
/demorgan setpoint
```

Также можно настроить точку освобождения из деморгана:
```
/demorgan setrelease
```

## Команды

### Команда Ban

Команда `/ban` используется для управления банами игроков.

| Подкоманда | Синтаксис | Описание |
|------------|-----------|----------|
| add | `/ban add <игрок> <причина> [срок]` | Забанить игрока |
| remove | `/ban remove <игрок/steamID> [причина]` | Разбанить игрока |
| info | `/ban info <игрок/steamID>` | Получить информацию о бане игрока |
| list | `/ban list [страница]` | Посмотреть список активных банов |
| history | `/ban history <игрок/steamID> [страница]` | Посмотреть историю банов игрока |

**Примеры использования:**
```
/ban add MrGriefer Использование читов
/ban add Notch Гриферство 3d
/ban remove 76561198012345678 Ошибочный бан
/ban info MrGriefer
/ban list 2
/ban history MrGriefer
```

**Форматы сроков:**
- `s` - секунды
- `m` - минуты
- `h` - часы
- `d` - дни
- `w` - недели
- `M` - месяцы
- `y` - годы

Если срок не указан или указан `0`/`perm`, бан будет перманентным.

### Команда Warn

Команда `/warn` используется для управления предупреждениями игроков.

| Подкоманда | Синтаксис | Описание |
|------------|-----------|----------|
| add | `/warn add <игрок> <причина> [срок]` | Выдать предупреждение игроку |
| remove | `/warn remove <игрок/steamID> <ID предупреждения> [причина]` | Снять предупреждение |
| info | `/warn info <игрок/steamID>` | Получить информацию о предупреждениях игрока |
| list | `/warn list [страница]` | Посмотреть список игроков с активными предупреждениями |
| history | `/warn history <игрок/steamID> [страница]` | Посмотреть историю предупреждений игрока |
| my | `/warn my [страница]` | Посмотреть собственные предупреждения |

**Примеры использования:**
```
/warn add MrTroll Спам в чате
/warn add Notch Нарушение правил сервера 5d
/warn remove MrTroll 152 Прошло достаточно времени
/warn info Notch
/warn list
/warn my
```

### Команда Demorgan

Команда `/demorgan` используется для управления системой тюрьмы (деморган).

| Подкоманда | Синтаксис | Описание |
|------------|-----------|----------|
| add | `/demorgan add <игрок> <причина> [срок]` | Поместить игрока в деморган |
| remove | `/demorgan remove <игрок/steamID> [причина]` | Освободить игрока из деморгана |
| info | `/demorgan info <игрок/steamID>` | Получить информацию о деморгане игрока |
| list | `/demorgan list [страница]` | Посмотреть список игроков в деморгане |
| setpoint | `/demorgan setpoint` | Установить точку деморгана |
| setrelease | `/demorgan setrelease` | Установить точку освобождения из деморгана |
| setradius | `/demorgan setradius <радиус>` | Установить радиус деморгана |
| tp | `/demorgan tp` | Телепортироваться к точке деморгана |

**Примеры использования:**
```
/demorgan add MrToxic Нарушение правил чата 30m
/demorgan remove MrToxic Обещал исправиться
/demorgan info MrToxic
/demorgan setpoint
/demorgan setradius 20
```

### Команда CC (CheckCheat)

Команда `/cc` используется для проверки игроков на читы.

| Подкоманда | Синтаксис | Описание |
|------------|-----------|----------|
| start | `/cc start <игрок> [discord]` | Отправить игрока на проверку |
| end | `/cc end <игрок>` | Завершить проверку |
| pause | `/cc pause <игрок>` | Приостановить таймер проверки |
| resume | `/cc resume <игрок>` | Возобновить таймер проверки |
| discord | `/cc discord <игрок> <discord>` | Изменить контакт Discord |
| result | `/cc result <игрок> <pass/fail> [причина]` | Установить результат проверки |

**Примеры использования:**
```
/cc start MrSuspicious MrAdmin#1234
/cc pause MrSuspicious
/cc resume MrSuspicious
/cc discord MrSuspicious MrAdmin#4321
/cc result MrSuspicious pass
/cc result MrSuspicious fail Отказ показать Processes
```

## Права доступа

Система модерации использует следующие права доступа:

| Право | Описание |
|-------|----------|
| `forge.moderation.ban` | Доступ к команде `/ban` |
| `forge.moderation.ban.add` | Возможность банить игроков |
| `forge.moderation.ban.remove` | Возможность разбанивать игроков |
| `forge.moderation.ban.info` | Возможность просматривать информацию о банах |
| `forge.moderation.ban.list` | Возможность просматривать список банов |
| `forge.moderation.ban.history` | Возможность просматривать историю банов |
| `forge.moderation.warn` | Доступ к команде `/warn` |
| `forge.moderation.warn.add` | Возможность выдавать предупреждения |
| `forge.moderation.warn.remove` | Возможность снимать предупреждения |
| `forge.moderation.warn.info` | Возможность просматривать информацию о предупреждениях |
| `forge.moderation.warn.list` | Возможность просматривать список предупреждений |
| `forge.moderation.warn.history` | Возможность просматривать историю предупреждений |
| `forge.moderation.demorgan` | Доступ к команде `/demorgan` |
| `forge.moderation.demorgan.add` | Возможность помещать игроков в деморган |
| `forge.moderation.demorgan.remove` | Возможность освобождать игроков из деморгана |
| `forge.moderation.demorgan.info` | Возможность просматривать информацию о деморгане |
| `forge.moderation.demorgan.list` | Возможность просматривать список игроков в деморгане |
| `forge.moderation.demorgan.setpoint` | Возможность устанавливать точку деморгана |
| `forge.moderation.demorgan.setradius` | Возможность устанавливать радиус деморгана |
| `forge.moderation.cc` | Доступ к команде `/cc` |
| `forge.moderation.cc.start` | Возможность отправлять игроков на проверку |
| `forge.moderation.cc.end` | Возможность завершать проверку |
| `forge.moderation.cc.pause` | Возможность приостанавливать таймер проверки |
| `forge.moderation.cc.resume` | Возможность возобновлять таймер проверки |
| `forge.moderation.cc.discord` | Возможность изменять контакт Discord |
| `forge.moderation.cc.result` | Возможность устанавливать результат проверки |
| `forge.moderation.cc.endany` | Возможность завершать любую проверку (даже чужую) |

## База данных

Система модерации использует JSON для хранения данных. Данные хранятся в директории `Rocket/Plugins/ForgeModeration/Database/`.

### Структура данных

- `players.json` - основной файл с информацией об игроках и их наказаниях

Резервные копии создаются автоматически и хранятся в директории `Rocket/Plugins/ForgeModeration/Database/Backups/`.

## Интеграция с Discord

Forge Moderation System поддерживает отправку уведомлений в Discord-канал через веб-хуки. Для настройки нужно добавить в конфигурацию следующий блок:

```xml
<DiscordWebhook>
  <Enabled>true</Enabled>
  <WebhookURL>https://discord.com/api/webhooks/your_webhook_url</WebhookURL>
  <ServerName>Мой Сервер Unturned</ServerName>
  <NotifyBans>true</NotifyBans>
  <NotifyWarns>true</NotifyWarns>
  <NotifyDemorgans>true</NotifyDemorgans>
  <NotifyCheckResults>true</NotifyCheckResults>
</DiscordWebhook>
```

## Устранение неполадок

### Общие проблемы

**Проблема**: Не работает прогресс-бар при проверке на читы.  
**Решение**: Проверьте правильность значений `CheckCheatsEffectID` и `CheckCheatsEffectKey` в конфигурации. Они должны соответствовать ID и ключу UI-эффекта.

**Проблема**: Ошибка "Не удалось получить HWID игрока".  
**Решение**: Эта ошибка может возникать на локальных серверах или при тестировании. Система будет использовать SteamID в качестве резервного идентификатора.

**Проблема**: Игроки не телепортируются в деморган.  
**Решение**: Убедитесь, что вы установили точку деморгана с помощью команды `/demorgan setpoint` и параметр `EnableDemorganTeleport` в конфигурации установлен в `true`.

**Проблема**: Ошибка "Database manager not initialized".  
**Решение**: Перезапустите сервер. Если проблема повторяется, проверьте права доступа к папке с базой данных.

### Логи

Система модерации записывает подробные логи в стандартный лог-файл Rocket. Проверьте файл `Rocket.log` для диагностики проблем.

## FAQ

### Как настроить автоматические наказания за предупреждения?

Настройте блок `WarnPunishments` в конфигурации. Пример:

```xml
<WarnPunishments>
  <Punishment warnsCount="1" type="Message">
    <Message>Вы получили предупреждение. При достижении 3 предупреждений вы будете временно забанены.</Message>
    <LogToConsole>true</LogToConsole>
  </Punishment>
  <Punishment warnsCount="3" type="Ban" Duration="86400">
    <Message>Вы были автоматически забанены из-за накопления 3 предупреждений.</Message>
    <Reason>Автоматический бан за 3 предупреждения</Reason>
    <LogToConsole>true</LogToConsole>
  </Punishment>
  <Punishment warnsCount="5" type="Ban" Duration="604800">
    <Message>Вы были автоматически забанены из-за накопления 5 предупреждений.</Message>
    <Reason>Автоматический бан за 5 предупреждений</Reason>
    <LogToConsole>true</LogToConsole>
  </Punishment>
</WarnPunishments>
```

### Как настроить систему деморгана?

1. Создайте специальное место для деморгана на вашей карте.
2. Установите точку деморгана, встав в нужное место и выполнив команду `/demorgan setpoint`.
3. Установите радиус деморгана командой `/demorgan setradius <радиус>`.
4. (Опционально) Установите точку освобождения командой `/demorgan setrelease`.

### Как работает система обнаружения обхода бана?

Система отслеживает IP-адреса и HWID игроков. Если игрок пытается зайти с нового аккаунта, но использует тот же IP или HWID, что и ранее забаненный аккаунт, система обнаружит это и применит бан.

### Как увеличить время для проверки на читы?

Измените параметр `CheckCheatsTimeoutSeconds` в конфигурации:

```xml
<CheckCheatsTimeoutSeconds>600</CheckCheatsTimeoutSeconds> <!-- 10 минут вместо 5 -->
```

### Могут ли обычные игроки видеть свои предупреждения?

Да, любой игрок может использовать команду `/warn my` для просмотра своих активных предупреждений.

---

**© Forge Development Team, 2025**  
Документация предоставлена для версии 1.0.0 Forge Moderation System
