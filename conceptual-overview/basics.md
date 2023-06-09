#### Ubiquitous Language

Ubiquitous Language is a term coined in Domain-Driven Design (DDD) by Eric Evans. It refers to a common language that's established around the business domain that the software is being built for. This language is used by all team members (including both technical and non-technical members) to bridge the gap between business and technology. The goal of a ubiquitous language is to reduce complexity by minimizing misunderstandings and miscommunications, thereby increasing the effectiveness of communication within the team. It's not merely a set of jargon - it's a living, evolving language, which should be used in all conversations, documentations, and code.

For example in shopping domain, we can have a term "product" which can mean different things to different people. It needs to be defined in the Ubiquitous Language so that everyone has a clear and consistent understanding of what it means. Including the actions that can be performed on it, such as "add to cart" or "remove from cart". For example in code:

```typescript
class Product {
  constructor(
    private readonly id: string,
    private readonly name: string,
    private readonly price: number,
    private readonly description: string,
    private readonly category: string
  ) {}

  public addToCart(): void {
    // ...
  }

  public removeFromCart(): void {
    // ...
  }
}
```

Summarized in bullet points:

- A common language established around the business domain in software development.
- Used by all team members, bridging the gap between business and technology.
- Aims to reduce complexity by minimizing misunderstandings and enhancing communication.
- It's not just jargon but a living, evolving language used in all team conversations, documentations, and code.

#### Bounded Context

Bounded Context, is a boundary within which a particular model is defined and applicable. It defines the context for a model within the software, such as a module, service, or subsystem. This context bounds or limits the applicability of a particular model to ensure its consistency and integrity, and to prevent any unwanted side effects of intermingling models. A bounded context allows different teams to work on different parts of the system without stepping on each other's toes. It's a key part of designing large-scale systems with multiple teams and can be seen as a way of managing and limiting complexity.

Summarized in bullet points:

- A boundary within which a particular model is defined and applicable in software development.
- Ensures the consistency and integrity of a model by limiting its applicability.
- Prevents unwanted side effects of intermingling models.
- Allows different teams to work on different parts of the system independently.
- Key part of designing large-scale systems and a method of managing and limiting complexity.

### Entities

In Domain-Driven Design, an Entity is a type of object that has a unique identity that remains the same even when its attributes change. Entities often represent meaningful concepts in the business domain that need to be tracked over time. For instance, in a banking application, a "Customer" could be an entity, having a unique ID and other attributes like name, address. Even if the customer changes their name or address, they're still the same customer because their unique ID remains the same.

Let's consider a simple example of an entity in a domain of an online store, representing a customer:

```typescript
class Customer {
  private _id: string;
  private _name: string;
  private _email: string;

  constructor(id: string, name: string, email: string) {
    this._id = id;
    this._name = name;
    this._email = email;
  }

  get id(): string {
    return this._id;
  }

  get name(): string {
    return this._name;
  }

  set name(newName: string) {
    this._name = newName;
  }

  get email(): string {
    return this._email;
  }

  set email(newEmail: string) {
    this._email = newEmail;
  }
}

// Creating a new customer entity
const customer = new Customer("1", "John Doe", "john.doe@example.com");

// Changing the customer's name and email
customer.name = "Johnathan Doe";
customer.email = "johnathan.doe@example.com";
```

In this example, the `Customer` class is an entity with a unique identity represented by the `_id` property. The `_name` and `_email` properties are attributes that can change over time. The class encapsulates the behavior for getting and setting these attributes.

### Value Objects

In Domain-Driven Design, a Value Object is an object that doesn't have a distinct identity. Unlike Entities, Value Objects are defined by their properties or attributes and the values those attributes hold. If the attributes are identical, then the Value Objects can be considered equal, even if they are not the same instance.

For a real-world example, consider a "Money" Value Object in an online shopping application. A Money object could have two attributes: amount and currency. For example, $5 (amount) in USD (currency) is a distinct Value Object. If you had another Money object with $5 in USD, even though they're not the same instance, they are considered equal because their attributes and values are identical.

Here's a basic example in TypeScript:

```typescript
class Money {
  constructor(private amount: number, private currency: string) {}

  equals(other: Money): boolean {
    return this.amount === other.amount && this.currency === other.currency;
  }
}

const fiveDollars = new Money(5, "USD");
const anotherFiveDollars = new Money(5, "USD");

console.log(fiveDollars.equals(anotherFiveDollars)); // Outputs: true
```

In this example, even though fiveDollars and anotherFiveDollars are different instances, they are considered equal because their amount and currency attributes are identical.

### Aggregates

Aggregate is a pattern that helps us ensure the consistency of changes to objects in the business domain.

An Aggregate is a group of related objects that are treated as a single unit. This group consists of one or more Entities, and possibly Value Objects. All of these objects together form a single conceptual whole, where one Entity leads as the "root," and the others are encapsulated within this root.

The root Entity has a global identity, meaning it can be directly accessed and referenced by other objects. The other objects inside the Aggregate can only be accessed through the root. The root is also responsible for maintaining the integrity of the Aggregate, ensuring all its rules or invariants are satisfied.

A real-world example of an Aggregate might be an Order in an e-commerce system. The Order could be the Aggregate root, with a unique order ID. It could contain several OrderLineItem objects, each representing a product in the order and the quantity ordered. When you add a new OrderLineItem, the Order Aggregate could check if the total order value exceeds a customer's credit limit, maintaining business rules.

Example of this:

```typescript
class OrderLineItem {
  constructor(public productId: string, public quantity: number) {}
}

class Order {
  private lineItems: OrderLineItem[] = [];

  constructor(public orderId: string) {}

  addLineItem(item: OrderLineItem) {
    this.lineItems.push(item);
    // Here we could enforce some rules, e.g., the total quantity must not exceed a certain value
  }

  getLineItems(): OrderLineItem[] {
    return this.lineItems;
  }
}

// Usage
const order = new Order("order1");
order.addLineItem(new OrderLineItem("product1", 2));
```

In this code, Order is an Aggregate root, and OrderLineItem is part of the Aggregate. Any modifications to the Order or associated OrderLineItems are made through the Order, ensuring consistency and integrity of the Aggregate.

### Repositories

Repository is a pattern that provides a way to obtain references to, find, and store Aggregates. Repositories act like a collection of Aggregates, hiding the details of how data is persisted, retrieved, and stored.

A Repository might provide methods for adding, removing, or retrieving Aggregates. Repositories should provide methods that align with the needs of the domain and its use cases, which might include finding Aggregates that meet certain criteria.

For instance, in an online shopping system, you might have an OrderRepository. This repository would provide methods for storing and retrieving Order Aggregates, possibly offering methods to find Orders by customer ID, status, or date range.

```typescript
class OrderRepository {
  private orders: Order[] = []; // This is simple example with array instead of data source.

  save(order: Order): void {
    this.orders.push(order);
  }

  findById(orderId: string): Order | undefined {
    return this.orders.find((order) => order.orderId === orderId);
  }

  findAll(): Order[] {
    return this.orders;
  }
}

// Usage
const orderRepository = new OrderRepository();

const order = new Order("order1");
order.addLineItem(new OrderLineItem("product1", 2));

orderRepository.save(order);

console.log(orderRepository.findById("order1"));
```

In this code, `OrderRepository` is a repository for `Order` Aggregates. It provides methods to save an Order, find an Order by its ID, and find all Orders. The actual data storage mechanism (in this case, an in-memory array) is abstracted away from the client code.

### Domain events

Domain Events are a key concept in Domain-Driven Design. They are a representation of a significant happening in the domain. When a state changes in the system that is of interest to other parts of the system, a Domain Event is raised. It's a way to communicate between different parts of the system in a decoupled manner.

Domain Events are different from system events or application events in that they represent something meaningful within your domain, not technical events like a button click or a file save.

For instance, in an e-commerce system, when an order is placed, it might raise an `OrderPlaced` Domain Event. This event could then be used to trigger various actions elsewhere in the system, like sending a confirmation email to the customer, updating the inventory system, notifying the shipping department, etc.

Here's a simple TypeScript example:

```typescript
class OrderPlaced {
  constructor(
    public readonly orderId: string,
    public readonly orderDate: Date
  ) {}
}

class Order {
  //...

  place() {
    // logic to place the order
    //...

    // raise domain event
    return new OrderPlaced(this.orderId, new Date());
  }
}

// Usage
const order = new Order("order1");
const event = order.place();

// Handle the event in Event Handler
if (event instanceof OrderPlaced) {
  console.log(`Order ${event.orderId} placed at ${event.orderDate}`);
}
```

In this code, the `Order` Entity raises an `OrderPlaced` Domain Event when the order is placed. This event can then be handled elsewhere in the system to trigger other actions.

#### What are Domain Services and Application Services?

Domain Services encapsulate domain-specific operations that do not naturally belong to any Entity or Value Object. These services become necessary when you need to express a sequence of domain operations that involves multiple domain entities or value objects, but doesn't logically fit into any single one of them.

Application Services, on the other hand, orchestrate the use of Domain Services, Entities, and Value Objects to implement use cases or user stories in the system. They are the entry point to the core domain layer and are typically used to handle infrastructure concerns like transaction management and coordinating responses from multiple domain objects.

Let's consider an e-commerce system where we have an `Order` entity and a `Customer` entity. We'll introduce a domain service called `DiscountService` which calculates a discount based on the customer's loyalty level and the total order amount. This doesn't naturally belong to any particular entity, hence we implement it as a Domain Service.

Then, we'll have an application service called `OrderService` which orchestrates the use of the `Order` entity, the `Customer` entity and the `DiscountService` to implement the use case of placing an order.

```typescript
// discount.service.ts
import { Injectable } from "@nestjs/common";
import { Customer } from "./customer.entity";

@Injectable()
export class DiscountService {
  calculateDiscount(customer: Customer, totalAmount: number): number {
    // Example: 10% discount for gold customers
    if (customer.loyaltyLevel === "gold") {
      return totalAmount * 0.1;
    }
    return 0;
  }
}

// order.service.ts
import { Injectable } from "@nestjs/common";
import { InjectRepository } from "@nestjs/typeorm";
import { Repository } from "typeorm";
import { Order } from "./order.entity";
import { Customer } from "./customer.entity";
import { DiscountService } from "./discount.service";

@Injectable()
export class OrderService {
  constructor(
    private discountService: DiscountService,
    @InjectRepository(Order) private orderRepository: Repository<Order>,
    @InjectRepository(Customer) private customerRepository: Repository<Customer>
  ) {}

  async placeOrder(customerId: number, order: Order) {
    const customer = await this.customerRepository.findOne(customerId);
    if (!customer) {
      throw new Error("Customer not found");
    }

    const discount = this.discountService.calculateDiscount(
      customer,
      order.totalAmount
    );
    order.applyDiscount(discount);

    await this.orderRepository.save(order);
  }
}
```

In this example, `DiscountService` is a Domain Service that encapsulates the domain logic for calculating discounts. `OrderService` is an Application Service that uses the `DiscountService` and repositories to implement the use case of placing an order. The `placeOrder` method in `OrderService` is a transaction script coordinating the necessary domain objects (`Order` and `Customer`) and the domain service (`DiscountService`).

#### What is Infrastructure Service in DDD?

Infrastructure Services in Domain-Driven Design (DDD) provide technical capabilities to support the layers of your application. These services abstract technical concerns, such as interacting with databases, sending emails, or communicating with external services or APIs.

Infrastructure Services are not to be confused with Domain Services or Application Services. While Domain Services encapsulate domain-specific operations that don't fit naturally into entities or value objects, and Application Services orchestrate domain objects to implement business use-cases, Infrastructure Services provide the technical capabilities that support both Domain Services and Application Services.

Let's consider an email notification service as an example of an Infrastructure Service. The service abstracts the technical details of sending an email, which might include connecting to an SMTP server, formatting the message, and handling errors.

Here is how it might look like in a NestJS application:

```typescript
// email.service.ts
import { Injectable } from "@nestjs/common";
import { ConfigService } from "@nestjs/config";

@Injectable()
export class EmailService {
  constructor(private configService: ConfigService) {}

  sendEmail(to: string, subject: string, message: string): void {
    // The actual implementation might involve a library like nodemailer
    // For the purpose of this example, let's just console.log the details
    console.log(
      `Sending email to ${to} with subject ${subject} and message ${message}`
    );
  }
}

// user.service.ts
import { Injectable } from "@nestjs/common";
import { InjectRepository } from "@nestjs/typeorm";
import { Repository } from "typeorm";
import { User } from "./user.entity";
import { EmailService } from "./email.service";

@Injectable()
export class UserService {
  constructor(
    @InjectRepository(User) private userRepository: Repository<User>,
    private emailService: EmailService
  ) {}

  async registerUser(email: string, password: string) {
    // Some registration logic here...

    // Use Infrastructure Service to send a welcome email
    this.emailService.sendEmail(
      email,
      "Welcome to Our Service",
      "Thank you for registering with us."
    );
  }
}
```

In this example, `EmailService` is an Infrastructure Service that encapsulates the technical concern of sending an email. The `UserService`, which might be considered an Application Service, uses `EmailService` to send a welcome email after registering a user. This demonstrates the principle of separation of concerns, with technical concerns handled by Infrastructure Services, while business logic is dealt with in the domain layer (entities, domain services) and coordinated by application services.
