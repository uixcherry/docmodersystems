# Forge.ModerationSystem - Документация

## Содержание
1. [Введение](#введение)
2. [Разрешения (Permissions)](#разрешения-permissions)
3. [Система банов](#система-банов)
4. [Система предупреждений (варны)](#система-предупреждений-варны)
5. [Система мутов](#система-мутов)
6. [Система проверки на читы](#система-проверки-на-читы)
7. [Конфигурация](#конфигурация)
8. [Discord-интеграция](#discord-интеграция)

## Введение

Forge.ModerationSystem - это комплексная система модерации для Unturned серверов, включающая:
- Управление банами с отслеживанием попыток обхода
- Систему предупреждений с автоматическими наказаниями
- Управление мутами голосового и текстового чата
- Инструмент проверки игроков на читы с интерактивным UI
- Интеграцию с Discord для уведомлений о действиях модерации
- Сохранение полной истории действий модерации

## Разрешения (Permissions)

### Основные разрешения
- `forge.moderation` - Базовое разрешение, доступ к получению уведомлений
- `forge.moderation.ban` - Доступ к командам бана
- `forge.moderation.warn` - Доступ к командам предупреждений
- `forge.moderation.mute` - Доступ к командам мута
- `forge.moderation.cc` - Доступ к командам проверки на читы

### Расширенные разрешения для банов
- `forge.moderation.ban.add` - Выдача банов
- `forge.moderation.ban.remove` - Снятие банов
- `forge.moderation.ban.info` - Просмотр информации о банах
- `forge.moderation.ban.list` - Просмотр списка активных банов
- `forge.moderation.ban.history` - Просмотр истории банов

### Расширенные разрешения для предупреждений
- `forge.moderation.warn.add` - Выдача предупреждений
- `forge.moderation.warn.remove` - Снятие предупреждений
- `forge.moderation.warn.info` - Просмотр информации о предупреждениях
- `forge.moderation.warn.list` - Просмотр списка активных предупреждений
- `forge.moderation.warn.history` - Просмотр истории предупреждений

### Расширенные разрешения для мутов
- `forge.moderation.mute.add` - Выдача мутов
- `forge.moderation.mute.remove` - Снятие мутов
- `forge.moderation.mute.info` - Просмотр информации о мутах
- `forge.moderation.mute.list` - Просмотр списка активных мутов
- `forge.moderation.mute.check` - Проверка статуса мута игрока

### Расширенные разрешения для проверки на читы
- `forge.moderation.cc.start` - Начало проверки на читы
- `forge.moderation.cc.end` - Завершение проверки
- `forge.moderation.cc.pause` - Приостановка проверки
- `forge.moderation.cc.resume` - Возобновление проверки
- `forge.moderation.cc.discord` - Изменение Discord контакта
- `forge.moderation.cc.result` - Установка результата проверки
- `forge.moderation.cc.endany` - Завершение любой проверки (даже начатой другим админом)
- `forge.moderation.cc.pauseany` - Приостановка любой проверки
- `forge.moderation.cc.resumeany` - Возобновление любой проверки
- `forge.moderation.cc.discordany` - Изменение Discord для любой проверки
- `forge.moderation.cc.resultany` - Установка результата для любой проверки

## Система банов

### Команды
```
/ban add <игрок/steamID> <причина> [срок]
/ban remove <игрок/steamID> [причина]
/ban info <игрок/steamID>
/ban list [страница]
/ban history <игрок/steamID> [страница]
```

### Примеры использования
```
/ban add Notch читы 7d
/ban add 76561198012345678 гриферство 30d
/ban add Notch нецензурная лексика perm
/ban remove Notch Ошибка администратора
/ban info Notch
/ban list 2
/ban history Notch 1
```

### Форматы срока
- `s` - секунды (например, `30s` = 30 секунд)
- `m` - минуты (например, `5m` = 5 минут)
- `h` - часы (например, `12h` = 12 часов)
- `d` - дни (например, `7d` = 7 дней)
- `w` - недели (например, `2w` = 2 недели)
- `M` - месяцы (например, `1M` = 1 месяц)
- `y` - годы (например, `1y` = 1 год)
- `perm` или `0` - перманентный бан

### Особенности
- Система отслеживает IP-адреса и обнаруживает попытки обхода бана
- Перед выдачей бана требуется подтверждение с кодом
- При выдаче бана онлайн-игрок автоматически кикается
- История банов сохраняется даже после истечения срока
- Перманентные баны не истекают

## Система предупреждений (варны)

### Команды
```
/warn add <игрок/steamID> <причина> [срок]
/warn remove <игрок/steamID> <ID предупреждения> [причина]
/warn info <игрок/steamID>
/warn list [страница]
/warn history <игрок/steamID> [страница]
/warn my [страница]
```

### Примеры использования
```
/warn add Notch нарушение правил чата 3d
/warn add 76561198012345678 спам 7d
/warn remove Notch 123 Ошибка модератора
/warn info Notch
/warn list 1
/warn history Notch 2
/warn my 1
```

### Особенности
- Предупреждения имеют ограниченный срок действия
- Игроки могут просматривать свои предупреждения через команду `/warn my`
- Система автоматических наказаний за накопление предупреждений (настраивается в конфигурации)
- Можно настроить автоматический бан при достижении определенного количества предупреждений

## Система мутов

### Команды
```
/mute add <игрок/steamID> <тип> [причина] [срок]
/mute remove <игрок/steamID>
/mute info <игрок/steamID>
/mute list [страница]
/mute check <игрок/steamID>
```

### Примеры использования
```
/mute add Notch voice Спам в войсе 1h
/mute add 76561198012345678 chat Нецензурная лексика 1d
/mute add Notch both Оскорбления 3d
/mute remove Notch
/mute info Notch
/mute list 1
/mute check Notch
```

### Типы мутов
- `voice`, `v` - Запрет использования голосового чата
- `chat`, `c` - Запрет использования текстового чата
- `both`, `b` - Запрет использования и голосового, и текстового чата

### Особенности
- Игроки получают уведомление при попытке использовать заблокированный чат
- Мут автоматически снимается после истечения срока
- Онлайн-игроки сразу получают уведомление о муте
- Мут можно наложить на оффлайн-игроков

## Система проверки на читы

### Команды
```
/cc start <игрок> [discord]
/cc end <игрок>
/cc pause <игрок>
/cc resume <игрок>
/cc discord <игрок> <discord>
/cc result <игрок> <pass|fail> [причина]
```

### Примеры использования
```
/cc start Notch MrNotch#1234
/cc end Notch
/cc pause Notch
/cc resume Notch
/cc discord Notch NewNotch#5678
/cc result Notch pass
/cc result Notch fail Обнаружен чит на ESP
```

### Особенности
- Открывает специальный UI для игрока с таймером и контактами администратора
- Если игрок не свяжется с администратором в течение указанного времени - автоматический бан
- Выход с сервера во время проверки - автоматический бан
- Возможность приостановки/возобновления проверки для админов
- После провала проверки игрок получает перманентный бан

## Конфигурация

Основная конфигурация плагина находится в файле `Plugins/ModerationSystem/ModerationSystem.configuration.xml`

### Основные настройки
```xml
<DiscordWebhookUrl></DiscordWebhookUrl> <!-- URL Discord вебхука для уведомлений -->
<ServerName>Unturned Server</ServerName> <!-- Название сервера для уведомлений -->
<EnableDiscordNotifications>false</EnableDiscordNotifications> <!-- Включить уведомления в Discord -->
<EnableGlobalNotifications>true</EnableGlobalNotifications> <!-- Включить глобальные уведомления в чате -->
<EnableAdminNotifications>false</EnableAdminNotifications> <!-- Включить уведомления для администраторов -->
<DefaultWarnDuration>604800</DefaultWarnDuration> <!-- Срок действия предупреждений по умолчанию (в секундах) -->
<EnableWarnNotifications>true</EnableWarnNotifications> <!-- Уведомлять игроков о предупреждениях -->
<EnableAutoActions>true</EnableAutoActions> <!-- Включить автоматические наказания -->
```

### Автоматические наказания за предупреждения
```xml
<WarnPunishments>
  <Punishment warnsCount="1" type="Message">
    <Message>Вы получили предупреждение. При достижении 3 варнов вы будете временно забанены.</Message>
    <Duration>0</Duration>
    <LogToConsole>true</LogToConsole>
    <Reason>Автоматическое наказание за накопление предупреждений</Reason>
  </Punishment>
  <Punishment warnsCount="3" type="Ban">
    <Message>Вы были автоматически забанены из-за накопления 3 предупреждений.</Message>
    <Duration>86400</Duration> <!-- 1 день -->
    <LogToConsole>true</LogToConsole>
    <Reason>Автоматическое наказание за накопление предупреждений</Reason>
  </Punishment>
</WarnPunishments>
```

### Настройки проверки на читы
```xml
<CheckCheatsEffectID>20000</CheckCheatsEffectID> <!-- ID эффекта для UI проверки -->
<CheckCheatsEffectKey>20000</CheckCheatsEffectKey> <!-- Ключ эффекта для UI -->
<CheckCheatsTimeoutSeconds>300</CheckCheatsTimeoutSeconds> <!-- Время на связь с администратором -->
<CheckCheatsBanReason>Перманентный бан за отказ/уклонение от проверки на читы</CheckCheatsBanReason>
```

## Discord-интеграция

Для включения уведомлений в Discord необходимо:

1. Создать вебхук в канале Discord
   - Зайти в настройки канала
   - Перейти в раздел "Интеграции" > "Вебхуки"
   - Создать новый вебхук и скопировать его URL

2. Настроить плагин
   - Указать URL вебхука в параметре `<DiscordWebhookUrl>`
   - Установить `<EnableDiscordNotifications>true</EnableDiscordNotifications>`
   - Заполнить `<ServerName>` для отображения в уведомлениях

3. Типы уведомлений в Discord:
   - Выдача/снятие банов
   - Выдача/снятие предупреждений
   - Выдача/снятие мутов
   - Начало/результаты проверок на читы

### Пример уведомления о бане в Discord:
```
🚫 Игрок забанен
👤 Игрок: Notch
🆔 SteamID: 76561198012345678
👮 Администратор: AdminName
📝 Причина: Использование читов
⏳ Срок: 7 дней
📅 Истекает: 28.05.2025 18:30 (6 дней 23 часа)
```

Этот плагин обеспечивает полный контроль над процессами модерации на сервере, позволяя эффективно управлять игроками и сохранять историю всех действий.
