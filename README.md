# Spring Boot Blog API

A comprehensive RESTful Blog API built with Spring Boot 3.5.6, Java 21, PostgreSQL, and JWT authentication. This project demonstrates enterprise-level backend development with clean architecture, security best practices, and complete CRUD operations for a modern blogging platform.

## 🛠️ Tech Stack

- **Spring Boot 3.5.6**
- **Java 21**
- **PostgreSQL 16**
- **Spring Security 6.x**
- **JWT (JJWT 0.12.5)**
- **Spring Data JPA**
- **Hibernate**
- **Lombok**
- **Maven**
- **Docker**
- 
## 🚀 Features

- **JWT Authentication**: Secure token-based authentication with BCrypt password hashing
- **Role-Based Authorization**: USER and ADMIN roles with method-level security
- **Complete Blog Functionality**: Posts, Comments, Categories, Tags, and User management
- **Advanced Filtering**: Dynamic post filtering with JPA Specifications
- **RESTful API Design**: 35+ endpoints following REST principles
- **DTO Pattern**: Clean separation between entities and API contracts
- **Validation**: Comprehensive input validation using Jakarta Bean Validation
- **Exception Handling**: Global exception handler with meaningful error responses
- **Docker Support**: PostgreSQL setup with Docker Compose
- **Automatic Timestamps**: CreatedAt and UpdatedAt fields with JPA lifecycle hooks
- **SEO Friendly**: Slug-based post URLs for better search engine optimization

## 🗃️ Project Structure
```
src/main/java/com/berkedev/springbootblogapi/
├── controller/
│   ├── AuthController.java           # Authentication endpoints (login, register)
│   ├── UserController.java           # User management (ADMIN only)
│   ├── PostController.java           # Blog post operations
│   ├── CommentController.java        # Comment operations
│   ├── CategoryController.java       # Category management
│   └── TagController.java            # Tag management
├── service/
│   ├── UserService.java              # Service interface
│   ├── PostService.java
│   ├── CommentService.java
│   ├── CategoryService.java
│   ├── TagService.java
│   └── impl/
│       ├── UserServiceImpl.java      # Service implementations
│       ├── PostServiceImpl.java
│       ├── CommentServiceImpl.java
│       ├── CategoryServiceImpl.java
│       ├── TagServiceImpl.java
│       └── CustomUserDetailsService.java  # Spring Security integration
├── data/
│   ├── entity/
│   │   ├── User.java                 # User entity (implements UserDetails)
│   │   ├── Post.java                 # Blog post entity
│   │   ├── Comment.java              # Comment entity
│   │   ├── Category.java             # Category entity
│   │   └── Tag.java                  # Tag entity
│   ├── repository/
│   │   ├── UserRepository.java       # JPA repositories
│   │   ├── PostRepository.java
│   │   ├── CommentRepository.java
│   │   ├── CategoryRepository.java
│   │   └── TagRepository.java
│   ├── dto/
│   │   ├── request/
│   │   │   ├── RegisterRequest.java  # Registration DTO
│   │   │   ├── LoginRequest.java     # Login DTO
│   │   │   ├── PostCreateRequest.java
│   │   │   ├── PostUpdateRequest.java
│   │   │   ├── PostFilterRequest.java # Dynamic filtering
│   │   │   ├── CommentCreateRequest.java
│   │   │   ├── CategoryCreateRequest.java
│   │   │   ├── TagCreateRequest.java
│   │   │   ├── UserCreateRequest.java
│   │   │   └── UserUpdateRequest.java
│   │   └── response/
│   │       ├── AuthResponse.java     # JWT token response
│   │       ├── UserResponse.java
│   │       ├── PostResponse.java
│   │       ├── CommentResponse.java
│   │       ├── CategoryResponse.java
│   │       └── TagResponse.java
│   ├── mapper/
│   │   ├── UserMapper.java           # Manual Entity-DTO mappers
│   │   ├── PostMapper.java
│   │   ├── CommentMapper.java
│   │   ├── CategoryMapper.java
│   │   └── TagMapper.java
│   ├── specification/
│   │   └── PostSpecification.java    # Dynamic query building
│   └── enums/
│       └── Role.java                 # USER, ADMIN roles
├── security/
│   ├── JwtService.java               # JWT token operations
│   ├── JwtAuthenticationFilter.java  # Request filtering
│   └── SecurityConfig.java           # Security configuration
├── exception/
│   ├── BlogApiException.java         # Base exception
│   ├── ResourceNotFoundException.java # 404 exceptions
│   ├── DuplicateResourceException.java # 409 exceptions
│   ├── BadRequestException.java      # 400 exceptions
│   ├── GlobalExceptionHandler.java   # Global error handling
│   └── dto/
│       ├── ErrorResponse.java
│       └── ValidationErrorResponse.java
└── SpringBootBlogApiApplication.java # Main application class
```

## 🎓 What I Learned in This Project
### 🔐 Spring Security & JWT Authentication

#### **1. JWT (JSON Web Token) Implementation**
- **Token Structure**: Understanding the three parts of JWT (Header, Payload, Signature)
- **Token Generation**: Using JJWT library to create secure tokens with HMAC-SHA256 signing
- **Token Validation**: Extracting and verifying token signature and expiration
- **Claims Management**: Storing user information (username, role) in JWT payload
- **Stateless Authentication**: No server-side session storage for better scalability

#### **2. BCrypt Password Hashing**
- **Password Security**: Never storing plain-text passwords in the database
- **Salt Generation**: BCrypt automatically generates unique salts for each password
- **One-Way Hashing**: Understanding that BCrypt hashing cannot be reversed
- **Password Verification**: Using `PasswordEncoder.matches()` to verify login credentials
- **Strength Configuration**: Default strength of 10 rounds for optimal security/performance balance

#### **3. Spring Security 6.x Architecture**
- **SecurityFilterChain**: Modern Spring Security 6.x configuration (no WebSecurityConfigurerAdapter)
- **Filter Chain Order**: Understanding how filters execute in sequence
- **OncePerRequestFilter**: Ensuring JWT filter runs exactly once per request
- **SecurityContext**: Managing authenticated user information throughout request lifecycle
- **AuthenticationManager**: Delegating authentication to configured providers

#### **4. UserDetails & UserDetailsService**
- **UserDetails Interface**: Making User entity implement Spring Security's UserDetails
- **getAuthorities()**: Converting user roles to Spring Security's GrantedAuthority
- **Account Status Methods**: isAccountNonExpired, isAccountNonLocked, isCredentialsNonExpired, isEnabled
- **CustomUserDetailsService**: Loading users from database for authentication
- **Database Integration**: Connecting Spring Security with JPA repositories

#### **5. Role-Based Authorization (RBAC)**
- **Role Enum**: Creating USER and ADMIN roles for access control
- **Endpoint-Level Security**: Using `.requestMatchers()` to protect specific endpoints
- **HTTP Method-Based Rules**: Different access levels for GET vs POST/PUT/DELETE
- **hasRole() vs hasAuthority()**: Understanding the "ROLE_" prefix convention
- **Public vs Protected Endpoints**: Configuring which endpoints require authentication

#### **6. Authentication Flow**
- **Registration Process**: User creation → Password BCrypt hashing → Save to database → Generate JWT token
- **Login Process**: Username/password → AuthenticationManager → Verify credentials → Generate JWT token
- **Request Authentication**: JWT in Authorization header → Filter extracts token → Validate → Load user → Set SecurityContext
- **Current User Injection**: Getting authenticated user with `SecurityContextHolder.getContext().getAuthentication()`

#### **7. JWT Filter Implementation**
- **Filter Execution Order**: Adding JwtAuthenticationFilter before UsernamePasswordAuthenticationFilter
- **Authorization Header Parsing**: Extracting "Bearer " prefix and token
- **Token Validation Logic**: Checking signature, expiration, and username match
- **SecurityContext Population**: Setting authentication for downstream filters and controllers
- **Error Handling**: Gracefully handling invalid or expired tokens

#### **8. Security Best Practices**
- **CSRF Protection**: Disabling CSRF for JWT-based stateless APIs
- **Session Management**: Configuring STATELESS session policy
- **Password Encoding**: Injecting PasswordEncoder as a Spring bean
- **Auto-Configuration**: Leveraging Spring Security 6.x auto-configuration for DaoAuthenticationProvider
- **Avoiding Deprecated APIs**: Using modern Spring Security 6.x patterns (no deprecated methods)

#### **9. SecurityConfig Configuration**
- **Bean Definition**: Creating SecurityFilterChain, PasswordEncoder, and AuthenticationManager beans
- **Lambda-Based Configuration**: Using modern lambda syntax for cleaner config
- **Authorization Rules**: Defining which endpoints are public, authenticated, or admin-only
- **Filter Registration**: Adding custom filters to the security filter chain
- **Cross-Cutting Concerns**: Handling CORS, CSRF, and session management

#### **10. Integration with Application Layers**
- **Service Layer**: Injecting PasswordEncoder in UserServiceImpl for password hashing
- **Controller Layer**: Creating AuthController for login/register endpoints
- **Entity Layer**: User entity implementing UserDetails for Spring Security integration
- **Repository Layer**: Finding users by username for authentication
- **Current User Context**: Accessing authenticated user in service methods with SecurityContextHolder

### 🔍 Spring Data JPA & Advanced Querying

#### **11. JPA Specifications for Dynamic Filtering**
- **Criteria API**: Using JPA Criteria API for type-safe dynamic queries
- **Specification Interface**: Implementing `Specification<T>` for reusable query logic
- **Predicate Building**: Creating dynamic predicates with `CriteriaBuilder`
- **Multiple Filters**: Combining multiple filter conditions with AND logic
- **JpaSpecificationExecutor**: Extending repository with `JpaSpecificationExecutor<Post>`
- **Null Safety**: Handling optional filter parameters gracefully
- **Join Operations**: Performing joins across entity relationships (author, category, tag)
- **Dynamic WHERE Clauses**: Building SQL WHERE clauses programmatically based on input
- **Type Safety**: Compile-time checking with Metamodel and type-safe queries
- **Reusability**: Creating reusable specification methods for common queries 

### 💡 Key Security Concepts Mastered

- **Stateless Authentication**: Understanding why JWT is better than sessions for scalability
- **Token Expiration**: Configuring and validating token expiration times (24 hours default)
- **Password Storage**: Never storing plain-text passwords, always using BCrypt
- **Authorization vs Authentication**: Clear distinction between "who you are" and "what you can do"
- **Principal**: Understanding the authenticated user object in Spring Security
- **Credentials**: Knowing when to include/exclude passwords (null after authentication)
- **Security Context**: Thread-local storage for authenticated user information
- **Filter Chain**: How Spring Security processes requests through multiple filters
- **Method Security**: Foundation for future @PreAuthorize implementation
- **Production Readiness**: Using environment variables for JWT secret keys in production

## 🔒 Security Features

- **JWT Authentication**: Stateless token-based authentication (24-hour expiration)
- **BCrypt Password Hashing**: Secure password storage with salt generation
- **Role-Based Access Control**: USER and ADMIN roles with endpoint-level authorization
- **SecurityContext Integration**: Automatic current user injection in service methods
- **Protected Endpoints**: Fine-grained access control on all sensitive operations
- **Modern Security Configuration**: Spring Security 6.x best practices (no deprecated APIs)
- **Stateless Sessions**: No server-side session storage for better scalability
- **CSRF Protection**: Disabled for JWT-based API (tokens in Authorization header)
- **Filter-Based Validation**: Every request validated through JwtAuthenticationFilter
- **Automatic Password Encoding**: All passwords hashed before database storage

## 🗄️ Database Schema

### Users Table
| Column      | Type            | Constraints       |
|-------------|-----------------|-------------------|
| id          | BIGINT          | PRIMARY KEY, AUTO |
| username    | VARCHAR         | UNIQUE, NOT NULL  |
| email       | VARCHAR         | UNIQUE, NOT NULL  |
| password    | VARCHAR         | NOT NULL (BCrypt) |
| full_name   | VARCHAR         | NULLABLE          |
| role        | VARCHAR(20)     | NOT NULL (USER/ADMIN) |
| created_at  | TIMESTAMP       | NOT NULL          |
| updated_at  | TIMESTAMP       | NULLABLE          |

### Posts Table
| Column        | Type            | Constraints       |
|---------------|-----------------|-------------------|
| id            | BIGINT          | PRIMARY KEY, AUTO |
| title         | VARCHAR         | NOT NULL          |
| slug          | VARCHAR         | UNIQUE, NOT NULL  |
| content       | TEXT            | NOT NULL          |
| published     | BOOLEAN         | NOT NULL, DEFAULT false |
| published_at  | TIMESTAMP       | NULLABLE          |
| created_at    | TIMESTAMP       | NOT NULL          |
| updated_at    | TIMESTAMP       | NULLABLE          |
| user_id       | BIGINT          | FK → users.id     |
| category_id   | BIGINT          | FK → categories.id |

### Comments Table
| Column      | Type            | Constraints       |
|-------------|-----------------|-------------------|
| id          | BIGINT          | PRIMARY KEY, AUTO |
| content     | TEXT            | NOT NULL          |
| created_at  | TIMESTAMP       | NOT NULL          |
| updated_at  | TIMESTAMP       | NULLABLE          |
| user_id     | BIGINT          | FK → users.id     |
| post_id     | BIGINT          | FK → posts.id     |

### Categories Table
| Column      | Type            | Constraints       |
|-------------|-----------------|-------------------|
| id          | BIGINT          | PRIMARY KEY, AUTO |
| name        | VARCHAR         | UNIQUE, NOT NULL  |
| description | VARCHAR         | NULLABLE          |
| created_at  | TIMESTAMP       | NOT NULL          |
| updated_at  | TIMESTAMP       | NULLABLE          |

### Tags Table
| Column      | Type            | Constraints       |
|-------------|-----------------|-------------------|
| id          | BIGINT          | PRIMARY KEY, AUTO |
| name        | VARCHAR         | UNIQUE, NOT NULL  |
| created_at  | TIMESTAMP       | NOT NULL          |
| updated_at  | TIMESTAMP       | NULLABLE          |

### Post_Tags Table (Many-to-Many)
| Column  | Type   | Constraints       |
|---------|--------|-------------------|
| post_id | BIGINT | FK → posts.id     |
| tag_id  | BIGINT | FK → tags.id      |

---

## 🚦 Getting Started

### Prerequisites
- Java 21 or higher
- Docker and Docker Compose
- Maven 3.x
---

## 🔒 Authorization Matrix

| Endpoint Pattern | Method | Public | USER | ADMIN |
|-----------------|--------|--------|------|-------|
| /api/auth/**    | ALL    | ✅     | ✅   | ✅    |
| /api/posts      | GET    | ✅     | ✅   | ✅    |
| /api/posts      | POST   | ❌     | ✅   | ✅    |
| /api/posts/{id} | PUT    | ❌     | ✅   | ✅    |
| /api/posts/{id} | DELETE | ❌     | ✅   | ✅    |
| /api/comments   | GET    | ✅     | ✅   | ✅    |
| /api/comments   | POST   | ❌     | ✅   | ✅    |
| /api/categories | GET    | ✅     | ✅   | ✅    |
| /api/categories | POST   | ❌     | ❌   | ✅    |
| /api/tags       | GET    | ✅     | ✅   | ✅    |
| /api/tags       | POST   | ❌     | ❌   | ✅    |
| /api/users/**   | ALL    | ❌     | ❌   | ✅    |

---

## 📝 Notes

- The application uses **Hibernate's ddl-auto: update** for development. For production, consider using database migration tools like **Flyway** or **Liquibase**.
- All timestamps are configured for **Europe/Istanbul** timezone.
- JWT tokens expire after **24 hours** (configurable in application.yml).
- Passwords are hashed using **BCrypt** with automatic salt generation.
- The application includes SQL logging for debugging (disable in production).
- ID generation uses **PostgreSQL sequences** for better performance.
- **Stateless authentication** - no server-side sessions are used.
- Post slugs are used for **SEO-friendly URLs**.
- JWT secret key should be stored in **environment variables** in production.
- Spring Security 6.x modern configuration - **no deprecated APIs**.

## 📚 API Endpoints

### 🔓 Authentication (Public)

#### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "password123",
  "fullName": "John Doe"
}
```

**Response**: `201 CREATED`
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "message": "User registered successfully"
}
```

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "username": "johndoe",
  "password": "password123"
}
```

**Response**: `200 OK`
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "message": "Login successful"
}
```

---

### 📝 Posts

#### Get All Posts (Public)
```http
GET /api/posts
GET /api/posts?authorId=1&categoryId=2&tagId=3&published=true
```

**Response**: `200 OK`
```json
[
  {
    "id": 1,
    "title": "Getting Started with Spring Boot",
    "slug": "getting-started-with-spring-boot",
    "content": "Spring Boot makes it easy to create...",
    "published": true,
    "publishedAt": "2024-01-15T10:30:00",
    "createdAt": "2024-01-15T10:00:00",
    "updatedAt": null,
    "author": {
      "id": 1,
      "username": "johndoe",
      "fullName": "John Doe"
    },
    "category": {
      "id": 1,
      "name": "Programming"
    },
    "tags": [
      { "id": 1, "name": "Java" },
      { "id": 2, "name": "Spring" }
    ]
  }
]
```

#### Get Post by ID (Public)
```http
GET /api/posts/{id}
```

**Response**: `200 OK`

#### Create Post (Authenticated - USER/ADMIN)
```http
POST /api/posts
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "title": "My New Blog Post",
  "slug": "my-new-blog-post",
  "content": "This is the content of my blog post...",
  "categoryId": 1,
  "tagIds": [1, 2, 3],
  "published": false
}
```

**Response**: `201 CREATED`

#### Update Post (Authenticated - USER/ADMIN)
```http
PUT /api/posts/{id}
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "title": "Updated Title",
  "content": "Updated content...",
  "published": true
}
```

**Response**: `200 OK`

#### Delete Post (Authenticated - USER/ADMIN)
```http
DELETE /api/posts/{id}
Authorization: Bearer {jwt_token}
```

**Response**: `204 NO CONTENT`

#### Publish/Unpublish Post (Authenticated - USER/ADMIN)
```http
PUT /api/posts/{id}/publish
PUT /api/posts/{id}/unpublish
Authorization: Bearer {jwt_token}
```

**Response**: `200 OK`

---

### 💬 Comments

#### Get All Comments (Public)
```http
GET /api/comments
GET /api/comments/post/{postId}
GET /api/comments/author/{authorId}
```

**Response**: `200 OK`

#### Create Comment (Authenticated - USER/ADMIN)
```http
POST /api/comments
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "content": "Great article! Thanks for sharing.",
  "postId": 1
}
```

**Response**: `201 CREATED`

#### Delete Comment (Authenticated - USER/ADMIN)
```http
DELETE /api/comments/{id}
Authorization: Bearer {jwt_token}
```

**Response**: `204 NO CONTENT`

---

### 📁 Categories (Read: Public, Write: ADMIN only)

#### Get All Categories
```http
GET /api/categories
GET /api/categories/{id}
GET /api/categories/name/{name}
GET /api/categories/search?keyword=programming
```

**Response**: `200 OK`

#### Create Category (ADMIN only)
```http
POST /api/categories
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "name": "Programming",
  "description": "All about programming and software development"
}
```

**Response**: `201 CREATED`

#### Delete Category (ADMIN only)
```http
DELETE /api/categories/{id}
Authorization: Bearer {jwt_token}
```

**Response**: `204 NO CONTENT`

---

### 🏷️ Tags (Read: Public, Write: ADMIN only)

#### Get All Tags
```http
GET /api/tags
GET /api/tags/{id}
GET /api/tags/name/{name}
GET /api/tags/search?keyword=java
```

**Response**: `200 OK`

#### Create Tag (ADMIN only)
```http
POST /api/tags
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "name": "Java"
}
```

**Response**: `201 CREATED`

---

### 👤 Users (ADMIN only)

#### Get All Users
```http
GET /api/users
GET /api/users/{id}
GET /api/users/username/{username}
GET /api/users/email/{email}
Authorization: Bearer {jwt_token}
```

**Response**: `200 OK`

#### Update User
```http
PUT /api/users/{id}
Authorization: Bearer {jwt_token}
Content-Type: application/json

{
  "email": "newemail@example.com",
  "fullName": "John Updated Doe"
}
```

**Response**: `200 OK`

#### Delete User
```http
DELETE /api/users/{id}
Authorization: Bearer {jwt_token}
```

**Response**: `204 NO CONTENT`

---

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/spring-boot-blog-api.git
cd spring-boot-blog-api
```

2. **Start PostgreSQL with Docker**
```bash
docker-compose up -d
```

3. **Configure application (optional)**

Edit `src/main/resources/application.yml` if needed:
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5434/blog_db
    username: blog_user
    password: secret

jwt:
  secret-key: your-secret-key-here
  expiration: 86400000  # 24 hours
```

4. **Build the project**
```bash
mvn clean install
```

5. **Run the application**
```bash
mvn spring-boot:run
```

The API will be available at `http://localhost:8081`

### Quick Test

1. **Register a new user**
```bash
curl -X POST http://localhost:8081/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "email": "test@example.com",
    "password": "password123",
    "fullName": "Test User"
  }'
```

2. **Login and get JWT token**
```bash
curl -X POST http://localhost:8081/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "password": "password123"
  }'
```

3. **Use the token to create a post**
```bash
curl -X POST http://localhost:8081/api/posts \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN_HERE" \
  -d '{
    "title": "My First Post",
    "slug": "my-first-post",
    "content": "This is my first blog post!",
    "categoryId": 1,
    "tagIds": [1],
    "published": true
  }'
```



## 👨‍💻 Author

**Berke Genç**
- GitHub: [@gencberke](https://github.com/gencberke)

