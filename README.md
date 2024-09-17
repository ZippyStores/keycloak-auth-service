# Keycloak Authentication Service

This microservice handles authorization and authentication using **Keycloak**. It is a part of a larger microservice architecture designed to manage secure access, user roles, and token-based authentication.

## Features

- User Authentication (Login/Logout)
- Role-based Authorization
- JWT Token Generation and Validation
- Integration with Keycloak
- Secure access to microservices using access tokens

## Requirements

- Java 11+
- Spring Boot
- Keycloak Server (vX.X.X)
- Maven/Gradle
- Docker (Optional, for containerized deployment)

## Setup Instructions

### 1. Install and Configure Keycloak

1. Download and install Keycloak from [here](https://www.keycloak.org/downloads).
2. Start Keycloak:
   ```bash
   ./bin/standalone.sh
   ```
3. Access the Keycloak Admin Console at http://localhost:8080.
4. Create a realm, client, and roles as per your application's needs.
5. Update Keycloak configurations (like client id, secret) in `application.yml` or `application.properties`.

### 2. Clone the Repository

```bash
git clone https://github.com/your-username/keycloak-authentication.git
cd keycloak-authentication
```

### 3. Configuration

Update the `application.yml` file with your Keycloak server details:

```yaml
keycloak:
auth-server-url: http://localhost:8080/auth
realm: your-realm
resource: your-client-id
credentials:
secret: your-client-secret
use-resource-role-mappings: true
bearer-only: true
security-constraints:
- authRoles:
- user
- admin
```

### 4. Build the Application

Use Maven or Gradle to build the project:

```bash
# For Maven
mvn clean install

# For Gradle
gradle build
```

### 5. Run the Application

```bash
java -jar target/keycloak-authentication-0.0.1-SNAPSHOT.jar
```

### 6. Test the Service

- Access protected resources by passing the JWT token issued by Keycloak.
- Example endpoint:
  ```
  GET /api/protected/resource
  ```
  Pass the token in the \`Authorization\` header:
  ```
  Authorization: Bearer <your-jwt-token>
  ```

## Keycloak Integration

This service integrates with Keycloak to provide token-based authentication and role-based access control (RBAC). It validates tokens issued by Keycloak and provides protected endpoints based on user roles.

### Keycloak Configuration Steps

1. **Realm:** Create a new realm.
2. **Client:** Create a client with `confidential` access type and set `redirect_uris`.
3. **Roles:** Define roles such as `user`, `admin`, etc.
4. **Users:** Create users and assign them roles.

### Securing Endpoints

The endpoints in the service are secured based on roles. You can configure role-based access by modifying `SecurityConfig.java`:

```text
http
.authorizeRequests()
.antMatchers("/api/protected/**")
.hasRole("user")
.anyRequest().permitAll();
```

## Docker Deployment (Optional)

You can containerize the service with Docker:

1. Build the Docker image:
   ```bash
   docker build -t keycloak-auth-service .
   ```
2. Run the Docker container:
   ```bash
   docker run -p 8080:8080 keycloak-auth-service
   ```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.