Пример приложения Spring Boot с использованием Spring Security, OAuth2 и Keycloak.

### Шаг 1: Запуск Keyclock в docker:

```text
docker compose up -d
```

Keycloak настроен на порт 9090. После запуска контейнера мы можем перейти на наш сервер по
ссылке http://localhost:9090. Для того чтобы войти в настройки keycloak, нам нужно вести данные, которые мы указали при
создании контейнера:

- KEYCLOAK_ADMIN: admin
- KEYCLOAK_ADMIN_PASSWORD: admin

### Шаг 2: Настройка Keycloak

1. Создание realm (myrealm)
2. Создание client (myclient, root url -> http://localhost:8083)
3. Создание пользователей (user и manager c ролью ROLE_MANAGER)

Проверка настроек:

```text
curl --location "http://localhost:9090/realms/myrealm/protocol/openid-connect/token" 
-H "Content-Type: application/x-www-form-urlencoded" 
-d "client_id=myclient" 
-d "client_secret=client_secret" 
-d "username=manager" 
-d "password=123" 
-d "grant_type=password"
```

Пример ответа:

```json
{
  "access_token": "eyJhbGciOiJSUzI1. . . ",
  "expires_in": 300,
  "refresh_expires_in": 1800,
  "refresh_token": "eyJhbGciOiJIUzUxMiI. . .",
  "token_type": "Bearer",
  "not-before-policy": 1738064067,
  "session_state": "812b2bc8-b839-4180-926a-aa4d713a4fe7",
  "scope": "profile"
}
```

### Шаг 3: Тестирование

* Идем на хост - http://localhost:8083 
* -> перебрасывает на форму авторизации Keycloak
* -> вводим login/password пользователя user
* -> Переходим на http://localhost:8083/authenticated.html
* -> OK
* 
* -> Переходим на  http://localhost:8083/manager.html
* -> 403 ошибка
* 
* -> Логинимся пользователем manager
* -> Переходим на  http://localhost:8083/manager.html
* -> OK


