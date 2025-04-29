# Forge Moderation System

Система модерации для Unturned на базе RocketMod. Предоставляет расширенные инструменты для администрирования сервера, включая управление банами, предупреждениями и проверку игроков на читы.

## Основные функции

- **Система банов:** временные и перманентные баны с детальной информацией и историей
- **Система предупреждений:** автоматическое наказание при накоплении предупреждений
- **Проверка на читы:** удобный UI для проверки игроков с интеграцией Discord
- **Обнаружение обхода бана:** определение попыток обхода бана по IP и HWID
- **Интеграция с Discord:** уведомления о модерационных действиях через webhook
- **Детальная база данных:** история всех модерационных действий по каждому игроку

## Команды

### Управление банами

| Команда | Описание | Права |
|---------|----------|-------|
| `/ban add <игрок/steamID> <причина> [срок]` | Выдать бан игроку | forge.moderation.ban.add |
| `/ban remove <игрок/steamID> [причина]` | Снять бан с игрока | forge.moderation.ban.remove |
| `/ban info <игрок/steamID>` | Информация о бане игрока | forge.moderation.ban.info |
| `/ban list [страница]` | Список активных банов | forge.moderation.ban.list |
| `/ban history <игрок/steamID> [страница]` | История банов игрока | forge.moderation.ban.history |

#### Форматы срока бана
- `s` - секунды, `m` - минуты, `h` - часы, `d` - дни, `w` - недели, `M` - месяцы, `y` - годы
- Пример: `7d` = 7 дней, `12h` = 12 часов, `30m` = 30 минут
- Перманентный бан: оставьте поле пустым или укажите `0`

#### Примеры использования
```
/ban add Notch читы
/ban add 76561198012345678 гриферство 7d
/ban remove Notch Ошибка администратора
/ban info 76561198012345678
```

### Управление предупреждениями

| Команда | Описание | Права |
|---------|----------|-------|
| `/warn add <игрок/steamID> <причина> [срок]` | Выдать предупреждение | forge.moderation.warn.add |
| `/warn remove <игрок/steamID> <ID> [причина]` | Снять предупреждение | forge.moderation.warn.remove |
| `/warn info <игрок/steamID>` | Информация о предупреждениях | forge.moderation.warn.info |
| `/warn list [страница]` | Список игроков с активными предупреждениями | forge.moderation.warn.list |
| `/warn history <игрок/steamID> [страница]` | История предупреждений игрока | forge.moderation.warn.history |
| `/warn my [страница]` | Просмотр собственных предупреждений | forge.moderation.warn |

#### Примеры использования
```
/warn add Notch нарушение правил чата
/warn add 76561198012345678 спам 3d
/warn remove Notch 15 Ошибка администратора
/warn info 76561198012345678
```

### Проверка на читы

| Команда | Описание | Права |
|---------|----------|-------|
| `/cc start <игрок> [discord]` | Начать проверку на читы | forge.moderation.cc.start |
| `/cc end <игрок>` | Завершить проверку | forge.moderation.cc.end |
| `/cc pause <игрок>` | Приостановить проверку (таймер не идёт) | forge.moderation.cc.pause |
| `/cc resume <игрок>` | Возобновить проверку | forge.moderation.cc.resume |
| `/cc discord <игрок> <discord>` | Изменить Discord для проверки | forge.moderation.cc.discord |
| `/cc result <игрок> <pass\|fail> [причина]` | Установить результат проверки | forge.moderation.cc.result |

#### Дополнительные права для проверки
- `forge.moderation.cc.endany` - завершение проверки, начатой другим администратором
- `forge.moderation.cc.pauseany` - приостановка проверки, начатой другим администратором
- `forge.moderation.cc.resumeany` - возобновление проверки, начатой другим администратором
- `forge.moderation.cc.discordany` - изменение Discord для проверки, начатой другим администратором
- `forge.moderation.cc.resultany` - установка результата для проверки, начатой другим администратором

#### Примеры использования
```
/cc start Notch notch#1234
/cc discord Notch notch#5678
/cc pause Notch
/cc resume Notch
/cc result Notch pass
/cc result Notch fail Использование чит-программы Vape
/cc end Notch
```

### Алиасы команд
- `/ban` и `/bans` - управление банами
- `/warn` и `/warns` - управление предупреждениями
- `/cc` и `/checkcheat` - проверка на читы

## Настройка

Система модерации использует файл конфигурации с настройками:

```xml
<DiscordWebhookUrl>URL Discord вебхука</DiscordWebhookUrl>
<ServerName>Название сервера</ServerName>
<EnableDiscordNotifications>true</EnableDiscordNotifications>
<EnableGlobalNotifications>true</EnableGlobalNotifications>
<DefaultWarnDuration>604800</DefaultWarnDuration>
<EnableWarnNotifications>true</EnableWarnNotifications>
<EnableAutoActions>true</EnableAutoActions>
```

## Примечания

- Для идентификации игрока можно использовать имя персонажа, имя Steam или SteamID
- Система автоматически обнаруживает и предотвращает попытки обхода бана
- Предупреждения могут автоматически приводить к бану при достижении определенного количества
- Игрок, не прошедший проверку на читы или отказавшийся от неё, получает перманентный бан
- Все модерационные действия логируются и могут быть отправлены в Discord (при настройке вебхука)
