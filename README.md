# Групповой проект: Yamdb

Проект YaMDb собирает отзывы пользователей на произведения. Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку..

## Функционал и особенности работы API:

1. Обращение к API Yamdb для CRUD операций.
2. Просмотр, создание, редактирование и удаление отзывов к произведениям и комментариев к отзывам.
3. Пользователь регистрируется через код доступа по почте и получает личный токен c этим кодом. Используется simple_jwt.
4. Разпределение ролей для пользователей: user, moderator, admin.
5. Возможность добавить в своем профиле биографию о себе.

## Порядок запуска API-сервиса:

Клонируйте репозиторий себе на компьютер/сервер:

```bash
git clone git@github.com:user_name/api_final_yatube.git
```

Создайте виртуальное окружение:

```bash
python3 -m venv venv
```

Активируйте виртуальное окружение:

*Windows:*
```bash
source venv/Scripts/activate
```
*Linux & MacOS:*
```bash
source venv/Bin/activate
```

Установите зависимости из файла requirements.txt:

```bash
pip install -r requirements.txt
```

Загрузка данных в БД для тестирования приложения и его функционала:

```bash
py manage.py makemigrations
```

```bash
py manage.py migrate
```

```bash
py manage.py import_csv_data
```

## Примеры запросов к API:

1. **Путь к эндпоинтам API.**
*http://127.0.0.1:8000/api/v1/* - стандартный путь к API;

*http://127.0.0.1:8000/api/v1/users/* - регистрация пользователя;

*http://127.0.0.1:8000/api/v1/auth/* - получение токена для зарегистрированного пользователя;

*http://127.0.0.1:8000/api/v1/categories/* - категории (типы) произведений («Фильмы», «Книги», «Музыка»);

*http://127.0.0.1:8000/api/v1/genre/* - жанры произведений. Одно произведение может быть привязано к нескольким жанрам;

*http://127.0.0.1:8000/api/v1/titles/* - произведения, к которым пишут отзывы (определённый фильм, книга или песенка);

*http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/* - отзывы на произведения;

*http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{reviews_id}/comments* - комментарии к отзывам.

Ко всем эндпоинтам можно обратиться за детальной информацией, указав в конце id.
2. **Регистрация и получение токена.**
Эндпоинт: */api/v1/users/* принимает запросы GET, PATCH, POST и DELETE доступны только администраторам.
POST - Регистрация пользователя для получения кода доступа (возможен повторный запрос):
*http://127.0.0.1:8000/api/v1/users/* 
```json
{
    "email": "string",
    "username": "string",
    "first_name": "string (not required)",
    "last_name": "string (not required)",
    "bio": "string (not required)",
    "role": "string (not required, auto - user)"
}
```
POST - Самостоятельная регистрация пользователя для получения кода доступа (возможен повторный запрос):
*http://127.0.0.1:8000/api/v1/auth/signup/*
```json
{
    "email": "string",
    "username": "string"
}
```
POST - Получить JWT-токен указав код доступа отправленный на почту:
*http://127.0.0.1:8000/api/v1/auth/* -
```json
{
    "username": "string",
    "confirmation_code": "string"
}
```
Полученный токен необходимо использовать для любых операций к API, кроме GET.
3. **Жанры и категориии.**
Эндпоинты: */api/v1/genre/* и */api/v1/categories/* принимает запросы GET от любого пользователя, POST и DELETE доступны только администраторам.
```json
{
    "name": "string",
    "slug": "string"
}
```
4. **Произведения и отзывы.**
Эндпоинт: */api/v1/titles/*  принимает запросы GET от любого пользователя, POST, PATCH и DELETE доступны только администраторам.
```json
{
    "name": "string",
    "year": "integer",
    "description": "string (not required)",
    "genre": "Array of strings",
    "category": "string"
}
```
Для поиска произведений можно использовать параметр фильтрации по: name, year, genre, category
Эндпоинт: */api/v1/titles/{title_id}/reviews/* принимает запросы GET от любого пользователя, POST, PATCH и DELETE доступны только администратору, модератору и автору.
```json
{
    "text": "string",
    "score": "integer [от 1 до 10]"
}
```
5. **Комментарии к отзывам.**
Эндпоинт: */api/v1/titles/{title_id}/reviews/{reviews_id}/comment/* принимает запросы GET от любого пользователя, POST, PATCH и DELETE доступны только администратору, модератору и автору.
```json
{
    "text": "string"
}
```
