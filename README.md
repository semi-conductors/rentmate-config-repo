
# RentMate Configuration Repository

This repository contains **centralized configuration files** for all microservices in the RentMate ecosystem.  
It is used by the **Spring Cloud Config Server** to serve configurations dynamically to each service at runtime.

---

## 📂 Repository Structure

Each microservice should store its config files inside this repository.  
Typical structure:

```

rentmate-config-repo/
│
├── user-service.yml
├── user-service-dev.yml
│
├── payment-service.yml
├── payment-service-dev.yml
│
├── delivery-service.yml
├── delivery-service-dev.yml
│
└── ...

```

---

## 🛠️ Environment Conventions

Each service should have (at minimum):

| File | Purpose |
|------|---------|
| `service-name.yml` |  Default configuration |
| `service-name-dev.yml` | Development environment configuration |

Naming is important — the Config Server will fetch them by:

```

/{application}/{profile}

````

Example:

| Request | File Served |
|--------|-------------|
| `/user-service/default` | `user-service.yml` |
| `/user-service/dev` | `user-service-dev.yml` |

---

## 🔍 How Microservices Load Their Config

Make sure the microservice has in its `bootstrap.yml`:

```yaml
spring:
  application:
    name: user-service

  cloud:
    config:
      uri: http://localhost:8888
      profile: dev
      label: master
````

---

## 🌱 Adding New Services

To integrate a new microservice with centralized configuration:

1. Create its config files:

   ```
   {service-name}.yml
   {service-name}-dev.yml
   ```
2. Commit and push to this repo.
3. Restart the service **or** call its `/actuator/refresh` endpoint (if enabled).

---

## 🧪 Testing the Config Server

Test using API calls:

### Default profile

```
GET http://localhost:8888/user-service/default
```

### Dev profile

```
GET http://localhost:8888/user-service/dev
```

If configs load successfully, the server will return a JSON showing all properties.

---

## 🔒 Private Repository Support

Optionally, Config Server can clone private repos using:

```yaml
spring.cloud.config.server.git.username: ${GIT_USERNAME}
spring.cloud.config.server.git.password: ${GIT_PASSWORD}
```

Use GitHub **personal access tokens**, not passwords.

---
