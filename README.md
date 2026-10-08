# Apex-programming

We should build your Apex knowledge from **language fundamentals → Salesforce execution model → database → automation → asynchronous Apex → integrations → security → testing → architecture → advanced patterns**.

## 🚀 Apex Deep-Dive Roadmap

### Phase 1 — Apex & Salesforce Foundations
1. **What is Apex?**
2. Apex vs Java — similarities and differences
3. Apex execution environment
4. Governor limits
5. Multitenancy
6. Apex syntax
7. Variables and data types
8. Operators
9. Conditional statements
10. Loops
11. Methods
12. Parameters and return types
13. Collections
   - List
   - Set
   - Map
14. String handling
15. Date, Datetime, Time
16. Enum
17. Constants
18. Exception handling

---

# Phase 2 — Object-Oriented Apex

This is extremely important because almost every serious Apex implementation uses OOP.

### 1. Classes and Objects
- Class declaration
- Instance variables
- Static variables
- Constructors
- Methods
- Access modifiers
- `this`
- `static`

### 2. Encapsulation
- `private`
- `public`
- `protected`
- `global`
- Getter/setter
- Properties

### 3. Inheritance
```apex
public class Animal {
    public void speak() {
        System.debug('Animal speaks');
    }
}

public class Dog extends Animal {
    public void bark() {
        System.debug('Dog barks');
    }
}
```

### 4. Polymorphism
- Method overriding
- Method overloading
- Runtime polymorphism

### 5. Abstraction
- Abstract classes
- Interfaces

### 6. Advanced OOP
- Virtual classes
- Final variables
- Inner classes
- Dependency injection
- Composition vs inheritance

---

# Phase 3 — Salesforce Data Model

Here we'll connect Apex programming with actual Salesforce data.

### Understand deeply:

```text
Object
 ↓
Record
 ↓
Field
 ↓
Relationship
 ↓
SOQL
 ↓
Apex
 ↓
DML
```

Topics:

- Standard objects
- Custom objects
- Relationships
- Lookup
- Master-detail
- Parent-child relationships
- Junction objects
- Record types
- Formula fields
- Roll-up summary fields
- External IDs

---

# Phase 4 — SOQL Deep Dive

You should become **very strong in SOQL**.

### Basic SOQL

```apex
List<Account> accounts = [
    SELECT Id, Name
    FROM Account
];
```

Then progress to:

### Filtering

```apex
SELECT Id, Name
FROM Account
WHERE Industry = 'Technology'
```

### Ordering

```apex
ORDER BY CreatedDate DESC
```

### Limiting

```apex
LIMIT 10
```

### Relationships

Parent → Child:

```apex
SELECT Id, Name,
       (SELECT Id, Name FROM Contacts)
FROM Account
```

Child → Parent:

```apex
SELECT Id, Name, Account.Name
FROM Contact
```

Then:

- Aggregate queries
- `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`
- `GROUP BY`
- `HAVING`
- Semi-joins
- Anti-joins
- Dynamic SOQL
- SOQL injection
- Query optimization
- Selectivity
- Query limits

---

# Phase 5 — DML

Master:

```apex
insert
update
delete
undelete
upsert
merge
```

Then:

### Database class

```apex
Database.insert(records, false);
```

Understand the difference between:

```apex
insert records;
```

and

```apex
Database.insert(records, false);
```

including:

- `Database.SaveResult`
- Partial success
- Error handling
- External IDs
- Upsert behavior

---

# Phase 6 — Triggers 🔥

This is one of the most important parts of Apex development.

We'll go very deep into:

```text
before insert
before update
before delete

after insert
after update
after delete
after undelete
```

Understand:

- Trigger context variables
- `Trigger.new`
- `Trigger.old`
- `Trigger.newMap`
- `Trigger.oldMap`
- `Trigger.isInsert`
- `Trigger.isUpdate`
- `Trigger.isDelete`
- `Trigger.isBefore`
- `Trigger.isAfter`

Then:

### Trigger design

Bad:

```apex
trigger AccountTrigger on Account (after insert) {

    for(Account a : Trigger.new) {
        // huge logic
    }
}
```

Better:

```text
Trigger
   ↓
Handler
   ↓
Service
   ↓
Selector
   ↓
Database
```

You'll learn **Trigger Handler Pattern**, **Service Layer**, **Selector Layer**, and **Domain Layer**.

---

# Phase 7 — Salesforce Transaction & Order of Execution

This is where Apex starts becoming **advanced Salesforce development**.

You need to understand exactly what happens when:

```apex
insert account;
```

runs.

We'll study:

```text
Database operation
      ↓
Before-save flow
      ↓
Before trigger
      ↓
Validation
      ↓
Duplicate rules
      ↓
Save
      ↓
After trigger
      ↓
Assignment rules
      ↓
Auto-response
      ↓
Workflow
      ↓
Process/Flow
      ↓
Commit
```

And importantly:

### Recursion

Example:

```text
Trigger
 ↓
Update Account
 ↓
Trigger runs again
 ↓
Update Account
 ↓
Trigger runs again
```

We'll learn how to prevent this correctly.

---

# Phase 8 — Governor Limits 🔥🔥

You **must** understand governor limits deeply.

Examples:

```apex
SOQL queries
DML statements
CPU time
Heap size
Callouts
Future calls
Queueable jobs
Scheduled jobs
```

Most importantly:

### Bad

```apex
for(Account acc : accounts) {

    List<Contact> contacts = [
        SELECT Id
        FROM Contact
        WHERE AccountId = :acc.Id
    ];
}
```

### Good

```apex
Set<Id> accountIds = new Set<Id>();

for(Account acc : accounts) {
    accountIds.add(acc.Id);
}

List<Contact> contacts = [
    SELECT Id, AccountId
    FROM Contact
    WHERE AccountId IN :accountIds
];
```

We'll learn **bulkification** until you can identify these problems automatically.

---

# Phase 9 — Asynchronous Apex

Very important for real projects.

### Future

```apex
@future
public static void processData(Set<Id> ids) {
}
```

### Queueable

```apex
public class MyQueueable implements Queueable {

    public void execute(QueueableContext context) {
        
    }
}
```

### Batch

```apex
Database.executeBatch(
    new MyBatch(),
    200
);
```

### Scheduled

```apex
System.schedule(
    'My Job',
    cronExpression,
    new MyScheduler()
);
```

We'll compare:

| Type | Best use |
|---|---|
| Future | Simple async operation |
| Queueable | Complex async processing |
| Batch | Large data volumes |
| Scheduled | Time-based execution |

And learn:

- Chaining
- Limits
- Callouts
- Stateful batches
- Batch lifecycle
- Monitoring async jobs
- Flex queue

---

# Phase 10 — Apex + Salesforce Automation

You'll learn how Apex interacts with:

- Flow
- Process Builder legacy automation
- Workflow rules
- Approval processes
- Validation rules
- Platform Events
- Invocable Apex
- Apex-defined types
- Screen Flow
- Record-triggered Flow

Especially:

```apex
@InvocableMethod
```

and:

```apex
@InvocableVariable
```

---

# Phase 11 — Security 🔐

This is **essential for production Apex**.

Learn:

### Object-level security

CRUD

### Field-level security

FLS

### Record-level security

Sharing

Then:

```apex
with sharing
without sharing
inherited sharing
```

Also:

- `Schema.Describe`
- `Security.stripInaccessible()`
- User mode
- System mode
- Sharing rules
- Permission sets
- Profiles
- Restriction rules
- Apex managed sharing

You'll learn why this can be dangerous:

```apex
public without sharing class AccountService {
}
```

---

# Phase 12 — Testing Apex

This deserves an entire section.

You'll learn:

```apex
@isTest
```

and:

```apex
Test.startTest();
Test.stopTest();
```

Deeply.

Topics:

- Test classes
- Test methods
- Test data
- `@TestSetup`
- Assertions
- Bulk testing
- Trigger testing
- Async testing
- Queueable testing
- Batch testing
- Scheduled Apex testing
- Callout testing
- `HttpCalloutMock`
- Integration testing
- Test isolation
- Code coverage vs meaningful testing

Most importantly:

> **Don't learn how to get 75% coverage. Learn how to prove that your code works.**

---

# Phase 13 — Integration Apex 🌐

This will connect directly with the kind of work you've already been doing.

### HTTP Callouts

```apex
HttpRequest req = new HttpRequest();
req.setEndpoint('callout:My_Named_Credential');
req.setMethod('POST');

Http http = new Http();

HttpResponse res = http.send(req);
```

Learn:

- GET
- POST
- PUT
- PATCH
- DELETE
- Headers
- Request body
- JSON
- Deserialization
- Serialization
- Named Credentials
- External Credentials
- OAuth
- JWT
- REST APIs
- SOAP APIs
- Callouts from Queueable
- Callouts from Batch
- Callout mocks

We'll also study real-world patterns such as:

```text
Salesforce
    ↓
Queueable
    ↓
Named Credential
    ↓
External API
    ↓
Response
    ↓
Deserialize
    ↓
Update Salesforce
```

---

# Phase 14 — Advanced Apex

After the fundamentals are strong:

### Dynamic Apex

```apex
Schema.SObjectType
Schema.DescribeSObjectResult
Schema.SObjectField
```

Example:

```apex
Schema.SObjectType objType =
    Schema.getGlobalDescribe().get('Account');
```

Then:

- Dynamic SOQL
- Dynamic field access
- Dynamic object access
- Describe information
- Metadata-driven development

---

# Phase 15 — Advanced Design Patterns

This is where you'll move from **Apex developer → strong Salesforce developer**.

We'll study:

### Trigger Handler

```text
Trigger
 ↓
Handler
 ↓
Service
```

### Service Layer

Business logic.

### Selector Pattern

Database queries centralized.

### Factory Pattern

Create different implementations dynamically.

### Strategy Pattern

Different business rules.

### Unit of Work

Manage related DML operations.

### Dependency Injection

Make code testable and loosely coupled.

### Repository Pattern

Data-access abstraction.

---

# Phase 16 — Platform Events & Event-Driven Architecture

Learn:

```text
Salesforce
    ↓
Platform Event
    ↓
Subscriber
    ↓
Processing
```

Topics:

- Platform Events
- Publish behavior
- Subscribe
- Trigger on Platform Event
- Replay IDs
- Event Bus
- Change Data Capture
- Event-driven architecture

---

# Phase 17 — LWC + Apex

Since Apex is often used behind Lightning Web Components, we'll learn:

```text
LWC
 ↓
@AuraEnabled Apex
 ↓
Service
 ↓
SOQL/DML
 ↓
Database
```

Example:

```apex
@AuraEnabled(cacheable=true)
public static List<Account> getAccounts() {
    return [
        SELECT Id, Name
        FROM Account
        LIMIT 10
    ];
}
```

Then:

- Imperative Apex
- Wired Apex
- Parameters
- Wrapper classes
- DTOs
- Error handling
- Cacheable methods
- Security
- Bulk operations

---

# Phase 18 — Real Production-Level Apex

Finally, we'll learn how to build systems like:

```text
LWC
 │
 ▼
Controller
 │
 ▼
Service Layer
 │
 ▼
Domain Layer
 │
 ▼
Selector
 │
 ▼
SOQL
 │
 ▼
Database
```

and:

```text
Trigger
 │
 ▼
Trigger Handler
 │
 ▼
Service
 │
 ├── Sync processing
 │
 └── Queueable
       │
       ▼
     External API
```

We'll also cover:

- Error logging
- Custom Exceptions
- Custom Metadata
- Custom Settings
- Platform Cache
- Limits monitoring
- Transaction management
- Recursion control
- Large Data Volume
- Bulk processing
- Deployment considerations
- Production debugging
- Debug logs
- Apex Replay Debugger
- Code quality
- PMD/static analysis
- Apex best practices

---

# 🧠 How I Suggest We Learn

Since you already know **Java at an intermediate level**, I'll use Java as a bridge wherever it helps.

For example:

### Java

```java
List<String> names = new ArrayList<>();
```

### Apex

```apex
List<String> names = new List<String>();
```

Then we'll discuss **what looks similar but behaves differently**.

We'll also use actual Salesforce examples rather than only toy examples.

For every topic, I'll follow this structure:

### 1. Concept
What it is.

### 2. Why it exists
What Salesforce problem it solves.

### 3. Syntax

```apex
// syntax
```

### 4. Execution
Exactly what happens internally.

### 5. Example
Real Salesforce scenario.

### 6. Common mistakes
What developers usually get wrong.

### 7. Governor-limit implications
If applicable.

### 8. Best practices
Production-quality approach.

### 9. Java comparison
Where useful.

### 10. Interview questions
Questions you should be able to answer.

### 11. Practice problem
You implement it yourself.

---

# 🎯 Your Final Apex Skill Level

By the end, you should be able to look at something like:

```apex
trigger BookingTrigger on Booking__c (
    before insert,
    before update,
    after insert,
    after update
) {
    BookingTriggerHandler.handle(
        Trigger.new,
        Trigger.oldMap,
        Trigger.isInsert,
        Trigger.isUpdate
    );
}
```

and understand **every part of it**:

```text
Trigger
  ↓
Context variables
  ↓
Bulk processing
  ↓
Handler
  ↓
Business logic
  ↓
SOQL
  ↓
DML
  ↓
Governor limits
  ↓
Security
  ↓
Transaction
  ↓
Async processing if required
  ↓
Testing
```

And eventually you'll be able to design this architecture yourself rather than just understand existing code.
