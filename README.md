## Имя: Дорджиев Виктор
## Группа: ЭФМО-02-25
# ПЗ №3 — Логирование с помощью logrus

## Цель
Научиться внедрять структурированные логи в сервис и
применять единый стандарт логирования для диагностики и
эксплуатации.

Почему logrus: 
- проще для старта;
- удобен для “полей”;
- распространён в учебных проектах.

Стандарт полей логов
Обязательные поля:
- level — уровень (DEBUG/INFO/WARN/ERROR)
- ts — время (автоматически логгером)
- service — имя сервиса (auth / tasks)
- request_id — request id (из заголовка или сгенерированный)
- method — HTTP метод
- path — путь запроса (например /v1/tasks)
- status — код ответа
- duration_ms — длительность обработки (миллисекунды)
Для ошибок дополнительно:
- error — текст ошибки (без секретов)
- component — компонент/слой (например auth_client,
repository, handler)

## Установка и запуск

(Необходимы предустановленные Go версии 1.22 и выше и Git)

Клонировать репозиторий:

```
git clone <URL_РЕПОЗИТОРИЯ>
cd tech-ip-proto
```

Команда запуска сервера:

Терминал 1
```
go run ./services/auth/cmd/auth
```
Терминал 2
```
go run ./services/tasks/cmd/tasks
```

## Структура проекта
```plaintext
tech-ip-logrus/
├── proto/
│   └── auth.proto
├── gen/
│   └── auth/
│       └── v1/
│           ├── auth.pb.go
│           └── auth_grpc.pb.go
├── docs/
├── services/
│   ├── auth/
│   │   ├── cmd/
│   │   │   └── auth/
│   │   │       └── main.go
│   │   └── internal/
│   │       ├── grpc/
│   │       │   └── server.go
│   │       ├── http/
│   │       │   └── handler.go
│   │       └── service/
│   │           └── auth.go
│   └── tasks/
│       ├── cmd/
│       │   └── tasks/
│       │       └── main.go
│       └── internal/
│           ├── client/
│           │   └── authclient/
│           │       └── client.go
│           ├── http/
│           │   └── handler.go
│           └── service/
│               └── tasks.go
├── shared/
│   ├── httpx/
│   │   └── json.go
│   ├── logger/
│   │   └── logger.go
│   └── middleware/
│       ├── logging.go
│       └── requestid.go
├── go.mod
├── go.sum
└── README.md
```



## Скриншоты
### Скрин/лог с request-id, подтверждающий прокидывание.

<img width="1795" height="565" alt="image" src="https://github.com/user-attachments/assets/c4f2dfdc-aca1-489c-b1cb-574492ee860e" />


<img width="1796" height="513" alt="image" src="https://github.com/user-attachments/assets/23db8837-c6b1-4010-9c75-62ced7f8a078" />

### Получить токен

```
http://185.250.46.179:8081/v1/auth/login
```

<img width="479" height="461" alt="image" src="https://github.com/user-attachments/assets/60881aac-9dfa-4d68-8dcb-b2212fd19967" />

### Проверка токена напрямую

```
http://185.250.46.179:8081/v1/auth/verify
```

<img width="597" height="458" alt="image" src="https://github.com/user-attachments/assets/7f9939e4-980f-41c6-aeb7-c624bb15e4c5" />

### Создать задачу через Tasks (с проверкой Auth)

```
http://185.250.46.179:8082/v1/tasks
```

<img width="568" height="517" alt="image" src="https://github.com/user-attachments/assets/a3b9844e-81e8-400c-aaa2-65dc9feee181" />

### Попробовать без токена (должно быть 401)

```
http://185.250.46.179:8082/v1/tasks
```

<img width="536" height="455" alt="image" src="https://github.com/user-attachments/assets/bd55ad75-af30-4ac0-b412-51ddb6e47a78" />
