![yamdb_workflow](https://github.com/EvgeniyBudaev/yamdb_final/actions/workflows/yamdb_workflow.yaml/badge.svg)

# Яндекс.Практикум. Python backend. API YamDB

## Содержание
- [Описание_проекта](#Описание_проекта)
- [Технологии](#Технологии)
- [Workflow](#Workflow)
- [Запуск проекта](#Запуск_проекта)
- [Тесты](#Тесты)
- [Авторы](#Авторы)

### <a name="Описание_проекта">Описание</a>

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Title). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха. Произведению может быть присвоен жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые читатели оставляют к произведениям текстовые отзывы (Review) и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти). Из множества оценок автоматически высчитывается средняя оценка произведения.

### <a name="Технологии">Технологии</a>

В проекте применяется 
- **Django REST Framework**, 
- **Python 3**,
- **PostgreSQL**,
- **Docker**, 
- **Nginx**,
- **Gunicorn**,
- **Git**, 
- **Simple JWT** - аутентификация реализована с помощью **JWT-токена**.

### <a name="Workflow">Workflow состоит из четырёх шагов:</a>
- *Тестирование проекта (flake8 и pytest)*,
- *Сборка и публикация образа на DockerHub*,
- *Автоматический деплой на удаленный сервер*,
- *Отправка уведомления в телеграм-чат*.

### <a name="Запуск проекта">Запуск проекта</a>

- Клонируем репозиторий и перейдем в него
```python
 git clone https://github.com/EvgeniyBudaev/yamdb_final
 cd yamdb_final/
```

- Активируем виртуальное окружение
```python
 .\venv\Scripts\activate
```

- Остановить службу nginx на сервере:

```python
 sudo systemctl stop nginx
```

- Установите docker и docker-compose на сервер:

```python
 sudo apt install docker.io
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

- Примените разрешения для исполняемого файла к двоичному файлу:

```python
  sudo chmod +x /usr/local/bin/docker-compose
```

- Протестируйте установку:

```python
  docker-compose --version
```

-  Отредактируйте файл nginx/default.conf и в строке server_name впишите свой IP

- Скопируйте файлы docker-compose.yaml и nginx/default.conf из проекта на сервер
  (на локальной машине в терминале по месту нахождения файла,
  нужно создать на сервере mkdir nginx):

```python
  scp docker-compose.yaml <username>@<host>/home/<username>/docker-compose.yaml
  scp default.conf <username>@<host>/home/<username>/nginx/default.conf
```

- После успешного деплоя зайдите на боевой сервер и выполните команды (только после первого деплоя):
    Собрать статические файлы в STATIC_ROOT:
```python
  docker-compose exec web python3 manage.py collectstatic --noinput
```

- Применить миграции и создать суперпользователя:

```python
  docker-compose exec web python manage.py makemigrations
  docker-compose exec web python manage.py migrate --noinput
  docker-compose exec web python manage.py createsuperuser
```

- Заполнить базу начальными данными:

```python
  docker-compose exec web python manage.py loaddata fixtures.json
```

###
Теперь проект доступен по адресу http://62.84.119.85.

- Остановить приложение:

```python
  docker-compose down
```


Инструкция как пользоваться данным API доступна по адресу http://localhost/redoc/

### <a name="Тесты">Тесты</a>
```python
  pytest
```

### <a name="Авторы">Авторы</a>
```
 Евгений Будаев
```