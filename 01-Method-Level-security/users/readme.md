# Method Level Security in Reactive programming

# Create user

```sh
curl --location 'http://localhost:8080/users' \
--header 'Content-Type: application/json' \
--data-raw '{
    "firstName": "John",
    "lastName": "Doe",
    "email": "john.doe1@gmail.com",
    "password": "123456789"
}'
```

```json
{"id":"49aec911-fdbe-4b7a-8edf-1ea4c5954637","firstName":"John","lastName":"Doe","email":"john.doe1@gmail.com"}
```

# Login using above user 

```sh
curl --location 'http://localhost:8080/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "john.doe1@gmail.com",
    "password": "123456789"
}'
```

<img width="971" alt="Screenshot 2025-03-24 at 8 42 19 AM" src="https://github.com/user-attachments/assets/b9ebca6e-9fd2-4d6d-84cb-8ed78a6a4992" />

# Get user

```sh
curl --location 'http://localhost:8080/users/49aec911-fdbe-4b7a-8edf-1ea4c5954637' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiI0OWFlYzkxMS1mZGJlLTRiN2EtOGVkZi0xZWE0YzU5NTQ2MzciLCJpYXQiOjE3NDI3NDc1NzMsImV4cCI6MTc0Mjc1MTE3M30.PIl6C_66IiWXpT9hBdqiwcQBWrHexCRMO71A4dLqivHylBD2i7ntqwyHZI4RGI4Jv7B_eEBxZyiBvYbAFLGCGQ'
```

```json
{"id":"49aec911-fdbe-4b7a-8edf-1ea4c5954637","firstName":"John","lastName":"Doe","email":"john.doe1@gmail.com"}
```
