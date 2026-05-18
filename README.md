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



### Успешный запрос

```
http://91.200.84.37:8086/v1/tasks
```

<img width="1280" height="712" alt="image" src="https://github.com/user-attachments/assets/193140ae-b6b0-4caf-8a2a-222a6399a8d4" />

### Запрос с ошибкой (неверный токен)

```
http://91.200.84.37:8086/v1/tasks
```

<img width="1280" height="617" alt="image" src="https://github.com/user-attachments/assets/e4fc5981-29af-41bd-a864-ec67c0835246" />

### Запрос с межсервисным вызовом

```
http://91.200.84.37:8086/v1/tasks
```

<img width="1280" height="741" alt="image" src="https://github.com/user-attachments/assets/47e20ac1-29ec-41b9-b987-528b11de8f67" />

### Логи

<img width="1280" height="445" alt="image" src="https://github.com/user-attachments/assets/6f398caa-b197-4562-9719-6fab27d7f8bd" />

<img width="1280" height="451" alt="image" src="https://github.com/user-attachments/assets/114a06f1-381b-4a5b-8b30-88fa4b92a0a7" />

