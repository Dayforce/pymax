# Документация API PyMax

Этот документ содержит описание доступных методов и функционала библиотеки **PyMax** для работы с мессенджером Max.

## Основные концепции

Библиотека работает через протокол WebSocket (для Web-версии) или через сырые сокеты (для мобильных и десктопных приложений). Она имитирует работу реальных клиентов, используя соответствующие заголовки и отправляя телеметрию.

### Клиенты

1.  **`MaxClient`**: Предназначен для имитации **Web-версии** (`device_type="WEB"`). Работает через WebSocket.
2.  **`SocketMaxClient`**: Предназначен для имитации **мобильных приложений** (`ANDROID`, `IOS`) или **Desktop-версии**. Работает через TCP-сокет.

---

## Методы

### Аутентификация

- `request_code(phone: str)`: Запрос кода подтверждения на номер телефона.
- `login_with_code(temp_token: str, code: str, start: bool = False)`: Завершает процесс входа с использованием кода.
- `register(first_name: str, last_name: str = None)`: Регистрация нового аккаунта (интерактивная).
- `register_with_code(temp_token: str, code: str, first_name: str, last_name: str = None, start: bool = False)`: Завершает процесс регистрации с использованием кода.
- `logout()`: Выход из аккаунта и аннулирование токена.

### Сообщения

- `send_message(text: str, chat_id: int, notify: bool = True, attachment=None, reply_to: int = None, use_queue=False)`: Отправка текстового сообщения.
- `edit_message(chat_id: int, message_id: int, text: str, attachment=None)`: Редактирование сообщения.
- `delete_message(chat_id: int, message_ids: list[int], for_me: bool)`: Удаление сообщений.
- `pin_message(chat_id: int, message_id: int, notify_pin: bool)`: Закрепление сообщения.
- `fetch_history(chat_id: int, from_time: int = None, forward: int = 0, backward: int = 200)`: Получение истории сообщений.
- `add_reaction(chat_id: int, message_id: str, reaction: str)`: Добавление реакции (emoji).
- `remove_reaction(chat_id: int, message_id: str, reaction: str)`: Удаление реакции.
- `read_messages(chat_id: int, message_ids: list[int])`: Пометка сообщений как прочитанных.

### Чаты и Каналы

- `create_channel(title: str, description: str = "")`: Создание нового канала.
- `create_group(title: str, user_ids: list[int])`: Создание группы.
- `join_channel(channel_id: int)`: Подписка на канал.
- `leave_channel(channel_id: int)`: Выход из канала.
- `get_channel_members(channel_id: int, limit: int = 100)`: Получение списка участников.
- `get_chat(chat_id: int)`: Получение информации о чате.

### Пользователи и Профиль

- `get_me()`: Получение информации о текущем пользователе.
- `get_user(user_id: int)`: Получение информации о пользователе.
- `update_profile(first_name: str, last_name: str = None, bio: str = None)`: Редактирование профиля.
- `update_username(username: str)`: Смена username.

---

## Обработчики событий (Handlers)

Вы можете использовать декораторы для обработки входящих данных:

```python
@client.on_message()
async def handler(msg: Message):
    print(f"Новое сообщение: {msg.text}")

@client.on_message_edit()
async def handler(msg: Message):
    print(f"Сообщение изменено: {msg.text}")

@client.on_reaction_change()
async def handler(message_id, chat_id, reaction_info):
    print(f"Реакции изменились в чате {chat_id}")
```

## Фильтры

Для фильтрации событий используйте класс `Filters`:

- `Filters.chat(chat_id)`: Только из конкретного чата.
- `Filters.user(user_id)`: Только от конкретного пользователя.
- `Filters.text("привет")`: Только сообщения с конкретным текстом.
- `Filters.regex(r"^[0-9]+$")`: Регулярные выражения.
