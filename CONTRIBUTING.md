# Руководство по разработке (Future Work)

Этот файл поможет вам ориентироваться в структуре проекта при добавлении нового функционала.

## Навигация по файлам

- `src/pymax/core.py` — Главный входной узел, логика подключения и управления циклом.
- `src/pymax/mixins/` — Весь основной функционал разбит на логические модули:
    - `message.py` — Работа с сообщениями и файлами.
    - `auth.py` — Регистрация и авторизация.
    - `channel.py` / `group.py` — Управление сообществами.
    - `user.py` — Поиск и получение инфо о юзерах.
- `src/pymax/payloads.py` — Pydantic модели для формирования JSON запросов к API.
- `src/pymax/types.py` — Внутренние модели данных (Message, User, Chat).

## Как добавить новый метод?

1. Найдите подходящий миксин в `src/pymax/mixins/`.
2. Опишите payload в `src/pymax/payloads.py` (если его еще нет).
3. Добавьте метод в класс миксина, используя `self._send_and_wait(...)`.

Пример:
```python
async def my_new_method(self, param: str):
    payload = MyPayload(p=param).model_dump(by_alias=True)
    return await self._send_and_wait(opcode=Opcode.SOME_OP, payload=payload)
```

## Тестирование
Для тестов используется `pytest`. Перед коммитом запускайте:
```bash
pytest
```
