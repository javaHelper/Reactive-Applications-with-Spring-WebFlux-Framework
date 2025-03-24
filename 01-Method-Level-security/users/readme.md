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


```java
package com.appsdeveloperblog.reactive.ws.users.presentation;

import com.appsdeveloperblog.reactive.ws.users.presentation.model.CreateUserRequest;
import com.appsdeveloperblog.reactive.ws.users.presentation.model.UserRest;
import com.appsdeveloperblog.reactive.ws.users.service.UserService;
import jakarta.validation.Valid;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.security.access.prepost.PostAuthorize;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

import java.net.URI;
import java.util.UUID;

@RestController
@RequestMapping("/users") //   http://localhost:8080/users
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @PostMapping
    public Mono<ResponseEntity<UserRest>> createUser(@RequestBody @Valid Mono<CreateUserRequest> createUserRequest) {
        return userService.createUser(createUserRequest)
                .map(userRest -> ResponseEntity
                        .status(HttpStatus.CREATED)
                        .location(URI.create("/users/" + userRest.getId()))
                        .body(userRest));
    }

    @GetMapping("/{userId}")
    //@PreAuthorize("authentication.principal.equals(#userId.toString()) or hasRole('ROLE_ADMIN')")
    @PostAuthorize("returnObject.body!=null and (returnObject.body.id.toString().equals(authentication.principal))")
    public Mono<ResponseEntity<UserRest>> getUser(@PathVariable("userId") UUID userId) {
        return userService.getUserById(userId)
                .map(userRest -> ResponseEntity.status(HttpStatus.OK).body(userRest))
                .switchIfEmpty(Mono.just(ResponseEntity.status(HttpStatus.NOT_FOUND).build()));
    }

    @GetMapping
    public Flux<UserRest> getUsers(@RequestParam(value = "page", defaultValue = "0") int page,
                                   @RequestParam(value = "limit", defaultValue = "50") int limit) {
        return userService.findAll(page,limit);
    }
}
```
