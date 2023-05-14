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
