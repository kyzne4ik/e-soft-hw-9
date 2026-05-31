```mermaid
erDiagram

    LEAD {
        UUID id PK
        VARCHAR(100) first_name
        VARCHAR(100) last_name
        VARCHAR(100) patronymic
        VARCHAR(255) email
        VARCHAR(50) phone
        VARCHAR(100) telegram
        TEXT experience
        VARCHAR(255) test_result
        VARCHAR(50) status
        UUID manager_id FK
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    USER {
        UUID id PK
        VARCHAR(100) first_name
        VARCHAR(100) last_name
        VARCHAR(100) patronymic
        VARCHAR(255) email "U"
        VARCHAR(255) password_hash
        VARCHAR(100) tg_id "U"
        VARCHAR(100) tg_username
        ROLES role "ENUM: ADMIN|MANAGER|MENTOR|STUDENT"
        BOOLEAN is_activated
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    NOTIFICATION {
        UUID id PK
        UUID user_id FK
        TEXT message
        BOOLEAN is_silent
        TIMESTAMP send_at
        VARCHAR(50) status
        BOOLEAN is_read
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    MENTOR_PROFILE {
        UUID id PK
        UUID user_id FK "U"
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    STUDENT_PROFILE {
        UUID id PK
        UUID user_id FK "U"
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    COURSE {
        UUID id PK
        VARCHAR(200) name
        TEXT description
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    STREAM {
        UUID id PK
        VARCHAR(100) name
        UUID course_id FK
        UUID mentor_id FK
        VARCHAR(50) status
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    REVIEW {
        UUID id PK
        UUID submission_id FK
        UUID mentor_id FK
        INTEGER score
        TEXT comment
        TIMESTAMP reviewed_at
    }

    SUBMISSION {
        UUID id PK
        UUID task_id FK
        UUID student_id FK
        VARCHAR(255) repo_link
        VARCHAR(50) status
        BOOLEAN is_activate
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    STREAM_STUDENT {
        UUID stream_id PK "FK"
        UUID student_id PK "FK"
        TIMESTAMP joined_at
    }

    LESSON {
        UUID id PK
        UUID stream_id FK
        VARCHAR(200) title
        TIMESTAMP start_time
        TIMESTAMP end_time
        VARCHAR(255) meeting_link
        VARCHAR(255) record_link
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    TASK {
        UUID id PK
        UUID stream_id FK
        VARCHAR(200) title
        TEXT description
        VARCHAR(255) repo_template
        TIMESTAMP deadline
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    USER ||--|{ LEAD : "пользователь(с ролью менеджера) управляет лидами"
    USER ||--|{ NOTIFICATION : "у пользователя есть уведомления"
    USER ||--o| MENTOR_PROFILE : "пользователь может быть ментором"
    USER ||--o| STUDENT_PROFILE : "пользователь может быть студентом"
    STUDENT_PROFILE ||--|{ SUBMISSION : "студент сдаёт домашние задания"
    STUDENT_PROFILE ||--|{ STREAM_STUDENT : "студент есть в разных потоках"
    STREAM ||--|{ STREAM_STUDENT : "поток содержит студентов"
    MENTOR_PROFILE ||--|{ REVIEW : "ментор делает ревью домашних заданий"
    SUBMISSION ||--|{ REVIEW : "по домашним заданиям делают ревью"
    TASK ||--|{ SUBMISSION : "у домашних заданий есть решения"
    STREAM ||--|{ TASK : "потоки содержат домашние задания"
    COURSE ||--|{ STREAM : "курсы есть в разных потоках"
    MENTOR_PROFILE ||--|{ STREAM : "ментор учит в разных потоках"
    STREAM ||--|{ LESSON : "у потока есть домашние задания"
```

### Описание связей

| Связь                            | Тип | Описание                                                                                                      |
| -------------------------------- | --- | ------------------------------------------------------------------------------------------------------------- |
| USER → LEAD                      | 1:N | Один менеджер может управлять разными лидами                                                                  |
| USER → NOTIFICATION              | 1:N | У одного пользователя может быть много уведомлений                                                            |
| USER → MENTOR_PROFILE            | 1:1 | У одного пользователя может быть лишь один профиль ментора                                                    |
| USER → STUDENT_PROFILE           | 1:1 | У одного пользователя может быть лишь один профиль студента                                                   |
| STUDENT_PROFILE → SUBMISSION     | 1:N | У студента может быть много сданных работ                                                                     |
| STUDENT_PROFILE → STREAM_STUDENT | 1:N | Студент может быть в множестве потоков                                                                        |
| STREAM → STREAM_STUDENT          | 1:N | Поток может быть у множества студентов                                                                        |
| STUDENT_PROFILE ↔ STREAM         | M:N | Через промежуточную таблицу `STREAM_STUDENT` - студент содержит много потоков, поток входит в много студентов |
| MENTOR_PROFILE → REVIEW          | 1:N | У одного ментора может быть множество ревью                                                                   |
| SUBMISSION → REVIEW              | 1:N | У одной сданной работы может быть множество ревью                                                             |
| TASK → SUBMISSION                | 1:N | У одного ТЗ может быть много сданных работ                                                                    |
| STREAM → TASK                    | 1:N | У одного потока может быть много ТЗ                                                                           |
| COURSE → STREAM                  | 1:N | У одного курса может быть много потоков                                                                       |
| MENTOR_PROFILE → STREAM          | 1:N | У одного ментора может быть много потоков                                                                     |
| STREAM → LESSON                  | 1:N | У одного потока может быть много занятий                                                                      |

### Ответы на вопросы:

1. Чем связь 1:N отличается от M:N? Приведите пример каждой из вашего проекта.

_Ответ:_
Отличается тем, что 1:N - это когда один элемент таблицы может быть в множестве других, а M:N - это когда один элемент таблицы может быть связан со многими элементами из второй, и наоборот.

2. Почему связь M:N нельзя реализовать двумя таблицами? Зачем нужна промежуточная?

_Ответ:_
Потому что нужна связь между двумя таблицами, а для такого достаточно и одной.
Промежуточная таблица нужна для связи, когда один объект из первой таблицы может быть связан со многими объектами из второй, и наоборот.

3. Что будет, если удалить запись, на которую ссылается FK? (Подумайте, мы разберём это на лекции)

_Ответ:_
Это зависит от того, какое удаление мы указали.
Если у нас `каскадное (cascade)` удаление, то зависимые записи (строки) имеющие в себе fk-колонку просто удалятся.
Если у нас `ограничивающие (restrict)` удаление, то бдшка вернёт ошибку и полностью отменит операцию на удаление.

4. Может ли FK быть NULL? Когда это полезно?

_Ответ:_
Да. Полезно может быть, например, в моём проекте с таблицами `lead` и `user`, когда заявка только падает в CRM от незарегистрированного кандидата, у неё ещё нет ответственного менежера, а значит `manager_id` будет **NULL**. Как только менеджер берёт заявку в работу, туда записывается его **ID**.
