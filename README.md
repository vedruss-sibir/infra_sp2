# Учебный проект INFRA_SP2

Цель работы над проектом - получить опыт c Docker.

## Задание

Упаковать приложение api_yamdb в контейнеры.

## Техническое описание проекта YaMDb

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles).

Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором.

Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

Отзыв может быть прокомментирован (Сomment) пользователями.

- ### Пользовательские роли

  - Аноним — может просматривать описания произведений, читать отзывы и комментарии.
  - Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
  - Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
  - Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
  - Суперюзер Django должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер.

- ### Самостоятельная регистрация новых пользователей

  - Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
  - Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
  - Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
  - В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.
  - После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ и заполнить поля в своём профайле (описание полей — в документации).

- ### Создание пользователя администратором

  - Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей запроса для этого случая — в документации). В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
  - После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/ , в ответ ему должно прийти письмо с кодом подтверждения.
  - Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.

- ### Ресурсы API YaMDb

  - Ресурс auth: аутентификация.
  - Ресурс users: пользователи.
  - Ресурс titles: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
  - Ресурс categories: категории (типы) произведений («Фильмы», «Книги», «Музыка»).
  - Ресурс genres: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.
  - Ресурс reviews: отзывы на произведения. Отзыв привязан к определённому произведению.
  - Ресурс comments: комментарии к отзывам. Комментарий привязан к определённому отзыву.
-

- ### База данных

  - В репозитории с заданием, в директории /api_yamdb/static/data, подготовлены несколько файлов в формате csv с контентом для ресурсов Users, Titles, Categories, Genres, Review и Comments.
  - Для тестирования работы проекта можно наполнить БД данным контентом из приложенных csv-файлов.
  - Процедура импорта из CSV - на усмотрение исполнителя.

---

## Как запустить проект

Клонировать репозиторий и перейти в него в командной строке:

```
git clone
cd infra
```

Cоздать и активировать виртуальное окружение:

```
python -m venv venv
source venv/Scripts/activate
```

Запустить проект:

```
docker-compose upg
```

Сам проект и админ-панель искать по адресам:

```
http://127.0.0.1:8000
http://127.0.0.1:8000/admin
```

Документация API

```
http://127.0.0.1:8000/redoc
```

***
