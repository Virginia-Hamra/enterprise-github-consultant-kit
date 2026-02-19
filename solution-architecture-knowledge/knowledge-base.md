# Solution Architecture Patterns — Practical Knowledge Base

> **Purpose:** A deep-dive reference guide for software and platform engineers covering architectural patterns, distributed systems, cloud patterns, API design, UI/UX architecture, and data modeling. Built for practitioners who value clarity over cleverness.

---

# Table of Contents

1. [SOLID Principles](#1-solid-principles)
2. [Gang of Four Design Patterns](#2-gang-of-four-design-patterns)
3. [Architectural Styles](#3-architectural-styles)
4. [Distributed Systems Fundamentals](#4-distributed-systems-fundamentals)
5. [Cloud Architecture Patterns](#5-cloud-architecture-patterns)
6. [REST API Design](#6-rest-api-design)
7. [UI/UX Architecture Patterns](#7-uiux-architecture-patterns)
8. [Data Modeling and Domain Patterns](#8-data-modeling-and-domain-patterns)
9. [Cross-Cutting Concerns](#9-cross-cutting-concerns)
10. [Patterns Reference Cheat Sheet](#10-patterns-reference-cheat-sheet)

---

# 1. SOLID Principles

SOLID is an acronym for five object-oriented design principles introduced by Robert C. Martin ("Uncle Bob"). They are as applicable to architectural thinking as they are to class-level code — and arguably more powerful when used to reason about module boundaries, service contracts, and system evolution.

---

## 1.1 Single Responsibility Principle (SRP)

**"A class (or module, service) should have one and only one reason to change."**

The key phrase is *reason to change* — not *one thing it does*. A reason to change corresponds to a stakeholder or actor: different people want different things from your software, and their concerns shouldn't collide in the same artifact.

### Code-Level Example

```typescript
// VIOLATION: This class has two reasons to change — business logic AND persistence
class UserService {
  createUser(name: string, email: string) {
    const user = { id: uuid(), name, email, createdAt: new Date() };
    // Business logic AND SQL in the same place
    db.query(`INSERT INTO users VALUES (?, ?, ?)`, [user.id, user.name, user.email]);
    return user;
  }
}

// FIX: Separate concerns
class UserRepository {
  save(user: User): void {
    db.query(`INSERT INTO users VALUES (?, ?, ?)`, [user.id, user.name, user.email]);
  }
}

class UserService {
  constructor(private repo: UserRepository) {}
  createUser(name: string, email: string): User {
    const user = new User(uuid(), name, email);
    this.repo.save(user);
    return user;
  }
}
```

### Architectural Application

At the service level, SRP translates to: each microservice owns one **bounded context** (see §8 DDD). When a service handles billing, authentication, AND notification, any change to any one of those domains forces a redeploy of all three — and introduces hidden coupling between teams.

In platform/DevOps contexts: a GitHub Actions workflow that handles linting, testing, building, AND deploying violates SRP. A change to the deployment strategy forces modification of the same file that holds linting config. Split into composable reusable workflows.

### Common Misconceptions

- SRP does **not** mean a class/function does only one small thing. A class can do many things if they all change for the same reason.
- "One reason to change" is about *actors*, not lines of code.

---

## 1.2 Open/Closed Principle (OCP)

**"Software entities should be open for extension, but closed for modification."**

Once a module is published and working, you should be able to add new behavior without modifying its internals. This is achieved through abstraction — polymorphism, strategies, plugins.

### Code-Level Example

```typescript
// VIOLATION: Adding a new payment type requires modifying this class
class PaymentProcessor {
  process(payment: Payment) {
    if (payment.type === 'credit') { /* ... */ }
    else if (payment.type === 'paypal') { /* ... */ }
    // Every new type breaks this class open
  }
}

// FIX: Define a contract, extend by adding new implementations
interface PaymentStrategy {
  process(payment: Payment): void;
}

class CreditCardStrategy implements PaymentStrategy {
  process(payment: Payment) { /* credit card logic */ }
}

class PayPalStrategy implements PaymentStrategy {
  process(payment: Payment) { /* paypal logic */ }
}

class PaymentProcessor {
  constructor(private strategy: PaymentStrategy) {}
  process(payment: Payment) { this.strategy.process(payment); }
}
```

### Architectural Application

OCP at scale = **plugin architectures**, **event-driven extensibility**, and **contract-first API design**. When a REST API is versioned properly (see §6), consumers don't need to change when the backend evolves — the interface is "closed" to breaking changes but "open" for internal enhancements.

In GitHub/platform context: a Terraform module that accepts variable inputs and is callable by many teams is OCP-compliant. You extend its behavior through variables, not by forking it.

---

## 1.3 Liskov Substitution Principle (LSP)

**"Objects of a subtype must be substitutable for objects of their base type without altering the correctness of the program."**

If class `B` extends class `A`, you should be able to swap `A` with `B` anywhere without breaking things. LSP violations often show up as `instanceof` checks or subclasses that throw `NotImplementedException`.

### Code-Level Example

```typescript
// VIOLATION: Square extends Rectangle but breaks the contract
class Rectangle {
  setWidth(w: number) { this.width = w; }
  setHeight(h: number) { this.height = h; }
  area() { return this.width * this.height; }
}

class Square extends Rectangle {
  setWidth(w: number) { this.width = w; this.height = w; } // Surprises caller!
  setHeight(h: number) { this.width = h; this.height = h; }
}

// FIX: Don't force inheritance where the IS-A relationship doesn't hold
interface Shape { area(): number; }
class Rectangle implements Shape { area() { return this.w * this.h; } }
class Square implements Shape { area() { return this.side * this.side; } }
```

### Architectural Application

At the API/service level, LSP means **behavioral contracts** matter as much as interface signatures. A v2 API endpoint that accepts the same parameters as v1 but silently changes behavior (e.g., pagination default changes from 10 to 100) violates LSP for API consumers.

---

## 1.4 Interface Segregation Principle (ISP)

**"Clients should not be forced to depend on interfaces they do not use."**

Fat interfaces force implementers to stub out methods they don't need, creating friction and fragility. Small, focused interfaces are more composable.

### Code-Level Example

```typescript
// VIOLATION: A "ReadOnly" repository is forced to implement write methods
interface Repository<T> {
  findById(id: string): T;
  save(entity: T): void;
  delete(id: string): void;
}

// FIX: Segregate into role-specific interfaces
interface ReadRepository<T> { findById(id: string): T; findAll(): T[]; }
interface WriteRepository<T> { save(entity: T): void; delete(id: string): void; }

// A reporting service only needs read
class ReportingService {
  constructor(private repo: ReadRepository<Order>) {}
}
```

### Architectural Application

ISP maps directly to **API design**: don't expose a single massive API surface when clients only need a subset. BFF pattern (see §3.9) is ISP applied at the API gateway level — each frontend gets a tailored interface, not a monolithic one.

In Terraform/IaC: a module with 40 required input variables forces all callers to provide everything even if 30 of those don't apply to their context. ISP says break it into composable sub-modules.

---

## 1.5 Dependency Inversion Principle (DIP)

**"High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details."**

This is the principle that enables testability, replaceability, and the clean architecture model (see §3.4). Depend on contracts, not concrete implementations.

### Code-Level Example

```typescript
// VIOLATION: High-level service depends on a specific low-level implementation
class OrderService {
  private db = new PostgresDatabase(); // Hardcoded dependency
  placeOrder(order: Order) {
    this.db.insert('orders', order);
  }
}

// FIX: Depend on an abstraction, inject the implementation
interface Database {
  insert(table: string, record: unknown): void;
}

class OrderService {
  constructor(private db: Database) {} // Injected from outside
  placeOrder(order: Order) {
    this.db.insert('orders', order);
  }
}

// Now you can inject PostgresDatabase, InMemoryDatabase (for tests), or DynamoDB
```

### The Relationship Between DIP and Clean Architecture

DIP is the engine behind **Clean Architecture** (§3.4). The dependency rule — *source code dependencies must point inward toward high-level policy* — is DIP applied at every layer boundary. Business logic never imports infrastructure code; infrastructure implements interfaces defined by the domain.

### When SOLID Becomes a Constraint

SOLID principles are guidelines, not laws. Applied dogmatically, they can lead to:

- **Over-abstraction**: Introducing interfaces for everything, including things that will never have a second implementation, makes code harder to navigate.
- **Premature design**: In early-stage systems, you don't yet know the "right" axes of change. Build first, refactor toward SOLID once you understand the domain.
- **Analysis paralysis**: Teams can spend more time debating which principle applies than solving the actual problem.

> **Practitioner Note:** Apply SOLID at the *pain points* — where change is frequent, where coupling causes bugs, where testing is hard. Don't apply it preemptively everywhere. The best signal that you need SRP is when a PR touches 8 files because one business rule changed.

---

# 2. Gang of Four Design Patterns

The **Gang of Four (GoF)** patterns come from the 1994 book *Design Patterns: Elements of Reusable Object-Oriented Software* by Gamma, Helm, Johnson, and Vlissides. The 23 patterns are grouped into three families: **Creational**, **Structural**, and **Behavioral**.

These patterns are solutions to recurring design problems. They are a vocabulary, not a checklist — knowing them lets you communicate design intent clearly.

---

## 2.1 Creational Patterns

Creational patterns abstract the instantiation process. They help make a system independent of how its objects are created, composed, and represented.

### Singleton

**Intent:** Ensure a class has only one instance and provide a global access point to it.

```
┌───────────────────────────┐
│        Singleton          │
├───────────────────────────┤
│ - instance: Singleton     │
├───────────────────────────┤
│ + getInstance(): Singleton│
│ - Singleton()             │
└───────────────────────────┘
```

```typescript
class ConfigManager {
  private static instance: ConfigManager;
  private config: Record<string, string> = {};

  private constructor() {
    this.config = loadFromEnv();
  }

  static getInstance(): ConfigManager {
    if (!ConfigManager.instance) {
      ConfigManager.instance = new ConfigManager();
    }
    return ConfigManager.instance;
  }

  get(key: string): string { return this.config[key]; }
}
```

**When to use:** Shared resources like config, logging, connection pools.
**When to avoid:** Singletons are global state — they make testing harder (can't swap them easily) and hide dependencies. Prefer dependency injection. Modern frameworks (NestJS, Spring) handle this via DI containers.
**Modern relevance:** Often replaced by DI containers or module-level singletons in JS/TS (`export const config = new ConfigManager()`).

---

### Factory Method

**Intent:** Define an interface for creating objects, but let subclasses decide which class to instantiate.

```
Creator ──────────> Product (interface)
   │                    △
   │ createProduct()    │
   ▼                    │
ConcreteCreator ──> ConcreteProduct
```

```typescript
interface Logger { log(msg: string): void; }
class ConsoleLogger implements Logger { log(msg: string) { console.log(msg); } }
class FileLogger implements Logger { log(msg: string) { fs.appendFileSync('app.log', msg); } }

abstract class App {
  abstract createLogger(): Logger;
  run() {
    const logger = this.createLogger();
    logger.log('App started');
  }
}

class DevelopmentApp extends App {
  createLogger(): Logger { return new ConsoleLogger(); }
}
class ProductionApp extends App {
  createLogger(): Logger { return new FileLogger(); }
}
```

**When to use:** When the exact type of object to create depends on context or configuration.
**Real-world:** AWS SDK creates service clients through factory methods. CI/CD runners use factory patterns to spin up environment-specific build contexts.

---

### Abstract Factory

**Intent:** Provide an interface for creating families of related objects without specifying concrete classes.

```typescript
// Family 1: AWS infrastructure
// Family 2: Azure infrastructure
interface CloudFactory {
  createStorage(): Storage;
  createQueue(): Queue;
  createDatabase(): Database;
}

class AWSFactory implements CloudFactory {
  createStorage(): Storage { return new S3Bucket(); }
  createQueue(): Queue { return new SQSQueue(); }
  createDatabase(): Database { return new RDSDatabase(); }
}

class AzureFactory implements CloudFactory {
  createStorage(): Storage { return new BlobStorage(); }
  createQueue(): Queue { return new ServiceBusQueue(); }
  createDatabase(): Database { return new AzureSQLDatabase(); }
}
```

**When to use:** When a system needs to be independent of how its products are created, and you want to enforce that products from the same "family" are used together.

---

### Builder

**Intent:** Separate the construction of a complex object from its representation, so the same construction process can create different representations.

```typescript
class QueryBuilder {
  private table = '';
  private conditions: string[] = [];
  private selectedFields: string[] = ['*'];
  private limitValue?: number;

  from(table: string): this { this.table = table; return this; }
  select(...fields: string[]): this { this.selectedFields = fields; return this; }
  where(condition: string): this { this.conditions.push(condition); return this; }
  limit(n: number): this { this.limitValue = n; return this; }

  build(): string {
    let query = `SELECT ${this.selectedFields.join(', ')} FROM ${this.table}`;
    if (this.conditions.length) query += ` WHERE ${this.conditions.join(' AND ')}`;
    if (this.limitValue) query += ` LIMIT ${this.limitValue}`;
    return query;
  }
}

// Usage
const query = new QueryBuilder()
  .from('users')
  .select('id', 'name', 'email')
  .where('active = true')
  .limit(50)
  .build();
```

**When to use:** Complex object creation with many optional parameters. Much cleaner than telescoping constructors.
**Real-world:** GitHub Actions workflow builders, Terraform resource builders, ORMs (TypeORM QueryBuilder), test data factories.

---

### Prototype

**Intent:** Create new objects by copying (cloning) an existing object.

**When to use:** When object creation is expensive and you need many similar instances. Common in game development, document templates, deep config cloning.
**Modern relevance:** JavaScript's prototypal inheritance is built on this concept. `Object.assign({}, defaults, overrides)` is a prototype pattern in spirit.

---

## 2.2 Structural Patterns

Structural patterns deal with object composition — how classes and objects are combined to form larger structures.

### Adapter

**Intent:** Convert the interface of a class into another interface that clients expect. Lets incompatible interfaces work together.

```
Client ──> [Target Interface] <── Adapter ──> [Adaptee (legacy/external)]
```

```typescript
// Legacy payment system
class LegacyPaymentSystem {
  makePayment(amount: number, currency: string, accountNo: string): boolean {
    // old API
    return true;
  }
}

// New interface your system expects
interface PaymentGateway {
  charge(payment: { amount: number; currency: string; recipient: string }): Promise<boolean>;
}

// Adapter bridges the gap
class LegacyPaymentAdapter implements PaymentGateway {
  constructor(private legacy: LegacyPaymentSystem) {}

  async charge(payment: { amount: number; currency: string; recipient: string }): Promise<boolean> {
    return this.legacy.makePayment(payment.amount, payment.currency, payment.recipient);
  }
}
```

**Real-world:** Anti-Corruption Layer in DDD (§8) is an Adapter at the service boundary. Strangler Fig migrations use adapters to route traffic between old and new systems.

---

### Decorator

**Intent:** Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.

```
Component (interface)
    △
    │
ConcreteComponent
    △
    │
Decorator (wraps Component)
    △
    │
ConcreteDecorator (adds behavior)
```

```typescript
interface DataSource {
  writeData(data: string): void;
  readData(): string;
}

class FileDataSource implements DataSource {
  writeData(data: string) { /* write to file */ }
  readData(): string { return ''; /* read from file */ }
}

class CompressionDecorator implements DataSource {
  constructor(private source: DataSource) {}
  writeData(data: string) { this.source.writeData(compress(data)); }
  readData(): string { return decompress(this.source.readData()); }
}

class EncryptionDecorator implements DataSource {
  constructor(private source: DataSource) {}
  writeData(data: string) { this.source.writeData(encrypt(data)); }
  readData(): string { return decrypt(this.source.readData()); }
}

// Stack decorators
const source = new EncryptionDecorator(
  new CompressionDecorator(
    new FileDataSource()
  )
);
```

**Real-world:** HTTP middleware chains (Express.js), Python decorators (`@cache`, `@retry`), logging wrappers, TypeScript class decorators (NestJS guards, interceptors).

---

### Facade

**Intent:** Provide a simplified interface to a complex subsystem.

```typescript
// Complex subsystem
class VideoEncoder { encode(path: string) {} }
class AudioMixer { mix(path: string) {} }
class ThumbnailGenerator { generate(path: string) {} }
class CDNUploader { upload(path: string) {} }

// Facade
class VideoPublisher {
  private encoder = new VideoEncoder();
  private mixer = new AudioMixer();
  private thumbnailer = new ThumbnailGenerator();
  private uploader = new CDNUploader();

  publish(videoPath: string) {
    this.encoder.encode(videoPath);
    this.mixer.mix(videoPath);
    this.thumbnailer.generate(videoPath);
    this.uploader.upload(videoPath);
  }
}
```

**Real-world:** AWS SDK clients are facades over HTTP APIs. BFF pattern (§3.9) is a Facade for frontend clients. Any "orchestration service" is usually a Facade.

---

### Proxy

**Intent:** Provide a surrogate or placeholder for another object to control access to it.

Types: **Virtual Proxy** (lazy loading), **Protection Proxy** (access control), **Remote Proxy** (local representative of remote object), **Caching Proxy**.

```typescript
interface UserService {
  getUser(id: string): User;
}

class RealUserService implements UserService {
  getUser(id: string): User { /* DB call */ return {} as User; }
}

class CachingUserServiceProxy implements UserService {
  private cache = new Map<string, User>();
  constructor(private real: RealUserService) {}

  getUser(id: string): User {
    if (this.cache.has(id)) return this.cache.get(id)!;
    const user = this.real.getUser(id);
    this.cache.set(id, user);
    return user;
  }
}
```

**Real-world:** Service meshes (Envoy, Istio sidecars) are proxies. API Gateway is a proxy. ORM lazy loading uses virtual proxies.

---

### Composite

**Intent:** Compose objects into tree structures to represent part-whole hierarchies. Lets clients treat individual objects and compositions uniformly.

**Real-world:** File systems (files and directories), UI component trees (React), Terraform module graphs, GitHub Actions job dependency chains.

---

### Bridge

**Intent:** Decouple an abstraction from its implementation so the two can vary independently.

**When to use:** When you have orthogonal dimensions of variation (e.g., shape AND rendering platform). Avoids a class explosion.

---

### Flyweight

**Intent:** Use sharing to efficiently support large numbers of fine-grained objects.

**Real-world:** String interning in JVMs, character objects in text editors, shared config objects in distributed systems.

---

## 2.3 Behavioral Patterns

Behavioral patterns focus on communication and responsibility between objects.

### Strategy

**Intent:** Define a family of algorithms, encapsulate each one, and make them interchangeable.

```typescript
interface SortStrategy {
  sort(data: number[]): number[];
}

class QuickSort implements SortStrategy {
  sort(data: number[]): number[] { /* quicksort */ return data; }
}

class MergeSort implements SortStrategy {
  sort(data: number[]): number[] { /* mergesort */ return data; }
}

class DataProcessor {
  constructor(private sorter: SortStrategy) {}
  process(data: number[]): number[] { return this.sorter.sort(data); }
}
```

**Real-world:** Authentication strategies (Passport.js), payment processors, compression algorithms, A/B testing (different UI strategies), feature flags toggling strategies.

---

### Observer

**Intent:** Define a one-to-many dependency so that when one object changes state, all its dependents are notified automatically.

```
Subject ──publishes──> Event
   △                     │
   │                     ▼
Observers ◄──notified── EventBus
```

```typescript
interface Observer { update(event: string, data: unknown): void; }

class EventBus {
  private subscribers = new Map<string, Observer[]>();

  subscribe(event: string, observer: Observer): void {
    if (!this.subscribers.has(event)) this.subscribers.set(event, []);
    this.subscribers.get(event)!.push(observer);
  }

  publish(event: string, data: unknown): void {
    this.subscribers.get(event)?.forEach(obs => obs.update(event, data));
  }
}
```

**Real-world:** This pattern underpins **Event-Driven Architecture** (§3.5). DOM event listeners, React state updates, Kafka topic subscriptions, GitHub webhook events.

---

### Command

**Intent:** Encapsulate a request as an object, allowing parameterization, queuing, logging, and undoable operations.

```typescript
interface Command { execute(): void; undo(): void; }

class CreateFileCommand implements Command {
  constructor(private path: string) {}
  execute() { fs.writeFileSync(this.path, ''); }
  undo() { fs.unlinkSync(this.path); }
}

class CommandHistory {
  private history: Command[] = [];
  execute(cmd: Command) { cmd.execute(); this.history.push(cmd); }
  undo() { this.history.pop()?.undo(); }
}
```

**Real-world:** CQRS command objects (§3.10), undo/redo in editors, task queues, database migration runners, Terraform plan/apply cycle.

---

### Template Method

**Intent:** Define the skeleton of an algorithm in a base class, deferring some steps to subclasses.

**Real-world:** GitHub Actions reusable workflows are a template method — the skeleton is defined centrally, steps are overridden per caller. CI/CD pipeline templates. Data ETL pipelines.

---

### Chain of Responsibility

**Intent:** Pass requests along a chain of handlers. Each handler decides to process the request or pass it on.

**Real-world:** HTTP middleware (Express `app.use()`), authentication pipelines (check API key → check JWT → check role), GitHub Actions step conditions.

---

### State

**Intent:** Allow an object to alter its behavior when its internal state changes. The object will appear to change its class.

**Real-world:** Order state machines (pending → confirmed → shipped → delivered), CI/CD job states (queued → running → success/failure), GitHub PR lifecycle.

---

### Iterator

**Intent:** Provide a way to access elements of a collection sequentially without exposing the underlying representation.

**Modern relevance:** Fully baked into modern languages (`for...of`, Python generators, Java Streams). Still useful when designing custom data structures.

---

### Visitor

**Intent:** Represent an operation to be performed on elements of an object structure. Lets you define a new operation without changing the classes of the elements.

**Real-world:** AST traversal in compilers/linters (ESLint uses visitors), report generation across heterogeneous object hierarchies.

---

### Mediator

**Intent:** Define an object that encapsulates how a set of objects interact, reducing direct dependencies between them.

**Real-world:** Message brokers (Kafka, RabbitMQ) are Mediators at scale. Chat room servers. Event buses. The BFF (§3.9) can act as a Mediator between backend services and frontend.

---

### Memento

**Intent:** Without violating encapsulation, capture and externalize an object's internal state so the object can be restored later.

**Real-world:** Undo mechanisms, snapshot-based backups, Terraform state files.

---

### Interpreter

**Intent:** Given a language, define a representation for its grammar and an interpreter that uses the representation to interpret sentences.

**Real-world:** SQL parsing, DSLs, Terraform HCL parsing, Regular expressions.

---

## 2.4 Modern Relevance of GoF Patterns

| Pattern | Status | Notes |
|---|---|---|
| Singleton | ⚠️ Use with care | Prefer DI containers; avoid global mutable state |
| Factory Method | ✅ Still core | Widely used in frameworks |
| Abstract Factory | ✅ Still core | Useful for multi-cloud/multi-env abstractions |
| Builder | ✅ Very relevant | Fluent APIs, query builders, test data |
| Prototype | ⚠️ Language-native | JS prototypes, `structuredClone()` |
| Observer | ✅ Foundational | Basis of reactive programming and EDA |
| Strategy | ✅ Very relevant | Replaces complex conditionals cleanly |
| Command | ✅ Very relevant | CQRS, task queues, undo systems |
| Decorator | ✅ Very relevant | Middleware, cross-cutting concerns |
| Adapter | ✅ Very relevant | Integration, anti-corruption layers |
| Facade | ✅ Very relevant | BFF, SDK design |
| Iterator | ✅ Language-native | Built into all modern languages |
| Visitor | ⚠️ Niche | Useful for ASTs, compilers |
| Mediator | ✅ Scales to infra | Message brokers at architecture level |

# 3. Architectural Styles

Architectural styles are high-level templates that define how a system is structured, how components communicate, and what constraints govern the design. Choosing the right style is one of the most consequential decisions in a system's lifecycle.

---

## 3.1 Monolithic Architecture (Baseline)

**Definition:** A single deployable unit where all components — UI, business logic, data access — run in the same process and are deployed together.

```
┌─────────────────────────────────────┐
│           Monolith                  │
│  ┌─────────┐  ┌─────────────────┐  │
│  │   UI    │  │  Business Logic │  │
│  └────┬────┘  └───────┬─────────┘  │
│       └───────────────┘            │
│              │                     │
│       ┌──────▼──────┐              │
│       │  Data Layer │              │
│       └──────┬──────┘              │
└──────────────┼──────────────────────┘
               │
        ┌──────▼──────┐
        │  Database   │
        └─────────────┘
```

**When to use:**

- Early-stage products where speed of iteration matters more than scale
- Small teams (1–5 engineers) where coordination overhead of microservices isn't worth it
- When the domain isn't well understood yet (don't distribute what you don't understand)

**Tradeoffs:**

| Pros | Cons |
|---|---|
| Simple to develop, test, and deploy | Scales as a unit (can't scale parts independently) |
| Easy to trace and debug | Long build/deploy times as it grows |
| Low operational overhead | High coupling risk over time |
| Strong consistency (single DB) | Risky deployments (all-or-nothing) |
| Great for early-stage products | Hard to adopt new technology incrementally |

> **Practitioner Note:** "Monolith" is not a dirty word. Most successful companies started with one, and many (Shopify, Stack Overflow) run very large monoliths in production today. The mistake is not starting with a monolith — it's staying in one *long after you've outgrown it*, or jumping to microservices before you understand your domain.

---

## 3.2 Modular Monolith

**Definition:** A monolith that is internally structured into well-defined, loosely-coupled modules with explicit interfaces between them. It deploys as one unit but is designed as if it could be split.

```
┌────────────────────────────────────────────────┐
│                  Monolith                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────────┐ │
│  │  Orders  │  │   Users  │  │   Billing    │ │
│  │  Module  │  │  Module  │  │   Module     │ │
│  └────┬─────┘  └────┬─────┘  └──────┬───────┘ │
│       │ (internal contracts only)    │         │
│       └─────────────┬────────────────┘         │
│               ┌─────▼──────┐                   │
│               │  Shared DB │                   │
│               └────────────┘                   │
└────────────────────────────────────────────────┘
```

**The key discipline:** Modules must not directly call each other's internal functions. They communicate through published interfaces (function calls, internal events, or a shared kernel). No module accesses another module's database tables directly.

**When to use:**

- When a monolith is getting messy but you're not ready (or don't need) to go to microservices
- When the team is growing but coordination costs of distributed deployment are too high
- When you want the *option* to extract services later without the complexity now

**Real-world:** Ruby on Rails with Packwerk (Shopify's gem for enforcing module boundaries), .NET with Projects/Namespaces, Java with Maven modules.

**Migration path:**

```
Big Ball of Mud → Modular Monolith → Selective Service Extraction
```

The modular monolith is the *intermediate step* that most teams skip, and they regret it. Jumping from a tangled monolith directly to microservices distributes the mess across the network — now you have a distributed ball of mud.

> **Practitioner Note:** The Modular Monolith deserves a renaissance. If your team is under 20 engineers, your system serves fewer than 10M users, and you don't have truly independent scaling requirements — a well-structured modular monolith will outperform a microservices system in developer velocity, operational simplicity, and debuggability. Start here, not at microservices.

---

## 3.3 Three-Layer / N-Tier Architecture

**Definition:** Organize the application into horizontal layers — each layer only communicates with the layer directly below it.

```
┌─────────────────────────────┐
│     Presentation Layer      │  ← Controllers, Views, API endpoints
├─────────────────────────────┤
│     Business Logic Layer    │  ← Services, Domain Objects, Use Cases
├─────────────────────────────┤
│      Data Access Layer      │  ← Repositories, ORMs, DB queries
└─────────────────────────────┘
           │
    ┌──────▼──────┐
    │  Database   │
    └─────────────┘
```

**Layer responsibilities:**

- **Presentation:** Handles HTTP, serialization/deserialization, input validation, authentication
- **Business Logic:** Implements use cases, enforces business rules, coordinates between entities
- **Data Access:** Abstracts persistence — hides SQL, ORM details, caching logic

**When to use:** Almost always appropriate as a starting point. It provides clear separation without over-engineering.

**Common Violation — Layer Skipping:**

```typescript
// VIOLATION: Controller directly queries the database
class OrderController {
  async getOrder(req: Request, res: Response) {
    const order = await db.query('SELECT * FROM orders WHERE id = ?', [req.params.id]);
    res.json(order);
  }
}

// CORRECT: Goes through the service layer
class OrderController {
  constructor(private orderService: OrderService) {}
  async getOrder(req: Request, res: Response) {
    const order = await this.orderService.findById(req.params.id);
    res.json(order);
  }
}
```

**Limitation:** Three-layer architecture doesn't address the dependency direction problem — business logic often ends up depending on data access implementations. **Clean Architecture** (§3.4) solves this.

---

## 3.4 Clean Architecture / Onion Architecture / Hexagonal (Ports & Adapters)

These three are closely related. They all share the same core insight: **business logic should depend on nothing outside itself**. All dependencies flow inward.

### Clean Architecture (Uncle Bob)

```
        ┌─────────────────────────────────────┐
        │           Frameworks & Drivers       │  ← Web, DB, UI, Devices
        │   ┌─────────────────────────────┐   │
        │   │       Interface Adapters     │   │  ← Controllers, Presenters, Gateways
        │   │   ┌─────────────────────┐   │   │
        │   │   │    Application       │   │   │  ← Use Cases / Interactors
        │   │   │  ┌───────────────┐  │   │   │
        │   │   │  │   Entities    │  │   │   │  ← Enterprise Business Rules
        │   │   │  └───────────────┘  │   │   │
        │   │   └─────────────────────┘   │   │
        │   └─────────────────────────────┘   │
        └─────────────────────────────────────┘
                Dependency Rule: Only pointing INWARD →
```

**The Dependency Rule:** Source code dependencies can only point inward. Nothing in an inner circle can know anything about something in an outer circle. The database, the web framework, the UI — these are all details. The domain is the core.

### Hexagonal Architecture (Ports & Adapters)

```
              ┌───────────────────────────────────┐
  HTTP ───────┤ Port (Driving)  APPLICATION  Port  ├──────── Database
  CLI  ───────┤                  CORE/DOMAIN       ├──────── Email Service
  Tests ──────┤ (Use Cases)                        ├──────── File System
              └───────────────────────────────────┘
                        Adapters connect to Ports
```

**Ports** are interfaces defined by the application core. **Adapters** are implementations that connect external systems to those ports.

```typescript
// CORE: Port (interface defined by domain)
interface NotificationPort {
  sendWelcome(user: User): Promise<void>;
}

// CORE: Use case depends only on the port
class RegisterUserUseCase {
  constructor(
    private users: UserRepository,      // Port
    private notifier: NotificationPort  // Port
  ) {}

  async execute(cmd: RegisterUserCommand): Promise<User> {
    const user = User.create(cmd.name, cmd.email);
    await this.users.save(user);
    await this.notifier.sendWelcome(user);
    return user;
  }
}

// ADAPTER: Lives in the infrastructure layer
class SendGridNotificationAdapter implements NotificationPort {
  async sendWelcome(user: User): Promise<void> {
    await sendgrid.send({ to: user.email, template: 'welcome' });
  }
}

// TEST ADAPTER: Used in unit tests
class FakeNotificationAdapter implements NotificationPort {
  sent: User[] = [];
  async sendWelcome(user: User): Promise<void> { this.sent.push(user); }
}
```

**Key benefit:** The use case can be tested in complete isolation — just swap in the fake adapters. No database, no HTTP, no email service needed.

**When to use Clean/Hexagonal:**

- Systems with complex domain logic that changes frequently
- Systems where the infrastructure (database, framework) might change
- When testability is critical

**When it's overkill:**

- Simple CRUD applications
- Short-lived scripts or utilities
- Small teams where the overhead of strict layering slows iteration

> **Practitioner Note:** Clean Architecture works best when you have real domain complexity. If your "business logic" is just moving data from an HTTP request to a database, you don't need five layers. The signal to move toward it: when your controllers are doing business logic, when your services are directly calling ORMs, and when changing the database requires changing business rules.

---

## 3.5 Event-Driven Architecture (EDA)

**Definition:** Components communicate through events — messages that represent something that happened. Producers emit events without knowing who consumes them. Consumers react to events without knowing who produced them.

```
┌─────────────┐         ┌──────────────────┐         ┌──────────────────┐
│  Producer   │─publish→│   Event Broker   │─consume→│   Consumer(s)    │
│ (Order Svc) │         │ (Kafka/RabbitMQ) │         │ (Inventory Svc,  │
└─────────────┘         └──────────────────┘         │  Billing Svc,    │
                                                     │  Notification)   │
                                                     └──────────────────┘
```

**Event types:**

- **Event Notification:** "Something happened" (minimal data, consumer fetches details)
- **Event-Carried State Transfer:** Full state in the event payload (consumer is self-sufficient)
- **Domain Event:** Business-meaningful occurrence (`OrderPlaced`, `PaymentFailed`, `UserRegistered`)

### EDA Patterns

**Fan-out:** One event → multiple consumers in parallel
**Event Chain:** Consumer of Event A produces Event B
**Dead Letter Queue (DLQ):** Events that fail processing go to a holding queue for inspection

```
OrderPlaced ──→ InventoryService (reserve stock)
            ──→ BillingService (charge card)
            ──→ NotificationService (email customer)
            ──→ AnalyticsService (update dashboard)
```

**When to use:**

- When services need to react to state changes without polling
- When you want to decouple producers from consumers (neither needs to know the other exists)
- When you need audit trails or event replay capabilities
- Async processing pipelines (image processing, report generation)

**When to avoid:**

- Simple request/response scenarios — EDA adds complexity without benefit
- When strong consistency is required (eventual consistency is inherent to EDA)
- When you need synchronous feedback (e.g., "did the payment succeed right now?")

**Tradeoffs:**

| Pros | Cons |
|---|---|
| Loose coupling between services | Hard to trace flows across services |
| Easy to add new consumers | Eventual consistency is complex |
| Resilient to partial failures | Event schema evolution is tricky |
| Natural audit log | Debugging requires good tooling |

> **Practitioner Note:** EDA is not "fire and forget and hope for the best." You need dead letter queues, idempotent consumers, schema registries (Avro/Protobuf/JSONSchema), distributed tracing, and event replay capabilities. Invest in observability before you invest in events.

---

## 3.6 Microservices Architecture

**Definition:** Structure an application as a collection of small, independently deployable services, each responsible for a specific business capability and communicating over a network.

```
          ┌─────────────────────────────────────────────────────┐
          │                   API Gateway                        │
          └────────────┬──────────────────┬─────────────────────┘
                       │                  │
          ┌────────────▼───┐         ┌────▼─────────────┐
          │  Order Service  │         │  Payment Service  │
          │   (own DB)      │         │   (own DB)        │
          └────────────────┘         └──────────────────┘
                       │                  │
               ┌───────▼──────────────────▼──────┐
               │         Message Broker           │
               │         (Kafka/RabbitMQ)          │
               └───────────┬──────────────────────┘
                           │
               ┌───────────▼──────────┐
               │  Notification Service │
               │   (own DB)            │
               └──────────────────────┘
```

**The core principles:**

- **Single responsibility per service:** Each service owns one business capability
- **Decentralized data:** Each service owns its own database — no shared schemas
- **Deploy independently:** A change to the Order Service doesn't require redeploying Payment Service
- **Failure isolation:** A crash in Notification Service doesn't bring down Order processing

**When microservices make sense:**

- Teams scale to 20+ engineers and Conway's Law starts affecting architecture
- Different services need different scaling characteristics (search scales differently than auth)
- Different tech stacks are genuinely required (ML service in Python, web API in Go)
- Independent deployment velocity is a real business need

**When microservices are premature:**

- Under 10 engineers — coordination overhead kills velocity
- When you don't understand the domain well enough to define service boundaries
- When your monolith isn't actually causing pain yet

**The hidden costs:**

- Network latency (service calls now cross network, not in-process)
- Distributed transactions (no ACID across services — see Saga pattern in §4)
- Operational complexity (N services × N environments × CI/CD per service)
- Observability (need distributed tracing, centralized logging, service meshes)
- Testing (integration testing across services is hard)

> **Practitioner Note:** The best microservices architectures I've seen started as modular monoliths. The team knew the domain deeply before drawing service boundaries. Service boundaries that map to DDD Bounded Contexts (§8) are almost always better than those drawn by technical layer or guesswork.

---

## 3.7 Distributed Architecture — General Principles

Distributed systems are systems where components on networked computers communicate and coordinate to achieve a common goal. This section covers the fundamental constraints and challenges.

### CAP Theorem

Any distributed data store can only guarantee **two** of three properties simultaneously:

```
            Consistency
               /\
              /  \
             /    \
            /      \
 Partition ──────── Availability
Tolerance
```

- **Consistency (C):** Every read receives the most recent write or an error
- **Availability (A):** Every request receives a response (not guaranteed to be the most recent)
- **Partition Tolerance (P):** The system continues operating despite network partition

**In practice:** Network partitions happen (P is non-negotiable). So you choose between **CP** (consistent but may be unavailable during partition) and **AP** (available but may return stale data during partition).

| System | Choice | Example |
|---|---|---|
| Traditional RDBMS | CA (no distribution) | PostgreSQL single node |
| HBase, Zookeeper | CP | Will reject writes during partition |
| Cassandra, DynamoDB | AP | Returns stale data during partition |
| MongoDB | Configurable | Default CP |

### BASE vs ACID

**ACID** (traditional relational databases):

- **A**tomic: All or nothing
- **C**onsistent: Always valid state
- **I**solated: Transactions don't interfere
- **D**urable: Committed data is permanent

**BASE** (distributed systems reality):

- **B**asically **A**vailable: System appears available most of the time
- **S**oft state: State may change even without input (due to eventual consistency)
- **E**ventually consistent: System will become consistent over time

### Eventual Consistency Patterns

**Read-your-writes consistency:** A user always sees their own updates (route user reads to the primary)
**Monotonic reads:** A user never sees older data after seeing newer data (sticky sessions to replica)
**Causal consistency:** Causally related operations appear in the correct order

---

## 3.8 Serverless Architecture

**Definition:** Build applications from functions that are triggered by events, managed and scaled automatically by the cloud provider. You don't manage servers, capacity, or runtime environments.

```
HTTP Request → API Gateway → Lambda Function → DynamoDB / S3 / SQS
                                    │
                    (Auto-scaled, pay-per-invocation)
```

**When to use:**

- Event-driven workloads with spiky or unpredictable traffic
- Background jobs, file processing, webhooks
- Rapid prototyping where infrastructure management is a distraction

**Tradeoffs:**

| Pros | Cons |
|---|---|
| Zero infrastructure management | Cold start latency |
| Auto-scaling to zero | Vendor lock-in |
| Pay-per-use (cost efficient for low traffic) | Debugging is harder |
| Fast deployment | Execution time limits |
| | State management complexity |

---

## 3.9 Backend for Frontend (BFF)

**Definition:** Create a dedicated backend service (or API layer) for each frontend client — one BFF for the web app, one for the mobile app, one for third-party APIs — tailored to the specific needs of each.

```
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   Web BFF    │  │  Mobile BFF  │  │  Partner BFF │
│ (optimized   │  │ (optimized   │  │ (optimized   │
│  for SPA)    │  │  for mobile) │  │  for 3rd     │
└──────┬───────┘  └──────┬───────┘  │  party API)  │
       │                 │          └──────┬────────┘
       └─────────────────┼────────────────┘
                         │
         ┌───────────────┼───────────────┐
         ▼               ▼               ▼
    UserService     OrderService     ProductService
```

**Why BFF exists:** A single generic API serving multiple clients becomes bloated. The mobile app needs smaller payloads. The web app needs aggregated data across services. Third-party APIs need different auth schemes. A single API can't serve all these optimally without becoming a Swiss army knife.

**BFF responsibilities:**

- Aggregate data from multiple downstream services
- Transform/shape responses for the specific client's data model
- Handle client-specific auth flows
- Implement client-specific caching strategies
- Translate between client protocols and backend protocols

**When NOT to use BFF:**

- When you have one client
- When the API is truly generic and clients can handle transformation
- When your team can't afford to maintain N BFFs

> **Practitioner Note:** BFF is especially powerful in a microservices environment where the frontend would otherwise need to know about 10+ services. The BFF acts as a Facade (GoF) and Mediator — it owns the aggregation logic, keeping the frontend simple and the backend services focused.

---

## 3.10 CQRS + Event Sourcing

### CQRS (Command Query Responsibility Segregation)

**Definition:** Separate the read model (queries) from the write model (commands). Different data models, different optimizations, possibly different databases.

```
                    ┌──────────────────────────────┐
                    │        Application            │
                    └────────────┬─────────────────┘
              Commands           │           Queries
               ┌─────────────────┼─────────────────────┐
               ▼                 │                       ▼
    ┌──────────────────┐         │          ┌──────────────────────┐
    │  Command Handler │         │          │    Query Handler     │
    │  (Write Model)   │         │          │    (Read Model)      │
    └────────┬─────────┘         │          └──────────┬───────────┘
             │                   │                     │
    ┌────────▼─────────┐         │          ┌──────────▼───────────┐
    │  Write Database  │─events──┘          │    Read Database     │
    │  (normalized,    │──────────────────→ │    (denormalized,    │
    │   ACID)          │                    │     optimized views) │
    └──────────────────┘                    └──────────────────────┘
```

**Why separate?** Read patterns and write patterns have different characteristics:

- Writes need consistency, validation, business rule enforcement
- Reads need performance, flexibility, aggregations across entities

**When to use CQRS:**

- Complex domains with divergent read/write shapes
- High read-to-write ratios where read optimization matters
- Systems using Event Sourcing (§ below)

### Event Sourcing

**Definition:** Instead of storing the current state of an entity, store the **sequence of events** that led to that state. The current state is derived by replaying events.

```
Traditional DB:
  orders table: { id: 1, status: 'shipped', total: 150.00 }

Event Sourced:
  events table:
    { type: 'OrderCreated',    orderId: 1, total: 150.00,  ts: T+0 }
    { type: 'PaymentReceived', orderId: 1, amount: 150.00, ts: T+5 }
    { type: 'OrderShipped',    orderId: 1, carrier: 'UPS', ts: T+60 }
```

**Benefits:**

- Complete audit log of all changes — built in, free
- Ability to replay events to reconstruct state at any point in time
- Natural fit for Event-Driven Architecture
- Can project the same events into multiple read models

**Costs:**

- Increased complexity (event versioning, schema evolution)
- Querying is non-trivial (you project events into read models)
- Eventual consistency between write and read models

> **Practitioner Note:** CQRS and Event Sourcing are often mentioned together but are independent. You can do CQRS without Event Sourcing. Event Sourcing without CQRS is painful (querying event streams is hard). Combined, they're powerful — but only justified by genuine complexity. Don't introduce Event Sourcing just because it sounds elegant.

# 4. Distributed Systems Fundamentals

## 4.1 The Saga Pattern

In microservices, you can't use database transactions across service boundaries. The **Saga pattern** coordinates a sequence of local transactions across services, each publishing an event or message to trigger the next step. If a step fails, compensating transactions undo prior steps.

### Choreography-Based Saga

Services react to events — no central coordinator. Each service listens for events and decides what to do.

```
OrderService       InventoryService      PaymentService       NotificationService
     │                    │                    │                      │
     │──OrderCreated──→   │                    │                      │
     │                    │──StockReserved──→  │                      │
     │                    │                    │──PaymentProcessed──→ │
     │                    │                    │                      │──email sent
     │                    │                    │                      │
     │  (on failure)      │                    │                      │
     │                    │──StockReserveFailed→│                     │
     │←──OrderFailed──────│                    │                      │
```

**Pros:** No single point of failure, services are autonomous
**Cons:** Complex to trace, hard to reason about the overall flow, distributed business logic

### Orchestration-Based Saga

A central **Saga Orchestrator** tells each participant what to do and handles failures.

```
               ┌─────────────────────────────┐
               │      Order Saga Orchestrator │
               └────┬──────┬───────┬─────────┘
                    │      │       │
       Reserve Stock│ Charge│  Send │
                    ▼      ▼ Card  ▼ Email
             Inventory  Payment  Notification
              Service   Service   Service
```

**Pros:** Centralized logic, easier to trace, explicit flow definition
**Cons:** Orchestrator can become a bottleneck, introduces coupling to the coordinator

**Compensating Transactions:**

| Step | Forward Action | Compensating Action |
|---|---|---|
| 1 | Reserve inventory | Release inventory reservation |
| 2 | Charge credit card | Issue refund |
| 3 | Send order confirmation | Send cancellation email |

---

## 4.2 Service Discovery, Load Balancing, Circuit Breakers

### Service Discovery

In dynamic environments (Kubernetes, ECS), services come and go. Service Discovery lets services find each other without hardcoded addresses.

```
Client → [Service Registry] → available instances of ServiceB
                (Consul, Eureka, Kubernetes DNS)
```

**Client-side discovery:** Client queries the registry and decides which instance to call (Eureka pattern)
**Server-side discovery:** Client calls a load balancer; the LB queries the registry (Kubernetes Services)

### Load Balancing

Distribute traffic across instances:

- **Round-robin:** Rotate through instances in order
- **Least connections:** Route to instance with fewest active connections
- **Weighted:** Route more traffic to more capable instances
- **Sticky sessions:** Route user's requests to the same instance

### Circuit Breaker

Prevent cascading failures when a downstream service is failing.

```
States:
  CLOSED ──(failures exceed threshold)──→ OPEN
  OPEN   ──(timeout expires)────────────→ HALF-OPEN
  HALF-OPEN ──(test call succeeds)──────→ CLOSED
  HALF-OPEN ──(test call fails)─────────→ OPEN
```

```typescript
class CircuitBreaker {
  private failures = 0;
  private state: 'CLOSED' | 'OPEN' | 'HALF_OPEN' = 'CLOSED';
  private nextAttempt = Date.now();

  async call<T>(fn: () => Promise<T>): Promise<T> {
    if (this.state === 'OPEN') {
      if (Date.now() < this.nextAttempt) throw new Error('Circuit is OPEN');
      this.state = 'HALF_OPEN';
    }

    try {
      const result = await fn();
      this.onSuccess();
      return result;
    } catch (err) {
      this.onFailure();
      throw err;
    }
  }

  private onSuccess() {
    this.failures = 0;
    this.state = 'CLOSED';
  }

  private onFailure() {
    this.failures++;
    if (this.failures >= 5) {
      this.state = 'OPEN';
      this.nextAttempt = Date.now() + 30_000; // 30s
    }
  }
}
```

---

## 4.3 Distributed Tracing and Observability

The **Three Pillars of Observability:**

| Pillar | What It Answers | Tools |
|---|---|---|
| **Logs** | What happened? | ELK, Loki, CloudWatch |
| **Metrics** | How is it performing? | Prometheus, Datadog, CloudWatch |
| **Traces** | Where did the time go across services? | Jaeger, Zipkin, Datadog APM, OTEL |

### Distributed Tracing Concepts

A **trace** represents a complete request across multiple services. A trace contains **spans** — individual operations. Each span has a `traceId` (same across all services) and a `spanId` (unique per operation).

```
[Trace: abc-123]
  [Span: API Gateway, 10ms]
    [Span: Order Service, 50ms]
      [Span: DB Query, 30ms]
      [Span: Inventory Service call, 15ms]
        [Span: Inventory DB, 10ms]
```

**OpenTelemetry (OTEL)** is the vendor-neutral standard for emitting traces, metrics, and logs. Use it instead of vendor-specific SDKs.

---

## 4.4 Idempotency

**Definition:** An operation is idempotent if performing it multiple times produces the same result as performing it once.

Critical in distributed systems because:

- Networks fail — messages may be retried
- At-least-once delivery guarantees duplicate messages
- Consumer restarts can re-process messages

```typescript
// Non-idempotent: creates duplicate if called twice with same data
async function createOrder(order: OrderInput) {
  return db.insert('orders', order);
}

// Idempotent: uses idempotency key to detect duplicates
async function createOrder(order: OrderInput, idempotencyKey: string) {
  const existing = await db.findByKey('orders', idempotencyKey);
  if (existing) return existing; // Return the same result as before
  return db.insert('orders', { ...order, idempotencyKey });
}
```

**Delivery guarantees:**

- **At-most-once:** Message delivered 0 or 1 times (may lose messages)
- **At-least-once:** Message delivered 1 or more times (may duplicate — make consumers idempotent)
- **Exactly-once:** Message delivered exactly once (hardest to achieve, often approximated)

---

## 4.5 Consensus Algorithms (Conceptual)

When distributed nodes need to agree on a value (leader election, configuration), consensus algorithms ensure agreement even in the presence of failures.

**Raft:** Understandable leader-based consensus. Used by etcd (Kubernetes), CockroachDB. Nodes elect a leader; the leader manages log replication.

**Paxos:** The academic foundation, notoriously complex to implement. Phases: Prepare → Promise → Accept → Accepted.

**Practical relevance:** You rarely implement these yourself. You use systems that do (etcd, Zookeeper, Kafka controller). Know what they provide: strong consistency, leader election, distributed locks.

---

# 5. Cloud Architecture Patterns

## 5.1 The 12-Factor App

A methodology for building software-as-a-service apps that are scalable, maintainable, and portable across cloud environments.

| Factor | Principle |
|---|---|
| **1. Codebase** | One codebase tracked in version control, many deploys |
| **2. Dependencies** | Explicitly declare and isolate dependencies |
| **3. Config** | Store config in the environment (not in code) |
| **4. Backing services** | Treat backing services as attached resources |
| **5. Build, release, run** | Strictly separate build and run stages |
| **6. Processes** | Execute app as one or more stateless processes |
| **7. Port binding** | Export services via port binding |
| **8. Concurrency** | Scale out via the process model |
| **9. Disposability** | Maximize robustness with fast startup and graceful shutdown |
| **10. Dev/prod parity** | Keep development, staging, and production as similar as possible |
| **11. Logs** | Treat logs as event streams |
| **12. Admin processes** | Run admin/management tasks as one-off processes |

> **Practitioner Note:** Factors 3 (Config in environment), 6 (Stateless processes), 9 (Disposability), and 11 (Logs as streams) are the most impactful for cloud-native deployment. If your app stores session state in memory or writes config to disk, it can't scale horizontally.

---

## 5.2 Resilience Patterns

### Retry Pattern

```typescript
async function withRetry<T>(
  fn: () => Promise<T>,
  maxAttempts = 3,
  backoffMs = 1000
): Promise<T> {
  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await fn();
    } catch (error) {
      if (attempt === maxAttempts) throw error;
      const delay = backoffMs * Math.pow(2, attempt - 1); // Exponential backoff
      await sleep(delay);
    }
  }
  throw new Error('Unreachable');
}
```

**Always use exponential backoff with jitter** to avoid thundering herd (all retries hitting at the same time).

### Bulkhead Pattern

Isolate resources so that a failure in one area doesn't exhaust resources needed by others. Named after ship compartments that prevent flooding.

```
Without Bulkhead:          With Bulkhead:
  Thread Pool               Thread Pool A (Payment)
  [P P P P P P]             [P P P P P P]
  ← Payment →              Thread Pool B (Orders)
  ← Orders  →               [O O O O O O]
  If payment hangs,         If payment hangs,
  orders starve too.        orders still have threads.
```

**In cloud context:** Separate Lambda concurrency limits, separate RDS connection pools, separate Kubernetes resource limits per namespace.

### Timeout Pattern

Never wait forever. Always set timeouts on external calls.

```typescript
async function fetchWithTimeout<T>(fn: () => Promise<T>, timeoutMs: number): Promise<T> {
  return Promise.race([
    fn(),
    new Promise<never>((_, reject) =>
      setTimeout(() => reject(new Error('Timeout')), timeoutMs)
    )
  ]);
}
```

### Fallback Pattern

When a service fails, degrade gracefully rather than failing completely.

```typescript
async function getProductRecommendations(userId: string): Promise<Product[]> {
  try {
    return await recommendationService.get(userId); // personalized
  } catch {
    return await productService.getBestsellers(); // fallback: generic list
  }
}
```

---

## 5.3 Container and Service Mesh Patterns

### Sidecar Pattern

Deploy a helper container alongside your main application container in the same pod. The sidecar handles cross-cutting concerns without changing the app.

```
┌─────────────────────────────────────────────┐
│                    Pod                      │
│  ┌─────────────────┐  ┌──────────────────┐  │
│  │  App Container  │  │ Sidecar Container│  │
│  │  (business      │  │ (logging, tracing│  │
│  │   logic)        │  │  service mesh,   │  │
│  │                 │  │  config reload)  │  │
│  └─────────────────┘  └──────────────────┘  │
│         Shared network namespace            │
└─────────────────────────────────────────────┘
```

**Real-world:** Envoy proxy as Istio sidecar, Datadog agent sidecar, Vault agent sidecar for secret injection.

### Ambassador Pattern

A sidecar that acts as a proxy for outbound traffic — adds retry, circuit breaking, logging without the app knowing.

### Adapter Pattern (Container)

A sidecar that transforms the app's output to match a standard format expected by the platform (e.g., normalizing log formats).

---

## 5.4 Migration Patterns

### Strangler Fig Pattern

Gradually replace a legacy system by routing traffic to new services while the old system handles the rest. The new system "strangles" the old one over time.

```
Phase 1: All traffic → Legacy
Phase 2: /api/users → New Service; rest → Legacy
Phase 3: /api/users + /api/orders → New; rest → Legacy
Phase 4: All traffic → New; Legacy decommissioned
```

```
         ┌─────────────────┐
HTTP ──→  │ Routing Layer   │ ──→ New Service (migrated routes)
          │ (Nginx/Gateway) │
          └─────────────────┘ ──→ Legacy System (unmigrated)
```

**Implementation:** A reverse proxy (Nginx, API Gateway, or custom facade) routes based on path, header, or feature flags. Critically, the legacy system is not modified.

**In platform/DevOps context:** When migrating CI/CD from Jenkins to GitHub Actions, you can run both systems in parallel, gradually shifting pipelines to GHA while Jenkins handles remaining jobs — a Strangler Fig for infrastructure.

### Anti-Corruption Layer (ACL)

A translation layer between your domain and an external system (legacy, third-party) that ensures the external system's model doesn't pollute your domain model.

```typescript
// External system uses its own model
interface LegacyCustomerDto {
  cust_id: string;
  cust_nm: string;
  cust_email_addr: string;
}

// Your domain model
interface Customer {
  id: string;
  name: string;
  email: string;
}

// ACL translates
class LegacyCustomerACL {
  translate(dto: LegacyCustomerDto): Customer {
    return {
      id: dto.cust_id,
      name: dto.cust_nm,
      email: dto.cust_email_addr
    };
  }
}
```

---

## 5.5 Multi-Tenancy Patterns

| Pattern | Description | Isolation | Cost |
|---|---|---|---|
| **Silo (one infra per tenant)** | Each tenant has their own DB, compute | Highest | Highest |
| **Bridge (shared compute, separate DB)** | Shared app layer, separate database per tenant | Medium | Medium |
| **Pool (fully shared)** | All tenants share everything, tenant ID discriminator | Lowest | Lowest |

**Choice drivers:** Regulatory requirements (data residency), tenant SLA needs, cost model, customization requirements.

---

# 6. REST API Design

## 6.1 REST Constraints

REST (Representational State Transfer) was defined by Roy Fielding in his 2000 dissertation. It has six architectural constraints:

1. **Client-Server:** Separation of concerns between UI and data storage
2. **Stateless:** Each request contains all information needed to process it; no session state on server
3. **Cacheable:** Responses must define themselves as cacheable or non-cacheable
4. **Uniform Interface:** Resources are identified in requests; manipulation through representations; self-descriptive messages; HATEOAS
5. **Layered System:** Client doesn't know if it's connected to the end server or an intermediary
6. **Code on Demand (optional):** Servers can send executable code to clients (e.g., JavaScript)

---

## 6.2 Richardson Maturity Model

A scale for measuring how "RESTful" an API is.

```
Level 3: Hypermedia Controls (HATEOAS)
Level 2: HTTP Verbs (GET, POST, PUT, DELETE properly)
Level 1: Individual Resources (/users/123, /orders/456)
Level 0: Single endpoint (RPC over HTTP)
```

**Level 0:** Everything goes to one endpoint: `POST /api?method=getUser`
**Level 1:** Separate resources: `GET /users/123`
**Level 2:** HTTP verbs convey semantics: `DELETE /users/123` vs `POST /users`
**Level 3:** Responses include links to related actions (HATEOAS)

Most production APIs target Level 2. Level 3 (HATEOAS) is theoretically powerful but rarely practical.

---

## 6.3 Resource Modeling

```
Good resource naming:
  /users              (collection)
  /users/{id}         (individual resource)
  /users/{id}/orders  (sub-resource)
  /orders/{id}/items  (sub-resource)

Avoid:
  /getUser            (verb in URL)
  /users/delete       (action in URL)
  /user               (inconsistent pluralization)
  /api/v1/get-all-users-by-status  (not a resource)
```

**HTTP Verb Semantics:**

| Method | Semantic | Idempotent? | Safe? |
|---|---|---|---|
| GET | Retrieve | Yes | Yes |
| POST | Create / action | No | No |
| PUT | Replace (full update) | Yes | No |
| PATCH | Partial update | No* | No |
| DELETE | Remove | Yes | No |

*PATCH can be made idempotent depending on implementation.

---

## 6.4 Versioning Strategies

| Strategy | Example | Pros | Cons |
|---|---|---|---|
| **URL versioning** | `/api/v1/users` | Simple, visible, easy to route | Breaks "resource identity" principle |
| **Header versioning** | `Accept: application/vnd.api+json;version=2` | Clean URLs | Harder to test/document |
| **Query param** | `/users?version=2` | Easy to add | Pollutes query space |
| **Content negotiation** | `Accept: application/vnd.company.user-v2+json` | Most RESTful | Most complex |

> **Practitioner Note:** URL versioning wins in practice — it's the most debuggable and the easiest to document. `/api/v2/users` is immediately obvious from logs, browser history, and curl commands. Use it. Avoid query param versioning (it gets entangled with filtering params).

---

## 6.5 Pagination, Filtering, Sorting

```
# Offset-based pagination (simple, problematic for large sets)
GET /orders?offset=20&limit=10

# Cursor-based pagination (consistent for real-time data)
GET /orders?after=cursor_abc&limit=10
Response: { data: [...], nextCursor: "cursor_xyz" }

# Filtering
GET /orders?status=shipped&customerId=123&createdAfter=2024-01-01

# Sorting
GET /orders?sort=createdAt:desc,amount:asc

# Field selection (sparse fieldsets)
GET /users?fields=id,name,email
```

**Cursor vs Offset pagination:**

| | Offset | Cursor |
|---|---|---|
| Simple to implement | ✅ | ❌ |
| Consistent with inserts | ❌ | ✅ |
| Works for large datasets | ❌ | ✅ |
| Allows random page access | ✅ | ❌ |

---

## 6.6 API Security

### OAuth 2.0 Flow Summary

```
Client → Authorization Server: "I want access to user data"
Authorization Server → User: "Do you authorize this?"
User → Authorization Server: "Yes"
Authorization Server → Client: Authorization Code
Client → Authorization Server: Code + Client Secret
Authorization Server → Client: Access Token + Refresh Token
Client → Resource Server: Request + Access Token
Resource Server → Client: Protected Resource
```

**Grant types:**

- **Authorization Code:** Web apps, most secure
- **Client Credentials:** Machine-to-machine, no user involved
- **Device Code:** TVs, CLIs, limited-input devices
- **PKCE (Proof Key for Code Exchange):** Mobile/SPA — no client secret

### JWT Structure

```
header.payload.signature

Header:  { "alg": "RS256", "typ": "JWT" }
Payload: { "sub": "user123", "roles": ["admin"], "exp": 1700000000 }
Signature: HMACSHA256(base64(header) + "." + base64(payload), secret)
```

**JWT best practices:**

- Short expiry (15 minutes for access tokens)
- Use refresh tokens stored in httpOnly cookies
- Always validate `exp`, `iss`, and `aud` claims
- Never store sensitive data in payload (it's base64-encoded, not encrypted)
- Use RS256 (asymmetric) in production — allows public key verification without sharing secrets

---

## 6.7 REST vs GraphQL vs gRPC

| Dimension | REST | GraphQL | gRPC |
|---|---|---|---|
| **Protocol** | HTTP/1.1 | HTTP/1.1 | HTTP/2 |
| **Data format** | JSON/XML | JSON | Protobuf (binary) |
| **Type safety** | Optional (OpenAPI) | Built-in schema | Strict (proto files) |
| **Overfetching** | Common | Eliminated | N/A |
| **Underfetching** | Common (N+1) | Eliminated | N/A |
| **Caching** | HTTP native | Complex | Complex |
| **Streaming** | Limited | Subscriptions | Native |
| **Browser support** | Native | Native | Needs grpc-web |
| **Best for** | Public APIs, CRUD | Complex client queries | Internal service comms |

> **Practitioner Note:** REST for public APIs, gRPC for internal microservices, GraphQL for data-rich frontends with complex query needs. Don't use GraphQL just because it's modern — it shifts complexity from the server to schema design and resolver management.

---

## 6.8 API Gateway Patterns

An API Gateway is a single entry point for all clients. It handles:

- **Routing:** Route requests to appropriate backend services
- **Authentication:** Validate tokens before they reach services
- **Rate limiting:** Protect backends from overload
- **Request transformation:** Modify headers, shape payloads
- **SSL termination:** Handle HTTPS centrally
- **Logging/monitoring:** Centralized observability

```
Clients → API Gateway → Service A
                     → Service B
                     → Service C
```

**Products:** AWS API Gateway, Kong, NGINX, Traefik, Envoy, Azure API Management

---

## 6.9 OpenAPI / Swagger Best Practices

```yaml
openapi: 3.0.3
info:
  title: Order API
  version: 2.0.0
paths:
  /orders:
    post:
      summary: Create an order
      operationId: createOrder
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateOrderRequest'
      responses:
        '201':
          description: Order created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
```

**Best practices:**

- Define all error responses (400, 401, 403, 404, 422, 500)
- Use `operationId` for code generation
- Use `$ref` to reuse schemas (DRY)
- Use examples to improve DX
- Generate documentation, mock servers, and client SDKs from the spec
- Treat the spec as the contract — validate against it in CI

# 7. UI/UX Architecture Patterns

## 7.1 Component-Based Architecture

Modern frontends are built as trees of reusable, composable **components**. Each component manages its own UI and, optionally, its own local state.

```
App
├── Header
│   ├── Logo
│   ├── Navigation
│   └── UserMenu
├── MainContent
│   ├── Sidebar
│   └── ProductList
│       ├── ProductCard (×N)
│       └── Pagination
└── Footer
```

**Component design principles:**

- **Single Responsibility:** Each component does one thing
- **Props down, events up:** Parent passes data down via props; child emits events up
- **Composability:** Small components combine into larger ones
- **Separation of container/presentational components:**

```typescript
// Presentational (dumb): only renders, no business logic
const UserCard = ({ name, email, avatarUrl }: UserCardProps) => (
  <div className="card">
    <img src={avatarUrl} alt={name} />
    <h3>{name}</h3>
    <p>{email}</p>
  </div>
);

// Container (smart): fetches data, passes to presentational
const UserCardContainer = ({ userId }: { userId: string }) => {
  const { data: user, isLoading } = useUser(userId);
  if (isLoading) return <Skeleton />;
  return <UserCard {...user} />;
};
```

---

## 7.2 Micro-Frontends

**Definition:** Extend microservices thinking to the frontend. Multiple independent frontend apps composed into a single user experience.

```
┌─────────────────────────────────────────────────────┐
│                  App Shell (host)                    │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────┐  │
│  │  Catalog MFE │  │  Cart MFE    │  │ Account   │  │
│  │  (Team A)    │  │  (Team B)    │  │ MFE       │  │
│  │              │  │              │  │ (Team C)  │  │
│  └──────────────┘  └──────────────┘  └───────────┘  │
└─────────────────────────────────────────────────────┘
```

**Integration approaches:**

- **Build-time:** NPM packages (tight coupling, shared build)
- **Run-time via iframes:** Strong isolation, poor UX
- **Run-time via Module Federation** (Webpack 5): Independent deploys, shared dependencies
- **Run-time via Web Components:** Framework-agnostic

**When micro-frontends make sense:**

- Multiple independent teams owning different parts of the UI
- Different release cadences per section
- Significant differences in technical requirements per section

**When they don't:**

- Small team, single codebase
- Consistency requirements are high (shared design system is hard across MFEs)
- Performance is critical (loading multiple bundles adds overhead)

> **Practitioner Note:** Micro-frontends solve team topology and deployment problems, not technical ones. If you don't have Conway's Law forcing you toward them (multiple autonomous teams with independent deployment needs), a well-structured modular monorepo (NX, Turborepo) will serve you better.

---

## 7.3 State Management Patterns

| Pattern | Library | Philosophy | Best For |
|---|---|---|---|
| **Flux** | Redux | Unidirectional data flow, single store | Large apps, complex state |
| **Atomic State** | Jotai, Recoil | Atom-based, derived state | Fine-grained reactivity |
| **Proxy State** | MobX, Valtio | Observable objects, auto-tracking | Object-heavy domains |
| **Signals** | SolidJS, Angular 17+ | Fine-grained reactive primitives | Performance-critical UI |
| **Server State** | TanStack Query, SWR | Separate server/client state | Data-fetching-heavy apps |

**The most common mistake:** Treating all state as global application state. Not everything belongs in Redux:

```
State categories:
  UI State         → Local component state (useState)
  Form State       → Local or React Hook Form
  Server State     → TanStack Query (cache, sync, refresh)
  App/Global State → Zustand/Redux (user session, feature flags)
  URL State        → Router params, query strings
```

---

## 7.4 Rendering Strategies

| Strategy | Description | Best For |
|---|---|---|
| **CSR** (Client-Side Rendering) | Full app rendered in browser | Highly interactive SPAs, dashboards |
| **SSR** (Server-Side Rendering) | HTML generated per request on server | SEO-critical, personalized content |
| **SSG** (Static Site Generation) | HTML generated at build time | Blogs, marketing sites, docs |
| **ISR** (Incremental Static Regen) | SSG with on-demand revalidation | E-commerce, content-heavy sites |
| **Streaming SSR** | HTML streamed progressively | Large pages with slow data |

```
                High Interactivity
                      ↑
                      │
          CSR ────────┼──────── Hybrid (Next.js, Nuxt)
                      │
SSG ──────────────────┼──────── SSR
                      │
                      ↓
                Low Interactivity
                SEO-critical ←────────────────→ Not SEO-critical
```

---

## 7.5 Performance Patterns

**Code Splitting:** Don't load what the user doesn't need yet.

```typescript
// Lazy load routes
const Dashboard = lazy(() => import('./Dashboard'));
const Settings = lazy(() => import('./Settings'));
```

**Virtualization:** Only render what's visible (for long lists).

```typescript
// React Virtual (or react-window)
// Only renders ~20 DOM nodes regardless of 10,000 items in list
```

**Image optimization:**

- Serve correct size per device (srcset)
- Use modern formats (WebP, AVIF)
- Lazy load below-fold images
- Use CDN with image transformation

---

## 7.6 Accessibility as Architecture

Accessibility (a11y) fails when it's treated as a late-stage checklist. It should be an architectural concern from the start:

- **Design tokens** that enforce sufficient color contrast ratios
- **Semantic HTML** by default — `<button>` not `<div onClick>`, `<nav>` not `<div class="nav">`
- **Focus management** as a first-class routing concern (SPA route changes must move focus)
- **ARIA roles and live regions** for dynamic content updates
- **Keyboard navigation** tested in CI (axe-core, Playwright a11y checks)

---

# 8. Data Modeling and Domain Patterns

## 8.1 Domain-Driven Design (DDD)

**DDD** is a software development approach introduced by Eric Evans in his 2003 book *Domain-Driven Design*. The central idea: software should model the business domain, and the language used by developers should match the language used by domain experts.

DDD has two halves: **Strategic Design** (how to divide the problem) and **Tactical Design** (how to implement within a division).

---

### Strategic Design

#### Bounded Context

A **Bounded Context** is an explicit boundary within which a particular domain model applies. The same term can mean different things in different bounded contexts — and that's fine.

```
┌─────────────────┐        ┌─────────────────┐        ┌─────────────────┐
│   Sales Context  │        │ Shipping Context │        │ Support Context  │
│                  │        │                  │        │                  │
│ Customer:        │        │ Customer:        │        │ Customer:        │
│  - creditLimit   │        │  - shippingAddr  │        │  - ticketHistory │
│  - salesRegion   │        │  - preferredCarr │        │  - agentAssigned │
└─────────────────┘        └─────────────────┘        └─────────────────┘
   "Customer" means something different in each context
```

**Key insight:** Don't try to build a single unified model for your entire organization. The complexity of reconciling every team's view of "Customer" or "Order" leads to an anemic, least-common-denominator model that serves no one well.

#### Ubiquitous Language

A shared vocabulary between developers and domain experts, used consistently in code, documentation, and conversation within a Bounded Context.

```typescript
// Bad: technical language, no domain signal
class DataProcessor {
  processRecord(data: any) { }
}

// Good: ubiquitous language
class ClaimsAdjudicator {
  adjudicateClaim(claim: InsuranceClaim): AdjudicationResult { }
}
```

#### Context Map

A diagram showing the relationships between Bounded Contexts. Relationship types:

| Pattern | Description |
|---|---|
| **Shared Kernel** | Two contexts share a subset of the domain model |
| **Customer-Supplier** | Upstream context defines the API; downstream consumes it |
| **Conformist** | Downstream conforms to upstream model (no negotiation) |
| **Anti-Corruption Layer** | Downstream translates upstream model to protect its own |
| **Open Host Service** | Upstream publishes a formal protocol for multiple consumers |
| **Published Language** | Shared, well-defined interchange language (events schema) |
| **Separate Ways** | No integration — contexts go their own way |

---

### Tactical Design

#### Entities

An **Entity** is an object defined by its **identity**, not its attributes. Two users with the same name and email are different if they have different IDs.

```typescript
class Order {
  constructor(
    public readonly id: OrderId,  // Identity
    private status: OrderStatus,
    private items: OrderItem[]
  ) {}

  addItem(item: OrderItem): void {
    if (this.status !== OrderStatus.DRAFT) throw new Error('Cannot modify a placed order');
    this.items.push(item);
  }
}
```

#### Value Objects

A **Value Object** is defined by its **attributes**, has no identity, and is immutable. Two money objects with the same amount and currency are equal.

```typescript
class Money {
  constructor(
    public readonly amount: number,
    public readonly currency: string
  ) {
    if (amount < 0) throw new Error('Amount cannot be negative');
  }

  add(other: Money): Money {
    if (this.currency !== other.currency) throw new Error('Currency mismatch');
    return new Money(this.amount + other.amount, this.currency); // Returns NEW instance
  }

  equals(other: Money): boolean {
    return this.amount === other.amount && this.currency === other.currency;
  }
}
```

#### Aggregates

An **Aggregate** is a cluster of entities and value objects treated as a single unit for data changes. An **Aggregate Root** is the entry point — external objects can only hold references to the root, never to internal entities.

```
Order (Aggregate Root)
  ├── OrderItem (Entity, only accessed through Order)
  ├── ShippingAddress (Value Object)
  └── Payment (Entity, only accessed through Order)
```

```typescript
class Order { // Aggregate Root
  private items: OrderItem[] = [];

  addItem(productId: ProductId, quantity: number, price: Money): void {
    // All invariants enforced here — no direct access to items from outside
    if (this.items.length >= 50) throw new Error('Order too large');
    const existing = this.items.find(i => i.productId.equals(productId));
    if (existing) {
      existing.increaseQuantity(quantity);
    } else {
      this.items.push(new OrderItem(productId, quantity, price));
    }
    // Enforce aggregate invariant
    this.ensureTotalDoesNotExceedCreditLimit();
  }
}
```

**Aggregate design rules:**

- Modify only one aggregate per transaction
- Reference other aggregates by ID only (not object reference)
- Aggregates publish **Domain Events** to communicate changes to other aggregates

#### Domain Events

A **Domain Event** represents something meaningful that happened in the domain. It's a record of fact, named in past tense.

```typescript
class OrderPlaced {
  constructor(
    public readonly orderId: OrderId,
    public readonly customerId: CustomerId,
    public readonly total: Money,
    public readonly occurredAt: Date
  ) {}
}

// Order aggregate publishes event when placed
class Order {
  private events: DomainEvent[] = [];

  place(): void {
    if (this.items.length === 0) throw new Error('Cannot place empty order');
    this.status = OrderStatus.PLACED;
    this.events.push(new OrderPlaced(this.id, this.customerId, this.total(), new Date()));
  }

  pullEvents(): DomainEvent[] {
    const events = [...this.events];
    this.events = [];
    return events;
  }
}
```

#### Repositories

A **Repository** provides collection-like access to aggregates, abstracting the persistence mechanism. Only aggregate roots have repositories.

```typescript
interface OrderRepository {
  findById(id: OrderId): Promise<Order | null>;
  findByCustomer(customerId: CustomerId): Promise<Order[]>;
  save(order: Order): Promise<void>;
  remove(order: Order): Promise<void>;
}

// Infrastructure layer implements the port
class PostgresOrderRepository implements OrderRepository {
  async findById(id: OrderId): Promise<Order | null> {
    const row = await this.db.query('SELECT * FROM orders WHERE id = $1', [id.value]);
    if (!row) return null;
    return this.mapper.toDomain(row);
  }
  // ...
}
```

---

### DDD and Microservices

**Bounded Contexts map naturally to microservices.** Each service owns one bounded context, its own database, and its own model. Services communicate through Domain Events (published language).

```
Order Bounded Context → Order Service
Inventory Context     → Inventory Service
Billing Context       → Billing Service
Notification Context  → Notification Service
```

> **Practitioner Note:** DDD's strategic design (Bounded Contexts, Context Maps) is valuable at the architecture level even if you don't use the tactical patterns. Understanding where your domain boundaries lie is the most important step before designing service boundaries. Get this wrong, and every microservice call becomes a distributed join.

---

## 8.2 Test-Driven Development (TDD)

**TDD** is a development practice where tests are written *before* the code that makes them pass. This inverts the traditional cycle and uses tests to drive design.

### The Red-Green-Refactor Cycle

```
    ┌─────────┐
    │   RED   │ ← Write a failing test for the next small piece of behavior
    └────┬────┘
         │
         ▼
    ┌─────────┐
    │  GREEN  │ ← Write the minimum code to make the test pass
    └────┬────┘
         │
         ▼
    ┌─────────┐
    │REFACTOR │ ← Clean up the code without breaking tests
    └────┬────┘
         │
         └──────────────────────→ Repeat
```

```typescript
// Step 1: RED — write test first
describe('Money', () => {
  it('should add two money values of the same currency', () => {
    const a = new Money(10, 'USD');
    const b = new Money(5, 'USD');
    expect(a.add(b)).toEqual(new Money(15, 'USD'));
  });

  it('should throw when adding different currencies', () => {
    const a = new Money(10, 'USD');
    const b = new Money(5, 'EUR');
    expect(() => a.add(b)).toThrow('Currency mismatch');
  });
});

// Step 2: GREEN — implement just enough
class Money {
  constructor(public readonly amount: number, public readonly currency: string) {}
  add(other: Money): Money {
    if (this.currency !== other.currency) throw new Error('Currency mismatch');
    return new Money(this.amount + other.amount, this.currency);
  }
}

// Step 3: REFACTOR — improve code quality
// (add input validation, consider immutability enforcement, etc.)
```

### TDD as a Design Tool

TDD's real value is not test coverage — it's **design feedback**. Hard-to-test code is usually hard-to-use code. If you can't write a test for a function without setting up half the application, that function has too many dependencies.

**TDD signals:**

- Test setup is 30 lines → class has too many dependencies (SRP violation)
- You can't test without a real database → business logic is entangled with infrastructure (DIP violation)
- Tests are brittle (break on every refactor) → testing implementation, not behavior

### Test Pyramid

```
        ▲
       /E2E\         Few — slow, expensive, brittle
      /──────\
     /Integration\   Some — real DB, real HTTP
    /──────────────\
   /  Unit Tests   \  Many — fast, isolated, cheap
  /__________________\
```

**Unit tests:** Test a single class/function in isolation. Fast (milliseconds). Mocked dependencies.

**Integration tests:** Test multiple components together. May use a real database. Slower (seconds).

**E2E tests:** Test the full system from the user's perspective. Slowest (minutes). Use sparingly for critical paths.

### Contract Testing

In microservices, **contract tests** verify that a consumer's assumptions about a provider's API are honored — without calling the real API.

**Pact** is the most common tool: consumers define contracts (expected request/response), providers verify they honor them. This prevents integration surprises without full E2E test suites.

### BDD (Behavior-Driven Development)

BDD extends TDD with a shared language between business and engineers. Tests are written in **Gherkin** syntax.

```gherkin
Feature: Placing an order
  Scenario: Successfully placing a non-empty order
    Given a customer has items in their cart
    When they confirm the order
    Then the order status should be PLACED
    And a confirmation email should be sent

  Scenario: Attempting to place an empty order
    Given a customer has no items in their cart
    When they try to confirm the order
    Then they should see an error "Cart is empty"
```

BDD bridges the gap between stakeholders and developers by making test scenarios readable by non-engineers.

---

## 8.3 Data Modeling Fundamentals

### Data Model Types

| Model | Best For | Examples |
|---|---|---|
| **Relational** | Structured data with relationships and ACID | PostgreSQL, MySQL, Oracle |
| **Document** | Flexible schema, hierarchical data | MongoDB, Firestore, DynamoDB |
| **Graph** | Highly connected data, relationship queries | Neo4j, Amazon Neptune |
| **Key-Value** | Simple lookups, caching, session state | Redis, DynamoDB, etcd |
| **Time-Series** | Metrics, sensor data, IoT | InfluxDB, TimescaleDB, Prometheus |
| **Column-Family** | Large-scale analytics, write-heavy | Cassandra, HBase |
| **Search** | Full-text search, faceting | Elasticsearch, OpenSearch |

### Normalization vs Denormalization

**Normalization** (3NF): Eliminate data redundancy. Each piece of data exists in one place.

```sql
-- Normalized: 3 tables, join to get full data
users: (id, name, email)
orders: (id, user_id, created_at)
order_items: (id, order_id, product_id, quantity, price)
```

**Denormalization:** Duplicate data to optimize read performance.

```json
// Denormalized document (MongoDB): everything in one place
{
  "orderId": "123",
  "customerName": "Jane Smith",       // duplicated from users
  "customerEmail": "jane@example.com", // duplicated from users
  "items": [
    { "productName": "Widget", "quantity": 2, "price": 9.99 }
  ]
}
```

**When to normalize:** OLTP systems, data that changes frequently, when consistency is critical.
**When to denormalize:** Read-heavy systems, reporting, when query performance matters more than write consistency, document databases.

### Polyglot Persistence

Use the right database for the right job within the same system.

```
User Profiles      → PostgreSQL (relational, ACID)
Product Catalog    → Elasticsearch (search, faceting)
Session Data       → Redis (key-value, TTL)
User Activity Feed → Cassandra (time-series, write-heavy)
Product Graph      → Neo4j (relationship queries)
```

**Tradeoff:** Operational complexity increases (multiple databases to manage, monitor, backup). Justified when each database genuinely solves a problem the others can't.

---

## 8.4 Observability and Data Flow

### The Three Pillars

**Logs** — Discrete events with context.

```json
{
  "level": "error",
  "timestamp": "2024-01-15T10:23:45Z",
  "service": "order-service",
  "traceId": "abc123",
  "orderId": "456",
  "message": "Payment failed: insufficient funds",
  "userId": "789"
}
```

Structured logging (JSON) >> unstructured logs for observability. Always include `traceId` for correlation.

**Metrics** — Aggregated numerical measurements over time.

```
# Prometheus metric types
Counter:   order_created_total (only goes up)
Gauge:     active_connections (can go up or down)
Histogram: http_request_duration_seconds (distribution)
Summary:   p50, p95, p99 latencies
```

**Traces** — Distributed request flow across services. See §4.3.

**Golden Signals (Google SRE):**

- **Latency:** How long does a request take?
- **Traffic:** How many requests per second?
- **Errors:** What's the error rate?
- **Saturation:** How full is the system (CPU, memory, queue depth)?

---

# 9. Cross-Cutting Concerns

## 9.1 Authentication vs Authorization

**Authentication (AuthN):** Verifying *who* you are. "Are you really Jane?"
**Authorization (AuthZ):** Determining *what* you can do. "Is Jane allowed to delete this order?"

```
Request → [AuthN: validate JWT] → [AuthZ: check permissions] → Service
                ↓ fail                    ↓ fail
              401 Unauthorized          403 Forbidden
```

**Never return 403 before 401.** If the user isn't authenticated, return 401 first.

**RBAC vs ABAC:**

| | RBAC (Role-Based) | ABAC (Attribute-Based) |
|---|---|---|
| Model | User → Role → Permissions | Policy = Subject + Object + Action + Environment |
| Complexity | Simple | Complex |
| Flexibility | Limited | High |
| Example | `admin`, `editor`, `viewer` | `user.dept == resource.dept AND time < 5pm` |

---

## 9.2 Secrets Management

**Never store secrets in code, config files, or environment variables committed to version control.**

```
❌ BAD:
  DB_PASSWORD=supersecret123 in .env committed to Git

✅ GOOD:
  - AWS Secrets Manager / Parameter Store
  - HashiCorp Vault
  - Azure Key Vault
  - GCP Secret Manager
  - Kubernetes Secrets (encrypted at rest, external secrets operator)
```

**Secret rotation:** Secrets should be rotatable without downtime. Applications should read secrets on startup or dynamically via sidecar injection (Vault agent).

**In GitHub Actions:**

```yaml
# Stored in GitHub Secrets, referenced via environment
env:
  DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
```

---

## 9.3 Configuration Management

Separate configuration by environment, but keep the structure consistent:

```
config/
  default.yaml      # Shared across all environments
  development.yaml  # Dev overrides
  staging.yaml      # Staging overrides
  production.yaml   # Prod overrides (sensitive values from secrets manager)
```

**Feature flags as configuration:** Feature flags let you deploy code without activating features — enabling trunk-based development, A/B testing, and gradual rollouts without branching.

**Tools:** LaunchDarkly, Unleash, AWS AppConfig, Flagsmith.

```typescript
// Feature-flagged code path
const useNewCheckout = featureFlags.isEnabled('new-checkout-flow', { userId });

if (useNewCheckout) {
  return renderNewCheckout();
} else {
  return renderLegacyCheckout();
}
```

> **Practitioner Note:** Feature flags are an architectural tool, not just a product management tool. They decouple **deployment** from **release** — critical for teams practicing continuous delivery. Combined with CI/CD, they enable: deploy daily to production, release when business is ready.

---

## 9.4 API Versioning as Governance

API versioning is not just a technical concern — it's a **governance** mechanism. It defines the contract between teams, between internal services, and between your platform and external consumers.

**Platform engineering framing:** When building internal developer platforms, treat your API and your infrastructure interfaces (Terraform modules, GitHub Actions workflows) the same way you treat external APIs:

- Version them explicitly
- Maintain a deprecation policy (e.g., 6 months notice before removal)
- Communicate breaking changes via changelogs
- Test backward compatibility in CI

---

# 10. Patterns Reference Cheat Sheet

## Architecture Styles Quick Reference

| Style | When to Use | Key Tradeoff |
|---|---|---|
| Monolith | Small team, early stage, unknown domain | Simple to start; hard to scale independently |
| Modular Monolith | Growing team, domain understood, not ready for services | Best of both worlds; requires discipline |
| Three-Layer | Almost always (starting point) | Clear, familiar; doesn't fix dependency direction |
| Clean/Hexagonal | Complex domain logic, testability critical | Steep learning curve; overhead for simple CRUD |
| Microservices | Large org, independent scaling, independent deployment | Distributed systems complexity in exchange for autonomy |
| Event-Driven | Async workflows, loose coupling needed | Hard to trace; eventual consistency |
| Serverless | Event-driven, spiky traffic, low ops overhead desired | Vendor lock-in, cold starts |
| BFF | Multiple clients with different needs | N BFFs to maintain |
| CQRS | High read-to-write ratio, complex queries | Eventual consistency between read/write models |
| Event Sourcing | Audit required, time-travel debugging | Complex; event versioning is hard |

## GoF Patterns Quick Reference

| Category | Pattern | One-Line Purpose |
|---|---|---|
| Creational | Singleton | One instance globally |
| Creational | Factory Method | Subclass decides what to create |
| Creational | Abstract Factory | Families of related objects |
| Creational | Builder | Step-by-step complex object construction |
| Creational | Prototype | Clone existing objects |
| Structural | Adapter | Convert incompatible interfaces |
| Structural | Decorator | Add behavior dynamically |
| Structural | Facade | Simplified interface to complex subsystem |
| Structural | Proxy | Control access to another object |
| Structural | Composite | Tree structure, treat parts and whole uniformly |
| Structural | Bridge | Separate abstraction from implementation |
| Structural | Flyweight | Share many fine-grained objects efficiently |
| Behavioral | Strategy | Interchangeable algorithms |
| Behavioral | Observer | Notify dependents of state changes |
| Behavioral | Command | Encapsulate requests as objects |
| Behavioral | Chain of Resp. | Pass request through chain of handlers |
| Behavioral | Template Method | Define algorithm skeleton, defer steps |
| Behavioral | State | Change behavior when state changes |
| Behavioral | Mediator | Central coordinator for object interactions |
| Behavioral | Iterator | Sequential access without exposing internals |
| Behavioral | Visitor | New operations without modifying classes |
| Behavioral | Memento | Capture and restore object state |
| Behavioral | Interpreter | Grammar and interpreter for a language |

## Cloud Resilience Patterns Quick Reference

| Pattern | Purpose | Key Tradeoff |
|---|---|---|
| Retry | Handle transient failures | Risk of thundering herd; add jitter |
| Circuit Breaker | Stop cascading failures | Complexity; needs careful threshold tuning |
| Bulkhead | Isolate failure domains | Resource isolation overhead |
| Timeout | Don't wait forever | Too short = false failures; too long = wasted resources |
| Fallback | Degrade gracefully | Stale/partial data served |
| Sidecar | Separate cross-cutting concerns from app | Extra container per pod overhead |
| Strangler Fig | Gradual legacy migration | Routing complexity during transition |
| ACL | Protect domain from external model | Additional translation layer |

## Data Modeling Decision Framework

```
Do you need ACID transactions?
  Yes → Relational DB (PostgreSQL, MySQL)
  No  → What's your primary access pattern?
          ├─ Key-based lookups     → Key-Value (Redis, DynamoDB)
          ├─ Flexible schema       → Document (MongoDB, Firestore)
          ├─ Time-ordered data     → Time-Series (InfluxDB, TimescaleDB)
          ├─ Connected data/graphs → Graph (Neo4j)
          ├─ Full-text search      → Search (Elasticsearch)
          └─ Analytics/OLAP       → Column-store (Redshift, BigQuery)
```

## API Design Decision Framework

```
Public-facing API?
  Yes → REST (familiar, cacheable, OpenAPI)

Internal service-to-service?
  High performance / binary efficiency needed → gRPC (Protobuf)
  Standard HTTP/JSON is fine → REST

Frontend with complex/varying query needs?
  Many clients with different data shapes → GraphQL
  Simpler needs → REST + BFF

Real-time requirements?
  Yes → WebSocket, SSE, or gRPC streaming
```

---

## Cross-References Index

| Topic | Related Sections |
|---|---|
| DDD Bounded Contexts | §3.6 (Microservices), §3.5 (EDA), §4.1 (Saga) |
| Clean Architecture | §1.5 (DIP), §8.1 (DDD Repositories) |
| Event-Driven Architecture | §3.10 (CQRS+ES), §4.1 (Saga), §8.1 (Domain Events) |
| Circuit Breaker | §4.2 (Service Discovery), §5.2 (Resilience) |
| BFF Pattern | §1.4 (ISP), §2.1 GoF Facade, §7.2 (Micro-frontends) |
| Saga Pattern | §3.10 (CQRS), §3.6 (Microservices), §4.1 |
| Feature Flags | §9.3 (Config Management), §2.1 Strategy pattern |
| Anti-Corruption Layer | §2.2 GoF Adapter, §5.4 (Migration patterns), §8.1 (DDD Context Map) |
| Observability | §4.3 (Distributed Tracing), §8.4, §3.5 (EDA pitfalls) |
| Test Pyramid | §8.2 (TDD), §6.7 (API contract testing) |

---

*This document is a living reference. Architecture patterns are not prescriptions — they're solutions to known problems in known contexts. Always understand the problem before reaching for the pattern.*
