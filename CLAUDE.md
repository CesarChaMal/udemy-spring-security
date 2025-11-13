# CLAUDE.md - AI Assistant Guide for Spring Security Course Repository

## Repository Overview

This is a **Packt/Udemy Spring Security 3.1 Course** repository containing progressive tutorial modules demonstrating Spring Security concepts. The course is structured as independent WAR modules, each representing a video lesson building upon previous concepts.

**Course Focus**: Spring Security 3.1.x with Spring Framework 3.2.x (legacy versions maintained for educational stability)

**Repository Type**: Educational/Tutorial code with section-based progression

**Last Updated**: 2025-11-13

---

## Directory Structure

```
udemy-spring-security/
├── README.md                    # Course overview and quick start
├── CLAUDE.md                    # This file - AI assistant guide
│
├── section1/                    # Spring Security Setup (5 videos)
│   ├── s1_vid1/                # Basic security setup
│   ├── s1_vid2/                # In-memory authentication
│   ├── s1_vid3/                # Form-based login
│   ├── s1_vid4/                # Custom login page
│   └── s1_vid5/                # Logout functionality
│
├── section2/                    # Registration (3+ videos)
│   ├── s2_vid1/                # User registration setup
│   ├── s2_vid2/                # Database-backed authentication
│   └── s2_vid3/                # Registration completion
│
├── section3/                    # Remember Me Authentication (4 videos)
│   ├── s3_vid1/                # Remember-me basics
│   ├── s3_vid2/                # SpEL expressions introduction
│   ├── s3_vid3/                # Advanced expressions
│   └── s3_vid4/                # isFullyAuthenticated() usage
│
├── section4/                    # Spring Security with LDAP (3+ videos)
│   ├── s4_vid1/                # LDAP introduction
│   ├── s4_vid2/                # LDAP configuration
│   └── s4_vid3/                # Hybrid LDAP/DB authentication
│
├── section5/                    # Authorization with Spring Expressions (3+ videos)
│   ├── s5_vid1/                # Method-level security
│   ├── s5_vid2/                # @Secured annotations
│   └── s5_vid3/                # Custom SpEL expressions
│
├── section6/                    # REST Authentication and Authorization (3+ videos)
│   ├── s6_vid1/                # REST security basics
│   ├── s6_vid2/                # Stateless authentication
│   └── s6_vid3/                # REST API testing
│
└── section7/                    # Spring Security ACL (2-4 videos)
    ├── s7_vid1/                # ACL introduction
    ├── s7_vid2/                # Object-level permissions
    └── s7_vid3/                # Advanced ACL usage
```

### Module Naming Convention
- **Pattern**: `s{section_number}_vid{video_number}`
- **Variants**: Some modules include `_done` suffix indicating completed implementations
- **Independence**: Each video module is a complete, standalone WAR project

---

## Technology Stack

### Core Frameworks (Consistent Across Modules)

| Technology | Version | Purpose |
|-----------|---------|---------|
| Spring Framework | 3.2.0 - 3.2.2.RELEASE | MVC, DI, AOP foundation |
| Spring Security | 3.1.3 - 3.1.4.RELEASE | Authentication & authorization |
| Java | 6/7 era (legacy) | Target runtime |
| Servlet Spec | 3.0 | Web container |

### Dependencies by Category

**Spring Modules**:
- `spring-webmvc` - Web MVC framework
- `spring-core`, `spring-context`, `spring-beans` - Core DI/IoC
- `spring-aop` - Aspect-oriented programming
- `spring-jdbc`, `spring-orm`, `spring-tx` - Persistence & transactions
- `spring-expression` - SpEL support (sections 3+)

**Spring Security Modules**:
- `spring-security-web` - Web security filters
- `spring-security-config` - Security configuration
- `spring-security-taglibs` - JSP security tags
- `spring-security-ldap` - LDAP authentication (section 4+)
- `spring-security-acl` - Access Control Lists (section 7)

**Persistence**:
- Hibernate `4.1.9 - 4.2.1` - JPA implementation
- Spring Data JPA `1.3.x` - Repository pattern
- MySQL Connector `5.1.2x` - Database driver (runtime)
- HikariCP or C3P0 - Connection pooling (varies by module)

**Data Marshalling**:
- Jackson `2.1.2` - JSON serialization/deserialization
- JAXB API - XML binding for REST responses

**Utilities**:
- Guava `13.x - 14.x` - Google core libraries
- Apache Commons Lang3 `3.1` - Utility functions
- SLF4J `1.7.x` + Logback `1.0.x` - Logging

**Testing**:
- JUnit `4.11` - Test framework
- Hamcrest `1.3` - Assertion matchers
- Mockito `1.9.5` - Mocking framework
- REST Assured `1.8.0` - REST API testing (section 6+)

---

## Architecture Patterns

### Backend/Frontend Separation

The codebase maintains **strict separation of concerns** between backend and frontend:

```
Backend Package: com.packt.springsecurity.backend.*
├── controller/              REST/API controllers
│   ├── UserController       User CRUD operations
│   ├── RegistrationController (section 2+)
│   └── [Other domain controllers]
│
├── model/                   Domain entities (JPA)
│   ├── User                 Core user entity
│   ├── Role/Authority       Security authorities
│   └── [Domain entities]
│
├── persistence/             Data access layer
│   ├── dao/                Spring Data JPA repositories
│   ├── service/            Business logic services
│   └── model/              Additional persistence models
│
├── security/                Security components
│   ├── CustomUserService   UserDetailsService implementation
│   └── SecurityUtil        Security helper methods
│
├── dto/                     Data Transfer Objects
│   └── RegistrationDto      (section 2+)
│
└── spring/                  Backend configuration
    ├── BackendConfig        Component scanning
    └── BackendPersistenceConfig  JPA/Database config

Frontend Package: com.packt.springsecurity.frontend.*
├── web/
│   └── controller/         UI controllers
│       └── WebMediatorController  Form/page controllers
│
└── spring/                 Frontend configuration
    ├── ClientWebConfig     MVC configuration
    ├── ClientContextConfig Context setup
    └── ClientSecurityConfig Security config import

Common Package: com.packt.springsecurity.common.*
└── spring/
    └── SecurityConfig      Shared security configuration
```

### Design Principles
1. **Backend** handles data, business logic, and REST APIs
2. **Frontend** handles UI presentation and user interaction
3. **No persistence** in frontend packages
4. **DTO pattern** for complex data transfer (section 2+)
5. **Service layer** provides transactional boundaries

### MVC Pattern Implementation

**Controllers**:
- Backend controllers: REST endpoints returning JSON/XML
- Frontend controllers: Form handling, page navigation

**Services**:
- Annotated with `@Service` and `@Transactional`
- Business logic encapsulation
- Repository orchestration

**Data Access**:
- Spring Data JPA repositories (interface-based)
- No custom DAO implementations needed
- Query methods derived from method names

**Example Data Flow**:
```
HTTP Request
  → DispatcherServlet (Spring MVC)
    → SecurityFilterChain (Spring Security)
      → Controller (Backend or Frontend)
        → Service (@Transactional)
          → Repository (Spring Data JPA)
            → Database (MySQL)
```

---

## Configuration Approaches

### Hybrid XML + Java Configuration

The repository uses a **pragmatic mix** of XML and Java configuration:

**Java Configuration** (Primary for MVC/Web):
```java
@Configuration
@ComponentScan({"com.packt.springsecurity.backend"})
@Import({BackendPersistenceConfig.class})
public class BackendConfig {
    // Bean definitions if needed
}

@Configuration
@EnableWebMvc
@ComponentScan({"com.packt.springsecurity.frontend.web"})
public class ClientWebConfig {
    // View resolvers, resource handlers, etc.
}
```

**XML Configuration** (Primary for Security):
```xml
<!-- webSecurityConfig.xml -->
<security:http use-expressions="true">
    <security:intercept-url pattern="/admin/**"
                           access="hasRole('ROLE_ADMIN')" />
    <security:form-login login-page="/login.html"
                        authentication-failure-url="/login.html?error=true" />
    <security:logout logout-success-url="/login.html" />
    <security:remember-me key="myAppKey" />  <!-- Section 3+ -->
</security:http>

<security:authentication-manager>
    <security:authentication-provider user-service-ref="customUserService">
        <security:password-encoder ref="passwordEncoder" />
    </security:authentication-provider>
</security:authentication-manager>
```

### Key Configuration Files

| File | Purpose | Format |
|------|---------|--------|
| `web.xml` | Servlet container configuration, Spring context initialization | XML |
| `clientapp-servlet.xml` | DispatcherServlet configuration, component scanning | XML |
| `webSecurityConfig.xml` | Security rules, authentication, authorization | XML |
| `persistenceConfig.xml` | JPA repositories, datasource, transactions | XML |
| `*Config.java` | Java-based configuration classes | Java |
| `*.properties` | Environment-specific properties | Properties |
| `logback.xml` | Logging configuration | XML |

### Spring Profiles

Modules use profiles to activate different configurations:
```xml
<context-param>
    <param-name>spring.profiles.active</param-name>
    <param-value>sec_client,client</param-value>
</context-param>
```

Common profiles:
- `sec_client` - Security client configuration
- `client` - Frontend client configuration
- `dev` - Development settings
- `test` - Testing configuration

---

## Development Workflow

### Building a Module

Each module is an independent Maven WAR project:

```bash
# Navigate to specific module
cd section1/s1_vid1

# Clean and build
mvn clean install

# Run tests only
mvn test

# Package without tests
mvn package -DskipTests

# Deploy to local Tomcat (if maven-tomcat plugin configured)
mvn tomcat7:run
```

### Database Setup

Most modules (section 2+) require MySQL:

1. **Create Database**:
```sql
CREATE DATABASE spring_security_course;
CREATE USER 'springuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON spring_security_course.* TO 'springuser'@'localhost';
FLUSH PRIVILEGES;
```

2. **Configure Datasource** (in `persistenceConfig.xml` or properties):
```xml
<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/spring_security_course"/>
    <property name="username" value="springuser"/>
    <property name="password" value="password"/>
</bean>
```

3. **Schema Generation**: Hibernate auto-creates tables on first run
```xml
<property name="hibernate.hbm2ddl.auto" value="create-drop"/>
```

### Running the Application

1. **Build the WAR**: `mvn clean package`
2. **Deploy** to Tomcat/Jetty/other servlet container
3. **Access**: `http://localhost:8080/spring-security/`
4. **Default credentials** (varies by module):
   - Section 1: `user/password` or `admin/password` (in-memory)
   - Section 2+: Register new user or check seed data

### Testing

```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=UserControllerTest

# Integration tests (section 6+)
mvn verify
```

---

## Key Conventions

### Package Naming
- **Base package**: `com.packt.springsecurity`
- **Backend**: `com.packt.springsecurity.backend.*`
- **Frontend**: `com.packt.springsecurity.frontend.*`
- **Common**: `com.packt.springsecurity.common.*`

### Class Naming Conventions

| Type | Suffix | Example | Annotation |
|------|--------|---------|------------|
| Entity | (none) | `User`, `Role` | `@Entity` |
| DTO | `Dto` | `RegistrationDto` | (none) |
| Repository | `DAO`, `Repository` | `IUserJpaDAO` | `@Repository` (auto) |
| Service Interface | `I*Service` | `IUserService` | (none) |
| Service Impl | `*Service` | `UserService` | `@Service` |
| Controller | `Controller` | `UserController` | `@Controller` / `@RestController` |
| Config | `Config` | `BackendConfig` | `@Configuration` |

### Entity Patterns

Standard entity structure across modules:

```java
@Entity
@Table(name = "user_account")  // Avoid SQL reserved words
@XmlRootElement  // REST compatibility
public class User implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @Column(unique = true, nullable = false)
    private String name;

    @Column(nullable = false)
    private String password;  // Always hashed, never plaintext

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(name = "user_authority",
               joinColumns = @JoinColumn(name = "user_id"),
               inverseJoinColumns = @JoinColumn(name = "authority_id"))
    private Set<Authority> authorities = new HashSet<>();

    private Boolean suspended = false;

    // Standard getters/setters
    // Override equals() and hashCode() based on business key
}
```

### Repository Pattern

Spring Data JPA interface-based repositories:

```java
public interface IUserJpaDAO extends JpaRepository<User, Long> {

    User findByName(String name);  // Query derived from method name

    @Query("SELECT u FROM User u WHERE u.name = :name AND u.suspended = false")
    User findActiveUserByName(@Param("name") String name);

    // No implementation needed - Spring Data provides it
}
```

### Service Layer Pattern

```java
public interface IUserService {
    User findByName(String name);
    void create(User user);
    List<User> findAll();
}

@Service
@Transactional  // Class-level transaction demarcation
public class UserService implements IUserService {

    @Autowired
    private IUserJpaDAO userDao;

    @Autowired
    private PasswordEncoder passwordEncoder;

    @Override
    public User findByName(String name) {
        return userDao.findByName(name);
    }

    @Override
    public void create(User user) {
        user.setPassword(passwordEncoder.encode(user.getPassword()));
        userDao.save(user);
    }
}
```

### Security Configuration Evolution

**Section 1 (Basic)**:
```xml
<security:http>
    <security:intercept-url pattern="/admin/**" access="ROLE_ADMIN" />
    <security:form-login />
</security:http>

<security:authentication-manager>
    <security:authentication-provider>
        <security:user-service>
            <security:user name="user" password="password" authorities="ROLE_USER" />
        </security:user-service>
    </security:authentication-provider>
</security:authentication-manager>
```

**Section 2+ (Database)**:
```xml
<security:authentication-manager>
    <security:authentication-provider user-service-ref="customUserService">
        <security:password-encoder ref="passwordEncoder" />
    </security:authentication-provider>
</security:authentication-manager>

<bean id="passwordEncoder"
      class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder" />
```

**Section 3+ (Remember Me + SpEL)**:
```xml
<security:http use-expressions="true">
    <security:intercept-url pattern="/admin/**"
                           access="hasRole('ROLE_ADMIN') and isFullyAuthenticated()" />
    <security:remember-me key="myAppKey" token-validity-seconds="86400" />
</security:http>
```

**Section 5+ (Method Security)**:
```xml
<security:global-method-security secured-annotations="enabled"
                                 pre-post-annotations="enabled" />
```

```java
@Secured("ROLE_ADMIN")
public void deleteUser(Long id) { }

@PreAuthorize("hasRole('ROLE_ADMIN') or #user.name == principal.username")
public void updateUser(User user) { }

@PostAuthorize("hasPermission(returnObject, 'read')")  // Section 7 ACL
public Possession findPossession(Long id) { }
```

---

## Common Patterns by Section

### Section 1: Spring Security Setup
**Key Concepts**: Filter chain, in-memory authentication, basic authorization

**Files to Focus On**:
- `web.xml` - `springSecurityFilterChain` filter registration
- `webSecurityConfig.xml` - Basic security namespace configuration
- `ClientSecurityConfig.java` - Java config importing XML

**Common Changes**:
- Adding new URL patterns to `<intercept-url>`
- Configuring custom login pages
- Adding in-memory users

### Section 2: Registration
**Key Concepts**: User registration, database-backed authentication, password encoding

**New Components**:
- `RegistrationController` - Handles signup forms
- `RegistrationDto` - Data transfer object for registration
- `CustomUserService implements UserDetailsService` - Loads users from database
- Password encoder bean (BCrypt)

**Files to Focus On**:
- `backend/controller/RegistrationController.java`
- `backend/dto/RegistrationDto.java`
- `backend/security/CustomUserService.java`
- `persistenceConfig.xml` - Database and JPA configuration

**Database Schema**:
```sql
CREATE TABLE user_account (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    suspended BOOLEAN DEFAULT FALSE
);

CREATE TABLE authority (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE user_authority (
    user_id BIGINT,
    authority_id BIGINT,
    PRIMARY KEY (user_id, authority_id)
);
```

### Section 3: Remember Me Authentication
**Key Concepts**: Persistent login, SpEL expressions, authentication levels

**Configuration Changes**:
```xml
<!-- Enable SpEL -->
<security:http use-expressions="true">

    <!-- Remember me with token -->
    <security:remember-me key="myAppKey"
                         token-validity-seconds="86400"
                         user-service-ref="customUserService" />

    <!-- Distinguish full authentication from remember-me -->
    <security:intercept-url pattern="/admin/**"
        access="hasRole('ROLE_ADMIN') and isFullyAuthenticated()" />
</security:http>
```

**SpEL Expressions Introduced**:
- `hasRole('ROLE_ADMIN')` - Role check
- `hasAnyRole('ROLE_ADMIN', 'ROLE_USER')` - Multiple roles
- `isFullyAuthenticated()` - Not remember-me
- `isAnonymous()` - Not authenticated
- `principal.username` - Access principal properties

### Section 4: LDAP Authentication
**Key Concepts**: LDAP authentication, ApacheDS embedded server, hybrid auth

**Dependencies Added**:
```xml
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-ldap</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.directory.server</groupId>
    <artifactId>apacheds-server-jndi</artifactId>
</dependency>
```

**Configuration**:
```xml
<security:authentication-manager>
    <!-- LDAP provider -->
    <security:ldap-authentication-provider
        user-dn-pattern="uid={0},ou=people"
        group-search-base="ou=groups"
        server-ref="ldapServer" />

    <!-- Fallback database provider -->
    <security:authentication-provider user-service-ref="customUserService">
        <security:password-encoder ref="passwordEncoder" />
    </security:authentication-provider>
</security:authentication-manager>

<security:ldap-server id="ldapServer"
                      ldif="classpath:users.ldif"
                      root="dc=springframework,dc=org" />
```

### Section 5: Authorization with Spring Expressions
**Key Concepts**: Method-level security, complex SpEL, custom expressions

**Enable Method Security**:
```xml
<security:global-method-security
    secured-annotations="enabled"
    pre-post-annotations="enabled"
    jsr250-annotations="enabled" />
```

**Annotation Examples**:
```java
// Simple role check
@Secured("ROLE_ADMIN")
public void deleteUser(Long id) { }

// Multiple roles (OR logic)
@Secured({"ROLE_ADMIN", "ROLE_SUPER_ADMIN"})
public void criticalOperation() { }

// Pre-authorization with SpEL
@PreAuthorize("hasRole('ROLE_ADMIN')")
public List<User> getAllUsers() { }

// Check object ownership
@PreAuthorize("#user.name == authentication.principal.username or hasRole('ROLE_ADMIN')")
public void updateUser(User user) { }

// Post-authorization (evaluate after method)
@PostAuthorize("returnObject.owner == authentication.principal.username")
public Document getDocument(Long id) { }

// JSR-250 annotations
@RolesAllowed("ROLE_ADMIN")
public void jsr250Example() { }
```

**Custom Security Expression**:
```java
@Component("customSecurityExpression")
public class CustomSecurityExpression {
    public boolean isOwner(Authentication authentication, Long userId) {
        // Custom logic
    }
}

// Usage in annotation
@PreAuthorize("@customSecurityExpression.isOwner(authentication, #userId)")
public void updateUser(Long userId) { }
```

### Section 6: REST Authentication and Authorization
**Key Concepts**: Stateless authentication, REST APIs, JSON responses, API testing

**Dependencies Added**:
```xml
<dependency>
    <groupId>com.jayway.restassured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>1.8.0</version>
    <scope>test</scope>
</dependency>
```

**Controller Pattern**:
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private IUserService userService;

    @GetMapping(produces = {"application/json", "application/xml"})
    public List<User> getAllUsers() {
        return userService.findAll();
    }

    @PostMapping(consumes = "application/json")
    @ResponseStatus(HttpStatus.CREATED)
    public void createUser(@RequestBody User user) {
        userService.create(user);
    }
}
```

**REST Testing Example**:
```java
@Test
public void whenGetUser_thenSuccess() {
    given()
        .auth().basic("admin", "password")
        .when()
        .get("/api/users/1")
        .then()
        .statusCode(200)
        .body("name", equalTo("john"));
}
```

**Security Considerations**:
- Consider stateless authentication (no session)
- CSRF protection may need adjustment for REST
- Use HTTP Basic or token-based auth
- Content negotiation (JSON/XML)

### Section 7: Spring Security ACL
**Key Concepts**: Object-level permissions, ACL tables, permission evaluation

**Dependencies Added**:
```xml
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-acl</artifactId>
</dependency>
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

**ACL Database Schema** (additional tables):
```sql
CREATE TABLE acl_sid (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    principal BOOLEAN NOT NULL,
    sid VARCHAR(100) NOT NULL,
    UNIQUE KEY (sid, principal)
);

CREATE TABLE acl_class (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    class VARCHAR(255) UNIQUE NOT NULL
);

CREATE TABLE acl_object_identity (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    object_id_class BIGINT NOT NULL,
    object_id_identity BIGINT NOT NULL,
    parent_object BIGINT,
    owner_sid BIGINT,
    entries_inheriting BOOLEAN NOT NULL,
    UNIQUE KEY (object_id_class, object_id_identity),
    FOREIGN KEY (object_id_class) REFERENCES acl_class(id),
    FOREIGN KEY (parent_object) REFERENCES acl_object_identity(id),
    FOREIGN KEY (owner_sid) REFERENCES acl_sid(id)
);

CREATE TABLE acl_entry (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    acl_object_identity BIGINT NOT NULL,
    ace_order INT NOT NULL,
    sid BIGINT NOT NULL,
    mask INT NOT NULL,
    granting BOOLEAN NOT NULL,
    audit_success BOOLEAN NOT NULL,
    audit_failure BOOLEAN NOT NULL,
    UNIQUE KEY (acl_object_identity, ace_order),
    FOREIGN KEY (acl_object_identity) REFERENCES acl_object_identity(id),
    FOREIGN KEY (sid) REFERENCES acl_sid(id)
);
```

**ACL Configuration**:
```xml
<bean id="aclService" class="org.springframework.security.acls.jdbc.JdbcMutableAclService">
    <constructor-arg ref="dataSource"/>
    <constructor-arg ref="lookupStrategy"/>
    <constructor-arg ref="aclCache"/>
</bean>

<bean id="lookupStrategy"
      class="org.springframework.security.acls.jdbc.BasicLookupStrategy">
    <constructor-arg ref="dataSource"/>
    <constructor-arg ref="aclCache"/>
    <constructor-arg ref="aclAuthorizationStrategy"/>
    <constructor-arg ref="permissionGrantingStrategy"/>
</bean>

<bean id="aclCache"
      class="org.springframework.security.acls.domain.EhCacheBasedAclCache">
    <constructor-arg>
        <bean class="org.springframework.cache.ehcache.EhCacheFactoryBean">
            <property name="cacheManager" ref="cacheManager"/>
            <property name="cacheName" value="aclCache"/>
        </bean>
    </constructor-arg>
</bean>
```

**ACL Annotations**:
```java
@PostAuthorize("hasPermission(returnObject, 'READ')")
public Possession findById(Long id) {
    return possessionDao.findOne(id);
}

@PreAuthorize("hasPermission(#possession, 'WRITE')")
public void update(Possession possession) {
    possessionDao.save(possession);
}

@PreAuthorize("hasPermission(#possession, 'DELETE')")
public void delete(Possession possession) {
    possessionDao.delete(possession);
}
```

**Permission Types**:
- `READ` (mask: 1)
- `WRITE` (mask: 2)
- `CREATE` (mask: 4)
- `DELETE` (mask: 8)
- `ADMINISTRATION` (mask: 16)

**Granting Permissions Programmatically**:
```java
@Autowired
private MutableAclService aclService;

public void grantPermission(Possession possession, String username, Permission permission) {
    Acl acl;
    ObjectIdentity oid = new ObjectIdentityImpl(Possession.class, possession.getId());
    Sid sid = new PrincipalSid(username);

    try {
        acl = (MutableAcl) aclService.readAclById(oid);
    } catch (NotFoundException e) {
        acl = aclService.createAcl(oid);
    }

    acl.insertAce(acl.getEntries().size(), permission, sid, true);
    aclService.updateAcl(acl);
}
```

---

## File Reference Guide

### Configuration Files

#### web.xml
Location: `src/main/webapp/WEB-INF/web.xml`

Purpose: Servlet container configuration

Key sections:
```xml
<!-- Spring Security Filter -->
<filter>
    <filter-name>springSecurityFilterChain</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>

<!-- Context Configuration -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:webSecurityConfig.xml</param-value>
</context-param>

<!-- Spring Profiles -->
<context-param>
    <param-name>spring.profiles.active</param-name>
    <param-value>sec_client,client</param-value>
</context-param>

<!-- DispatcherServlet -->
<servlet>
    <servlet-name>clientapp</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/clientapp-servlet.xml</param-value>
    </init-param>
</servlet>
```

#### webSecurityConfig.xml
Location: `src/main/resources/webSecurityConfig.xml`

Purpose: Spring Security configuration

Typical structure:
```xml
<beans xmlns:security="http://www.springframework.org/schema/security">

    <!-- HTTP Security Rules -->
    <security:http use-expressions="true">
        <security:intercept-url pattern="..." access="..." />
        <security:form-login ... />
        <security:logout ... />
        <security:remember-me ... />
    </security:http>

    <!-- Authentication Manager -->
    <security:authentication-manager>
        <security:authentication-provider ...>
    </security:authentication-manager>

    <!-- Method Security (Section 5+) -->
    <security:global-method-security ... />
</beans>
```

#### clientapp-servlet.xml
Location: `src/main/webapp/WEB-INF/clientapp-servlet.xml`

Purpose: DispatcherServlet MVC configuration

Typical structure:
```xml
<beans>
    <!-- Component Scanning -->
    <context:component-scan base-package="com.packt.springsecurity.frontend" />

    <!-- View Resolver -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/" />
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- Static Resources -->
    <mvc:resources mapping="/resources/**" location="/resources/" />
</beans>
```

#### persistenceConfig.xml
Location: `src/main/resources/persistenceConfig.xml` (or as Java config)

Purpose: JPA, datasource, transaction configuration

Key sections:
```xml
<!-- DataSource -->
<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource">
    <property name="driverClassName" value="${jdbc.driverClassName}" />
    <property name="jdbcUrl" value="${jdbc.url}" />
    <property name="username" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
</bean>

<!-- JPA Repositories -->
<jpa:repositories base-package="com.packt.springsecurity.backend.persistence.dao" />

<!-- EntityManagerFactory -->
<bean id="entityManagerFactory"
      class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
    <property name="dataSource" ref="dataSource" />
    <property name="packagesToScan" value="com.packt.springsecurity.backend.model" />
    <property name="jpaVendorAdapter">
        <bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter" />
    </property>
</bean>

<!-- Transaction Manager -->
<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
    <property name="entityManagerFactory" ref="entityManagerFactory" />
</bean>

<tx:annotation-driven transaction-manager="transactionManager" />
```

#### pom.xml
Location: Module root `pom.xml`

Purpose: Maven build configuration

Key patterns:
```xml
<properties>
    <spring.version>3.2.2.RELEASE</spring.version>
    <spring-security.version>3.1.4.RELEASE</spring-security.version>
    <hibernate.version>4.2.1.Final</hibernate.version>
</properties>

<dependencies>
    <!-- Always use property references -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>

<build>
    <finalName>spring-security</finalName>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

### Source Files

#### Entity Class (User.java)
Location: `backend/model/User.java`

Standard pattern:
```java
@Entity
@Table(name = "user_account")
@XmlRootElement
public class User implements Serializable {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @Column(unique = true, nullable = false)
    private String name;

    @Column(nullable = false)
    private String password;

    @ManyToMany(fetch = FetchType.EAGER)
    private Set<Authority> authorities = new HashSet<>();

    // Getters, setters, equals, hashCode
}
```

#### Repository Interface
Location: `backend/persistence/dao/IUserJpaDAO.java`

Standard pattern:
```java
public interface IUserJpaDAO extends JpaRepository<User, Long> {
    User findByName(String name);
}
```

#### Service Class
Location: `backend/persistence/service/UserService.java`

Standard pattern:
```java
@Service
@Transactional
public class UserService implements IUserService {
    @Autowired
    private IUserJpaDAO userDao;

    @Override
    public User findByName(String name) {
        return userDao.findByName(name);
    }
}
```

#### Controller Class
Location: `backend/controller/UserController.java` or `frontend/web/controller/WebMediatorController.java`

Backend (REST) pattern:
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private IUserService userService;

    @GetMapping
    public List<User> getAll() {
        return userService.findAll();
    }
}
```

Frontend (MVC) pattern:
```java
@Controller
public class WebMediatorController {
    @RequestMapping(value = "/login.html", method = RequestMethod.GET)
    public ModelAndView login() {
        return new ModelAndView("login");
    }
}
```

---

## Working with the Codebase

### Adding a New Security Rule

1. **Identify the section/module** you're working with
2. **Open** `webSecurityConfig.xml`
3. **Add** new `<intercept-url>` pattern:
```xml
<security:intercept-url pattern="/reports/**"
                       access="hasRole('ROLE_MANAGER')" />
```
4. **Rebuild** and test

### Adding a New Entity

1. **Create entity class** in `backend/model/`:
```java
@Entity
@XmlRootElement
public class Report implements Serializable {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    private String title;

    @ManyToOne
    private User author;
}
```

2. **Create repository** in `backend/persistence/dao/`:
```java
public interface IReportJpaDAO extends JpaRepository<Report, Long> {
    List<Report> findByAuthor(User author);
}
```

3. **Create service interface and implementation**
4. **Restart app** - Hibernate will create tables

### Adding Method Security

1. **Verify** `<global-method-security>` is enabled in `webSecurityConfig.xml`
2. **Annotate** service method:
```java
@PreAuthorize("hasRole('ROLE_ADMIN')")
public void deleteReport(Long id) {
    reportDao.delete(id);
}
```
3. **Test** - AccessDeniedException thrown if unauthorized

### Debugging Security Issues

1. **Enable debug logging** in `logback.xml`:
```xml
<logger name="org.springframework.security" level="DEBUG"/>
```

2. **Check filter chain order** - Review console output on startup

3. **Verify authentication**:
```java
Authentication auth = SecurityContextHolder.getContext().getAuthentication();
System.out.println("Current user: " + auth.getName());
System.out.println("Authorities: " + auth.getAuthorities());
```

4. **Common issues**:
   - `Access Denied` → Check role names (must include `ROLE_` prefix)
   - `Bad credentials` → Verify password encoding matches
   - `Session timeout` → Check remember-me configuration
   - `CSRF token` → Ensure CSRF meta tags in JSP for AJAX

### Module Progression Strategy

When learning or extending the codebase:

1. **Start with Section 1** to understand basic setup
2. **Progress sequentially** through sections - each builds on previous
3. **Don't skip sections** - concepts are cumulative
4. **Compare "_done" variants** to see completed implementations
5. **Experiment in isolated modules** - they're independent

### Best Practices for AI Assistants

1. **Always check the section number** - features vary by section
2. **Don't backport advanced features** - respect the learning progression
3. **Match existing patterns** - consistency is key in educational code
4. **Preserve XML security config** - it's intentional for this course
5. **Keep modules independent** - no cross-module dependencies
6. **Use BCrypt for passwords** - never store plaintext (section 2+)
7. **Follow the backend/frontend separation** - don't mix concerns
8. **Add `@Transactional` to service methods** - ensure proper boundaries
9. **Use `FetchType.EAGER` sparingly** - avoid N+1 queries
10. **Include `@XmlRootElement`** on entities - REST compatibility

---

## Common Tasks Quick Reference

### Add a new user (programmatically)
```java
User user = new User();
user.setName("newuser");
user.setPassword("password");  // Will be encoded by service
Authority authority = new Authority();
authority.setName("ROLE_USER");
user.getAuthorities().add(authority);
userService.create(user);  // Service handles password encoding
```

### Check if user has role
```java
Authentication auth = SecurityContextHolder.getContext().getAuthentication();
boolean isAdmin = auth.getAuthorities().stream()
    .anyMatch(a -> a.getAuthority().equals("ROLE_ADMIN"));
```

### Secure a REST endpoint
```java
@PreAuthorize("hasRole('ROLE_ADMIN')")
@DeleteMapping("/{id}")
public void deleteUser(@PathVariable Long id) {
    userService.delete(id);
}
```

### Get current username
```java
String username = SecurityContextHolder.getContext()
    .getAuthentication()
    .getName();
```

### Add remember-me checkbox to login form
```jsp
<form action="/login" method="post">
    <input type="text" name="username" />
    <input type="password" name="password" />
    <input type="checkbox" name="remember-me" /> Remember Me
    <input type="submit" value="Login" />
</form>
```

### Create ACL for object (Section 7)
```java
ObjectIdentity oid = new ObjectIdentityImpl(Possession.class, possession.getId());
MutableAcl acl = aclService.createAcl(oid);
Sid owner = new PrincipalSid(username);
acl.setOwner(owner);
acl.insertAce(0, BasePermission.READ, owner, true);
acl.insertAce(1, BasePermission.WRITE, owner, true);
aclService.updateAcl(acl);
```

---

## Security Considerations

### Password Storage
- **Always use BCrypt** (section 2+): `BCryptPasswordEncoder`
- **Never log passwords** - even encrypted ones
- **Minimum strength** - enforce in UI/validation

### SQL Injection
- **Use JPA/Spring Data** - automatic parameterization
- **Never concatenate SQL** with user input
- **Avoid native queries** unless necessary

### CSRF Protection
- **Enabled by default** in Spring Security
- **Include CSRF token** in forms:
```jsp
<form method="post" action="/register">
    <sec:csrfInput />
    <!-- form fields -->
</form>
```
- **For AJAX**, include in headers:
```javascript
var token = $("meta[name='_csrf']").attr("content");
var header = $("meta[name='_csrf_header']").attr("content");
$.ajax({
    beforeSend: function(xhr) {
        xhr.setRequestHeader(header, token);
    }
});
```

### Session Management
- **Fixed session protection** enabled by default
- **Concurrent session control** (optional):
```xml
<security:session-management>
    <security:concurrency-control max-sessions="1"
                                 error-if-maximum-exceeded="true" />
</security:session-management>
```

### Role Naming
- **Always prefix with `ROLE_`** - Spring Security convention
- Example: `ROLE_ADMIN`, `ROLE_USER`, `ROLE_MANAGER`
- Don't use in expressions: `hasRole('ADMIN')` NOT `hasRole('ROLE_ADMIN')`

---

## Troubleshooting Guide

### Build Issues

**Problem**: `mvn clean install` fails with missing dependencies

**Solution**: Check Maven repository connectivity, update `~/.m2/settings.xml`

**Problem**: `ClassNotFoundException` on startup

**Solution**: Verify all Spring dependencies use same version (check `<properties>`)

### Runtime Issues

**Problem**: 404 on all URLs after deployment

**Solution**: Check `web.xml` servlet mapping and context path

**Problem**: `Access Denied` for all users

**Solution**:
- Verify role names include `ROLE_` prefix in database
- Check `CustomUserService` returns `GrantedAuthority` correctly
- Enable debug logging to see authentication details

**Problem**: Login form redirect loop

**Solution**:
- Ensure `/login.html` is accessible anonymously
- Check `form-login` configuration in `webSecurityConfig.xml`
- Verify no conflicting `intercept-url` patterns

**Problem**: Remember-me not working

**Solution**:
- Check `remember-me` checkbox name is `remember-me`
- Verify `key` attribute matches across server restarts (for token validation)
- Check `token-validity-seconds` hasn't expired

### Database Issues

**Problem**: `Table doesn't exist` error

**Solution**:
- Set `hibernate.hbm2ddl.auto` to `create` or `update`
- Verify database exists and user has CREATE permissions
- Check `packagesToScan` includes entity packages

**Problem**: `Duplicate entry` on user creation

**Solution**: Username already exists (unique constraint)

**Problem**: Lazy initialization exception

**Solution**:
- Add `@Transactional` to service method
- Or change `FetchType` to `EAGER` (use sparingly)
- Or use `Hibernate.initialize()` within transaction

---

## Version Compatibility Notes

This codebase uses **legacy versions** intentionally for educational stability:

- **Spring 3.2.x** - Released 2012-2013
- **Spring Security 3.1.x** - Released 2011-2012
- **Java 6/7** - Modern JDK 8+ will work but code targets older APIs

### Modern Equivalents (for reference)

| Legacy Feature | Modern Equivalent (Spring Boot 3.x / Spring Security 6.x) |
|----------------|----------------------------------------------------------|
| `<security:http>` XML | `SecurityFilterChain` bean with `HttpSecurity` fluent API |
| `<security:authentication-manager>` | `AuthenticationManager` bean configuration |
| `ROLE_` prefix required | Optional (configurable) |
| `@Secured` annotation | `@PreAuthorize` preferred |
| BCrypt password encoder | Same, but with builder pattern |
| XML security namespace | Java configuration with lambda DSL |
| `hasRole('ADMIN')` | Same in Java config |

### If Upgrading (NOT recommended for learning)

1. **Spring 4.x** migration is straightforward
2. **Spring 5.x** requires Java 8+
3. **Spring Boot 2.x** simplifies configuration drastically
4. **Spring Security 5.x** deprecates XML config
5. **Spring Boot 3.x** requires Java 17+ and Jakarta EE namespace

**For this course**: Stick with the legacy versions to match tutorial content.

---

## Additional Resources

### Spring Security 3.1 Documentation
- [Reference Manual](http://static.springsource.org/spring-security/site/docs/3.1.x/reference/springsecurity.html)
- [Single Page Reference](http://static.springsource.org/spring-security/site/docs/3.1.x/reference/springsecurity-single.html)

### Spring Framework 3.2 Documentation
- [Spring Framework Reference](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/)

### Course Structure
- See `README.md` for section links and overview
- Each section has progressive video modules
- Reference implementations in `_done` modules

---

## Summary for AI Assistants

When working with this repository:

1. **This is educational code** - maintain learning progression
2. **Each module is independent** - no shared code between modules
3. **Sections build sequentially** - respect topic dependencies
4. **XML + Java config mix** is intentional - don't "modernize"
5. **Backend/frontend separation** is strict - don't cross boundaries
6. **Password encoding** is mandatory from section 2+
7. **Spring Data JPA** eliminates custom DAO code
8. **SpEL expressions** start in section 3
9. **Method security** starts in section 5
10. **ACL** is section 7 only

### Quick Decision Tree

**Question**: Should I use Java or XML config?
- **Security rules**: XML (`webSecurityConfig.xml`)
- **MVC/Web**: Java (`@Configuration` classes)
- **Persistence**: Either (varies by module)

**Question**: Where does this code go?
- **REST endpoint**: `backend/controller/`
- **UI form handler**: `frontend/web/controller/`
- **Business logic**: `backend/persistence/service/`
- **Data access**: `backend/persistence/dao/` (interface only)
- **Entity**: `backend/model/`

**Question**: How do I secure this?
- **URL pattern**: Add to `<intercept-url>` in XML
- **Method**: Add `@PreAuthorize` / `@Secured` annotation
- **Object-level**: Use ACL (section 7 only)

**Question**: What Spring Security version features can I use?
- **Check the section number**
- **Section 1**: Basic filter chain
- **Section 2**: Database auth
- **Section 3**: Remember-me + SpEL
- **Section 4**: LDAP
- **Section 5**: Method security
- **Section 6**: REST
- **Section 7**: ACL

---

**Document Version**: 1.0
**Last Updated**: 2025-11-13
**Maintained By**: AI-assisted documentation
**Repository**: udemy-spring-security (Spring Security 3.1 Course)
