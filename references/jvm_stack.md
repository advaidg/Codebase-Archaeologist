# Java / JSF / Spring — Complete Reference (Java 6 → Java 25)

---

## PART 1 — JAVA ERA DETECTION

### Detect Java version from build files
```bash
# Maven
grep -E "<java.version>|<source>|<target>|<release>" pom.xml | head -5
grep -E "maven.compiler" pom.xml | head -5

# Gradle
grep -E "sourceCompatibility|targetCompatibility|javaVersion|toolchain" \
  build.gradle build.gradle.kts 2>/dev/null | head -5

# SDK config
cat .java-version .sdkmanrc 2>/dev/null
```

### Java Era → What to Expect

| Java | Era Label | Spring | JSF | Typical App Server |
|------|-----------|--------|-----|--------------------|
| 6 | **Ancient** | 2.x–3.x, XML only | JSF 1.x / 2.0 | JBoss 4/5, Tomcat 6 |
| 7 | **Legacy** | Spring 3.x | JSF 2.1 / RichFaces | JBoss 6/7, Tomcat 7 |
| 8 | **Mature** | Spring 4.x / Boot 1.x | JSF 2.2 / PrimeFaces 5 | WildFly 8–10, Tomcat 8 |
| 11 | **Transitional** | Spring Boot 2.x | JSF 2.3 / PrimeFaces 7 | WildFly 17+, Tomcat 9 |
| 17 | **Modern** | Spring Boot 3.x / Spring 6 | Faces 3.0 (jakarta.*) | WildFly 27+, Tomcat 10 |
| 21 | **Current LTS** | Spring Boot 3.2+ | Faces 4.0 | WildFly 30+, Tomcat 10.1 |
| 25 | **Preview** | Spring Boot 3.4+ | Faces 4.1 | Latest |

### Confirm with feature usage
```bash
# Records → Java 16+
grep -rn "^public record\|^record " --include="*.java" . | grep -v test | head -3

# var keyword → Java 10+
grep -rn "^\s*var " --include="*.java" . | grep -v test | head -3

# Text blocks → Java 15+
grep -rn '"""' --include="*.java" . | grep -v test | head -3

# Virtual threads → Java 21+
grep -rn "Thread.ofVirtual\|newVirtualThreadPerTaskExecutor" \
  --include="*.java" . | grep -v test | head -3

# Sealed classes → Java 17+
grep -rn "^sealed\|permits " --include="*.java" . | grep -v test | head -3
```

---

## PART 2 — JSF GENERATION DETECTION

### JSF Version
```bash
# In pom.xml
grep -i "jsf\|faces\|primefaces\|richfaces\|icefaces\|omnifaces\|mojarra\|myfaces" \
  pom.xml build.gradle 2>/dev/null

# In web.xml — servlet mapping reveals version
cat src/main/webapp/WEB-INF/web.xml 2>/dev/null | grep -A2 "FacesServlet\|faces"

# namespace in .xhtml = version indicator
head -5 $(find . -name "*.xhtml" -not -path '*/target/*' | head -1) 2>/dev/null
```

### JSF Version → Key Signals

| JSF Version | Artifact | Namespace | Key Feature |
|---|---|---|---|
| JSF 1.x | `jsf-api`, `jsf-impl` | `http://java.sun.com/jsf` | faces-config.xml only |
| JSF 2.0 | `javax.faces` 2.0 | `http://java.sun.com/jsf/html` | Facelets (XHTML), `@ManagedBean` |
| JSF 2.1 | `javax.faces` 2.1 | same | CDI `@Named` support |
| JSF 2.2 | `javax.faces` 2.2 | same | Stateless views, file upload |
| JSF 2.3 | `javax.faces` 2.3 | same | `@Inject` in beans, CDI alignment |
| Faces 3.0 | `jakarta.faces` 3.0 | `jakarta.faces.*` | jakarta.* namespace (EE 9) |
| Faces 4.0 | `jakarta.faces` 4.0 | `jakarta.faces.*` | Streamlined (EE 10) |

### Component Library Detection
```bash
grep -i "primefaces\|richfaces\|a4j\|icefaces\|omnifaces\|bootsfaces\|butterfaces" \
  pom.xml build.gradle 2>/dev/null
```

| Library | pom.xml artifact | XHTML prefix | Era |
|---|---|---|---|
| PrimeFaces | `org.primefaces:primefaces` | `p:` | 2012–present |
| RichFaces 4 | `org.richfaces` | `rich:` `a4j:` | 2010–2016 (EOL) |
| ICEfaces | `org.icefaces` | `ice:` | 2008–present |
| OmniFaces | `org.omnifaces` | `o:` | Utility library |
| BootsFaces | `net.bootsfaces` | `b:` | Bootstrap + JSF |

### JSF Bean Scope Detection
```bash
grep -rn "@ManagedBean\|@Named\|@ViewScoped\|@SessionScoped\|@RequestScoped\|@ApplicationScoped\|@FlowScoped\|@ConversationScoped" \
  --include="*.java" . | grep -v test | grep -v target | head -30
```

| Annotation | Scope | Package | Notes |
|---|---|---|---|
| `@ManagedBean` | Any | `javax.faces.bean` | JSF 2.0–2.3, old style |
| `@Named` | CDI | `javax.inject` / `jakarta.inject` | Modern, CDI-managed |
| `@RequestScoped` | Request | jsf.bean or cdi | Shortest lifetime |
| `@ViewScoped` | View | jsf.bean or cdi | Lives for one page |
| `@SessionScoped` | Session | jsf.bean or cdi | Per HTTP session |
| `@ApplicationScoped` | App | jsf.bean or cdi | Singleton |
| `@ConversationScoped` | Conversation | CDI only | Multi-step flows |
| `@FlowScoped` | Flow | JSF 2.2+ | Wizard flows |

### Navigation: Config vs Implicit
```bash
# Old-style navigation rules
grep -A10 "navigation-rule" src/main/webapp/WEB-INF/faces-config.xml 2>/dev/null | head -40

# Implicit navigation (return string from action method)
grep -rn "return \"" --include="*.java" . | grep -i "xhtml\|faces\|redirect\|outcome" \
  | grep -v test | head -20
```

---

## PART 3 — SPRING GENERATION DETECTION

### Spring Boot vs Plain Spring
```bash
grep "spring-boot-starter-parent\|spring-boot-dependencies" pom.xml
grep "spring-webmvc\|spring-context" pom.xml | grep -v "spring-boot"
```

### Spring Boot Version → Era

| Boot Version | Spring Framework | Java Min | javax/jakarta | Tomcat | Notable |
|---|---|---|---|---|---|
| 1.x | Spring 4.x | 6 | javax.* | 8.x | First Boot, auto-config |
| 2.0–2.6 | Spring 5.x | 8 | javax.* | 9.x | Actuator, WebFlux added |
| 2.7 | Spring 5.3 | 8 | javax.* | 9.x | Last javax.* Boot |
| 3.0–3.1 | Spring 6.0 | 17 | jakarta.* | 10.x | Jakarta migration |
| 3.2+ | Spring 6.1 | 17 | jakarta.* | 10.x | Virtual threads, RestClient |
| 3.4+ | Spring 6.2 | 17 | jakarta.* | 10.x | Latest |

### Plain Spring (no Boot) Detection
```bash
# Version from pom.xml
grep "spring-core\|spring-context\|spring-webmvc" pom.xml | grep "<version>"

# Config style
find . \( -name "applicationContext*.xml" -o -name "spring*.xml" \
  -o -name "dispatcher-servlet.xml" \) -not -path '*/target/*'
find . -name "*.java" | xargs grep -l "@Configuration\|@EnableWebMvc" \
  2>/dev/null | grep -v test | head -10
```

### Spring MVC web.xml Patterns (WAR deployment)
```bash
cat src/main/webapp/WEB-INF/web.xml 2>/dev/null
```
Key entries to extract:
- `<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>` → Spring MVC
- `<servlet-class>javax.faces.webapp.FacesServlet</servlet-class>` → JSF
- Both present → Spring + JSF integration (via SpringBeanFacesELResolver or CDI bridge)
- `<context-param>contextConfigLocation</context-param>` → XML config location
- `<listener>ContextLoaderListener</listener>` → Spring root context bootstrap

### Spring + JSF Integration Patterns
```bash
# SpringBeanFacesELResolver (Spring 3/4 + JSF integration)
grep -rn "SpringBeanFacesELResolver\|SpringBeanFacesContextUtils\|FacesContextUtils" \
  --include="*.java" --include="*.xml" . | grep -v target | head -10

# CDI / Weld bridge
grep -rn "weld\|jboss-weld\|deltaspike\|joinfaces" pom.xml build.gradle 2>/dev/null

# JoinFaces (Spring Boot + JSF integration)
grep -rn "joinfaces\|org.joinfaces" pom.xml build.gradle 2>/dev/null
```

| Integration Approach | Signal | Era |
|---|---|---|
| `SpringBeanFacesELResolver` in faces-config.xml | Classic Spring+JSF | Spring 3/4 |
| CDI + `@Named` (no Spring beans in JSF) | Pure CDI | JEE 7+ |
| JoinFaces (`org.joinfaces`) | Spring Boot + JSF | Boot 2/3 |
| `@Inject` in ManagedBeans calling Spring services | Hybrid | Mixed |

---

## PART 4 — PERSISTENCE LAYER

### ORM Detection
```bash
grep "hibernate\|spring-data-jpa\|eclipselink\|mybatis\|jooq\|spring-jdbc\|openjpa" \
  pom.xml build.gradle 2>/dev/null

grep -rn "@Entity\|@Table\|@Id\|@Column\|@MappedSuperclass" \
  --include="*.java" . | grep -v test | grep -v target | head -30
```

### Spring Data Repositories
```bash
grep -rn "extends JpaRepository\|extends CrudRepository\|extends PagingAndSortingRepository" \
  --include="*.java" . | grep -v test | grep -v target | head -20
```

### DAO Pattern (pre-Spring Data, common in Java 6/7 era)
```bash
find . -name "*Dao*.java" -o -name "*DAO*.java" -o -name "*DaoImpl*.java" \
  | grep -v test | grep -v target | head -20

grep -rn "EntityManager\|SessionFactory\|Session\|HibernateTemplate\|JdbcTemplate" \
  --include="*.java" . | grep -v test | grep -v target | head -20
```

### Transaction Management
```bash
grep -rn "@Transactional" --include="*.java" . | grep -v test | grep -v target | head -20
grep -rn "<tx:annotation-driven\|<aop:config\|TransactionProxyFactoryBean" \
  $(find . -name "*.xml" -not -path '*/target/*') 2>/dev/null | head -10
```

---

## PART 5 — SPRING SECURITY DETECTION

```bash
grep "spring-security\|spring-boot-starter-security" pom.xml build.gradle 2>/dev/null

grep -rn "WebSecurityConfigurerAdapter\|SecurityFilterChain\|@EnableWebSecurity\|HttpSecurity" \
  --include="*.java" . | grep -v test | grep -v target | head -15
```

| Pattern | Version | Style | Status |
|---|---|---|---|
| XML `<security:http>` | Spring Security 3.x | XML DSL | Legacy |
| `extends WebSecurityConfigurerAdapter` | 3.x–5.6 | Class override | ⚠️ Deprecated |
| `@Bean SecurityFilterChain` | 5.7+ | Lambda DSL | ✅ Current |
| `antMatchers()` | Pre-6.x | URL matching | ⚠️ Deprecated |
| `requestMatchers()` | 6.x+ | URL matching | ✅ Current |
| `formLogin()` | Any | Form auth | Common with JSF |
| `oauth2Login()` | 5.x+ | OAuth2/OIDC | Modern |
| `jwt()` / `JwtDecoder` | 5.x+ | Stateless JWT | REST APIs |
| `sessionManagement()` | Any | Session control | Important for JSF |

### JSF + Spring Security Note
JSF uses POST for form submissions. Ensure CSRF config is compatible:
```bash
grep -rn "csrf\|CsrfFilter\|CsrfTokenRepository" --include="*.java" \
  --include="*.xml" . | grep -v test | grep -v target | head -10
```

---

## PART 6 — LEGACY FLAGS (always check and report)

Run these and list findings in the PRD Section 8:

```bash
# 1. Deprecated Security API
grep -rn "WebSecurityConfigurerAdapter" --include="*.java" . | grep -v target

# 2. Old javax.* namespace (needs migration to jakarta.* for Spring Boot 3 / Faces 3+)
grep -rn "^import javax\." --include="*.java" . | grep -v target | head -5
# Flag if: javax.faces.*, javax.servlet.*, javax.persistence.*, javax.inject.*

# 3. Field injection (testability / design issue)
grep -rn "@Autowired" --include="*.java" . | grep -v test | grep -v target \
  | grep -v "//" | wc -l

# 4. Old @ManagedBean (vs modern @Named CDI)
grep -rn "@ManagedBean" --include="*.java" . | grep -v target | wc -l

# 5. RichFaces (EOL since 2016)
grep -i "richfaces\|a4j" pom.xml build.gradle 2>/dev/null

# 6. RestTemplate (deprecated in Spring 6 for new code)
grep -rn "new RestTemplate\|RestTemplate " --include="*.java" . \
  | grep -v test | grep -v target | head -5

# 7. No structured logging
grep -rn "System\.out\.print\|e\.printStackTrace" --include="*.java" . \
  | grep -v test | grep -v target | head -5

# 8. God classes
find . -name "*.java" -not -path '*/test*' -not -path '*/target/*' \
  | xargs wc -l 2>/dev/null | sort -rn | head -10

# 9. Hardcoded config (should be externalized)
grep -rn "localhost\|127\.0\.0\.1\|jdbc:mysql\|jdbc:oracle\|jdbc:postgresql" \
  --include="*.java" . | grep -v test | grep -v target | head -10

# 10. Missing tests
find . -type d \( -name "test" -o -name "tests" \) -not -path '*/target/*' \
  | head -3
```
