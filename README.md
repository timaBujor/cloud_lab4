# Отчет по лабораторной работе №4
## Облачное хранилище данных. Amazon S3

**Дисциплина:** Облачные вычисления / Облачные технологии  
**Выполнил:** Бужор Тимофей  
**Вариант (Номер студента):** 04  
**Имена бакетов:** `cc-lab4-pub-04`, `cc-lab4-priv-04`, `cc-lab4-web-04`  
**Регион:** `eu-central-1` (Frankfurt)

---

# 1. Описание лабораторной работы и постановка задачи

## Цель работы
Знакомство с Amazon S3 и выполнение основных операций управления облачным хранилищем:
- создание бакетов с разными уровнями доступа;
- загрузка объектов через AWS Console и AWS CLI;
- управление доступом (ACL и политики);
- версионирование объектов;
- lifecycle rules;
- статический веб-сайт на S3.

## Результаты обучения
После выполнения работы студент:
- понимает различия S3 / EBS / EFS;
- умеет настраивать безопасность S3;
- использует AWS CLI;
- проектирует lifecycle storage стратегии.

---

# 2. Практическая часть

---

## Шаг 1. Подготовка структуры

```text
s3-lab/
 ├── public/
 │   ├── avatars/
 │   │   ├── user1.jpg
 │   │   └── user2.jpg
 │   └── content/
 │       └── logo.png
 ├── private/
 │   └── logs/
 │       └── activity.csv
 └── README.md
```

---

# Шаг 2. Создание бакетов

## Публичный бакет `cc-lab4-pub-04`

- ACL включены
- Block Public Access отключен

<img src="https://github.com/user-attachments/assets/837fbcf2-358b-485e-8928-dea5f1d8f432" width="800"/>

---

## Приватный бакет `cc-lab4-priv-04`

- Block Public Access включен
- доступ закрыт

<img src="https://github.com/user-attachments/assets/4578b0c8-28b1-4f95-b08a-341a2774b637" width="800"/>

---

## Контрольный вопрос: ACL vs Object Ownership

ACL:
- доступ на уровне объектов
- гибкая, но устаревшая модель

Object Ownership:
- доступ через IAM
- ACL отключены

---

## Block Public Access

Центральный механизм защиты от утечек данных.

---

# Шаг 3. Загрузка через Console

- создан префикс `avatars/`
- загружен `user1.jpg`
- включен public-read

<img src="https://github.com/user-attachments/assets/bcd5c0cf-fe10-4a76-a14a-d6157d45bbc4" width="800"/>

---

## Контрольный вопрос

S3:
- нет реальных папок
- ключ = полный путь

---

# Шаг 4. AWS CLI

```bash
aws configure
```

---

## Upload objects

```bash
aws s3 cp s3-lab/public/avatars/user2.jpg s3://cc-lab4-pub-04/avatars/user2.jpg --acl public-read
aws s3 cp s3-lab/public/content/logo.png s3://cc-lab4-pub-04/content/logo.png --acl public-read
aws s3 cp s3-lab/private/logs/activity.csv s3://cc-lab4-priv-04/logs/activity.csv
```

<img src="https://github.com/user-attachments/assets/380bab6f-80d0-4a43-b09a-3917325b761b" width="800"/>

---

## Разница команд

- cp — копирование
- mv — перемещение
- sync — синхронизация

---

# Шаг 5. Проверка доступа

## Public file

<img src="https://github.com/user-attachments/assets/668db5c5-857a-45d6-b41a-725f86946014" width="800"/>

## Private file

<img src="https://github.com/user-attachments/assets/476ca123-ccbf-4145-9b8b-ecf7ae5f2e10" width="800"/>

---

# Шаг 6. Versioning

- включен Versioning
- объект хранит версии

<img src="https://github.com/user-attachments/assets/9fa848de-1f12-4831-af2d-f8d6ced8d4bc" width="800"/>

---

## Контрольный вопрос

Versioning:
- нельзя отключить полностью
- можно только Suspend

---

# Шаг 7. Lifecycle rules

- logs/ prefix
- Standard → IA → Glacier → Delete

<img src="https://github.com/user-attachments/assets/676b66ae-e10d-4d9a-b8fc-54ac4ba2f0aa" width="800"/>

---

## Storage Classes

- Standard
- Standard-IA
- Glacier Deep Archive

---

# Шаг 8. Static Website Hosting

<img src="https://github.com/user-attachments/assets/41b2a4e7-867b-4ee8-a7ae-7f6cf1eaa19f" width="800"/>

<img src="https://github.com/user-attachments/assets/bc4bf0aa-7597-4c71-90e9-678a9f997337" width="800"/>

---

## Endpoint

```text
http://cc-lab4-web-04.s3-website.eu-central-1.amazonaws.com
```

<img src="https://github.com/user-attachments/assets/50a03bea-57d5-4c8f-a7b1-d302028cb658" width="800"/>

---

# 3. Источники

- AWS S3 Documentation  
- AWS CLI Reference  
- AWS Lifecycle Policies  

---

# 4. Вывод

В работе изучены:
- S3 buckets
- ACL и IAM
- CLI автоматизация
- Versioning
- Lifecycle policies
- Static website hosting

Amazon S3 позволяет строить serverless инфраструктуру хранения и хостинга.
