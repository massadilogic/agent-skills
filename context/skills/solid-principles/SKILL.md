---
name: solid-principles
description: >
  SOLID principles for software design. Trigger: When designing classes, interfaces, or architectural patterns.
license: Apache-2.0
metadata:
  author: author-name
  version: "1.0"
  scope: [root, ui, api]
  auto_invoke:
    - "Applying SOLID principles"
    - "Designing class architecture"
    - "Refactoring for better design"
    - "Creating interfaces"
allowed-tools: Read, Edit, Write, Glob, Grep, Bash, WebFetch, WebSearch, Task
---

## When to Use

Apply SOLID principles when:
- Designing new classes or interfaces
- Refactoring existing code for better maintainability
- Creating architectural patterns
- Evaluating code quality and design decisions
- Implementing dependency injection

## Critical Patterns

### Single Responsibility Principle (SRP)
- **Each class should have one reason to change**
- One class = one responsibility = one change trigger
- Avoid "God classes" that do everything

**❌ Anti-pattern:**
```typescript
class UserManager {
  createUser(data) { /* ... */ }
  sendEmail(user) { /* ... */ }
  logActivity(action) { /* ... */ }
  generateReport(user) { /* ... */ }
}
```

**✅ Correct:**
```typescript
class UserService { createUser(data) { /* ... */ } }
class EmailService { sendEmail(user) { /* ... */ } }
class ActivityLogger { log(action) { /* ... */ } }
class ReportGenerator { generate(user) { /* ... */ } }
```

### Open/Closed Principle (OCP)
- **Open for extension, closed for modification**
- Use interfaces, abstract classes, composition
- Strategy pattern is your friend

**✅ Use Strategy Pattern:**
```typescript
interface PaymentProcessor { process(amount: number): void }

class StripeProcessor implements PaymentProcessor {
  process(amount) { /* Stripe logic */ }
}

class PayPalProcessor implements PaymentProcessor {
  process(amount) { /* PayPal logic */ }
}

class OrderService {
  constructor(private processor: PaymentProcessor) {}
  
  checkout(amount) {
    this.processor.process(amount);
  }
}
```

### Liskov Substitution Principle (LSP)
- **Subtypes must be substitutable for base types**
- Child classes shouldn't break parent class expectations
- Avoid overriding with different behavior

**❌ Violation:**
```typescript
class Bird { fly() { console.log("Flying"); } }
class Penguin extends Bird { 
  fly() { throw new Error("Penguins can't fly!"); }
}
```

**✅ Correct:**
```typescript
class Bird { }
class FlyingBird extends Bird { fly() { console.log("Flying"); } }
class Penguin extends Bird { swim() { console.log("Swimming"); } }
```

### Interface Segregation Principle (ISP)
- **Clients shouldn't depend on unused interfaces**
- Split large interfaces into smaller, specific ones
- Prefer role-based interfaces

**❌ Fat Interface:**
```typescript
interface Worker {
  work(): void;
  eat(): void;
  sleep(): void;
}
```

**✅ Segregated:**
```typescript
interface Workable { work(): void; }
interface Eatable { eat(): void; }
interface Sleepable { sleep(): void; }
```

### Dependency Inversion Principle (DIP)
- **Depend on abstractions, not concretions**
- High-level modules shouldn't depend on low-level modules
- Use dependency injection

**✅ Dependency Injection:**
```typescript
// Don't do this
class OrderService {
  private database = new MySQLDatabase(); // Concrete dependency
  private email = new SendGridEmail();     // Concrete dependency
}

// Do this
class OrderService {
  constructor(
    private database: Database,      // Abstract dependency
    private email: EmailService      // Abstract dependency
  ) {}
}
```

## Code Examples

### Repository Pattern (DIP + SRP)
```typescript
interface Repository<T> {
  findById(id: string): Promise<T>;
  save(entity: T): Promise<void>;
  delete(id: string): Promise<void>;
}

class UserRepository implements Repository<User> {
  constructor(private db: Database) {}
  
  async findById(id: string): Promise<User> {
    return this.db.query('SELECT * FROM users WHERE id = ?', [id]);
  }
  
  async save(user: User): Promise<void> {
    await this.db.execute('INSERT INTO users SET ?', user);
  }
  
  async delete(id: string): Promise<void> {
    await this.db.execute('DELETE FROM users WHERE id = ?', [id]);
  }
}
```

### Factory Pattern (OCP)
```typescript
interface NotificationService {
  send(message: string, recipient: string): void;
}

class EmailNotification implements NotificationService {
  send(message: string, recipient: string) {
    // Email sending logic
  }
}

class SMSNotification implements NotificationService {
  send(message: string, recipient: string) {
    // SMS sending logic
  }
}

class NotificationFactory {
  static create(type: 'email' | 'sms'): NotificationService {
    switch (type) {
      case 'email': return new EmailNotification();
      case 'sms': return new SMSNotification();
      default: throw new Error(`Unknown notification type: ${type}`);
    }
  }
}
```

## Commands

```bash
# Check for code smells related to SOLID violations
grep -r "class.*{" src/ | grep -E "(Manager|Handler|Processor)" | head -10

# Find large interfaces
grep -r "interface.*{" src/ | awk '{print $2}' | xargs -I {} grep -c "{}" src/* | sort -nr

# Identify dependency injection opportunities
grep -r "new.*(" src/ | grep -v "test" | head -10
```

## Resources

- **Templates**: See [assets/](assets/) for SOLID design patterns
- **Documentation**: See [references/](references/) for architecture guides

---

## Quick Reference

| Principle | Key Idea | Common Pattern |
|-----------|----------|----------------|
| SRP | One responsibility per class | Service classes, Utils |
| OCP | Extend without modifying | Strategy, Factory, Observer |
| LSP | Substitutable subtypes | Proper inheritance |
| ISP | Small, focused interfaces | Role-based interfaces |
| DIP | Depend on abstractions | Dependency Injection |
