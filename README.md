# 🏛️ Codebase Archaeologist
### AI Skill for Java · JSF · Spring · Spring Boot

> Drop this skill into Claude Code or use it with GitHub Copilot to instantly understand any Java application — legacy or modern — and generate a full Product Requirements Document from the codebase alone.

---

## What It Does

You hand it a Java repository. It gives you back:

- **Tech Stack Card** — exact Java version, JSF generation, Spring era, database, security, deployment model
- **Architecture Map** — layering pattern, entry point chain, Mermaid diagrams
- **Screen Inventory** — every JSF `.xhtml` mapped to its backing bean and business function
- **API Surface** — REST endpoint table for Spring MVC apps
- **Data Model** — JPA entities, relationships, migration history
- **Business Rules Register** — validation, workflow states, roles, scheduled jobs extracted as plain-English rules
- **Product Requirements Document (PRD)** — full reverse-engineered PRD ready for stakeholders
- **Developer Quick-Start** — run commands, key files, package map

Works on **Java 6 through Java 25**, all JSF generations (1.x → Faces 4.0), and all Spring generations (XML beans → Spring Boot 3.x).

---

## File Structure

```
codebase-archaeologist/
├── SKILL.md                      ← Main playbook (8-step analysis workflow)
└── references/
    └── jvm-stack.md              ← Deep Java + JSF + Spring reference (all eras)
```

### SKILL.md — The 8-Step Workflow

| Step | Name | Output |
|------|------|--------|
| 0 | **Triage** | First Impression — repo shape, language fingerprint, health signal |
| 1 | **Stack Identification** | Tech Stack Card |
| 2 | **Architecture Map** | Layering pattern, entry point chain, Mermaid diagram |
| 3 | **Screen Inventory** | JSF screen → backing bean → function table |
| 4 | **API Surface** | REST endpoint table |
| 5 | **Data Model** | Entity map + migration history summary |
| 6 | **Business Rules Extraction** | BR-001…BR-NNN plain-English rules register |
| 7 | **PRD** | Full Product Requirements Document |
| 8 | **Developer Quick-Start** | Commands, key files, package structure |

### references/jvm-stack.md — What's Covered

| Section | Coverage |
|---|---|
| Java Era Table | Java 6, 7, 8, 11, 17, 21, 25 with expected Spring + JSF versions |
| JSF Generation Detection | JSF 1.x → Faces 4.0, namespace changes, artifact IDs |
| Component Libraries | PrimeFaces, RichFaces, ICEfaces, OmniFaces, BootsFaces |
| Bean Scope Reference | @ManagedBean vs @Named CDI, all scopes with era and package |
| Spring Boot Era Table | Boot 1.x → 3.4 with javax/jakarta split, Java minimums |
| Plain Spring Detection | XML → Annotation → Java Config progression |
| Spring + JSF Integration | SpringBeanFacesELResolver, JoinFaces, CDI bridge patterns |
| Persistence Layer | Spring Data JPA, DAO pattern, Hibernate, EntityManager |
| Spring Security Generations | WebSecurityConfigurerAdapter → SecurityFilterChain |
| Legacy Flag Checklist | 10 runnable bash checks, all with business risk framing |

---

## Supported Stacks

| Technology | Versions Covered |
|---|---|
| **Java** | 6, 7, 8, 11, 17, 21, 25 |
| **JSF / Jakarta Faces** | 1.x, 2.0, 2.1, 2.2, 2.3, 3.0, 4.0, 4.1 |
| **Spring Framework** | 2.x, 3.x, 4.x, 5.x, 6.x |
| **Spring Boot** | 1.x, 2.x, 3.x |
| **Component Libraries** | PrimeFaces, RichFaces (EOL), ICEfaces, OmniFaces |
| **ORM / Persistence** | Spring Data JPA, Hibernate, MyBatis, jOOQ, plain JDBC |
| **Build Tools** | Maven (single + multi-module), Gradle |
| **App Servers** | Tomcat, JBoss, WildFly, WebLogic, embedded |
| **Security** | Spring Security 3.x → 6.x, JAAS, custom |

---

## Sample Outputs

### Tech Stack Card
```
╔══════════════════════════════════════════════════════╗
║              TECH STACK CARD                         ║
╠══════════════════════════════════════════════════════╣
║ Java Version   : Java 8                              ║
║ UI Framework   : JSF 2.2 + PrimeFaces 6.x            ║
║ Backend        : Spring Boot 1.5.x                   ║
║ Config Style   : Spring Boot Autoconfiguration       ║
║ Build Tool     : Maven 3.6                           ║
║ Persistence    : Spring Data JPA + Hibernate 5.x     ║
║ Database       : PostgreSQL via HikariCP             ║
║ Security       : Spring Security 4.x (form login)    ║
║ App Server     : Embedded Tomcat 8.5                 ║
║
