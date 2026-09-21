# Проверка GigaChat через Yandex Cloud Function

Небольшой статический сайт для проверки связки **GitHub Pages → Yandex Cloud Function → GigaChat**.

## Что умеет

- отправлять текстовое сообщение в функцию;
- прикладывать один файл: изображение, PDF, TXT, DOC или DOCX;
- отображать ответ модели и ошибки сети;
- не содержит и не передаёт в браузер ключ GigaChat.

## Endpoint

В `index.html` задан URL функции:

```js
const FUNCTION_URL = 'https://functions.yandexcloud.net/d4e6gg7qd3tlqotvn86c';
```

## Ожидаемый API-контракт

Клиент выполняет `POST` с телом `multipart/form-data`:

- `message` — текст вопроса;
- `file` — необязательный вложенный файл.

Успешный ответ должен быть JSON, предпочтительно:

```json
{ "answer": "Текст ответа GigaChat" }
```

Также клиент распознаёт поля `response`, `message` и `result.answer`. Если ваша функция принимает JSON или использует другие имена полей, поправьте обработчик `fetch` в `index.html`.

## CORS

Для GitHub Pages функция должна разрешать запросы с домена:

```text
https://koposovds-sudo.github.io
```

Для диагностики допускается `Access-Control-Allow-Origin: *`; в рабочем варианте лучше ограничить его конкретным доменом. Также обработайте preflight-запрос `OPTIONS` и разрешите как минимум методы `POST, OPTIONS` и заголовок `Content-Type`.

## Публикация на GitHub Pages

1. Откройте настройки репозитория: **Settings → Pages**.
2. В разделе **Build and deployment** выберите **Deploy from a branch**.
3. Выберите ветку `main` и папку `/(root)`.
4. Сохраните настройки и дождитесь публикации.
5. Откройте адрес вида `https://koposovds-sudo.github.io/gigachat-function-test/`.

## Безопасность

Токен/ключ GigaChat должен храниться только в переменных окружения или секретах Yandex Cloud Function. Никогда не добавляйте ключ в `index.html`, коммиты или настройки GitHub Pages.
