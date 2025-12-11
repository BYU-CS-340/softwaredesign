# FINAL EXAM - Preparation Help

- Taken in the class room during exam week
- 24 Questions: 8 T/F (worth 2 points each),  11 M/C (worth 4 points each) and 5 open-ended essay questions (worth 8 points each)
    - Circle T/F and M/C answers in the exam booklet
    - Write open-ended essay answers in the exam booklet
- Time limit: 3 hours
- Scratch paper is allowed but you must bring your own and turn it in with your exam
- If there is code in a question, it will be in TypeScript. If you are to write code for a response, it is expected to be in TypeScript
- General preparation tip: review the reading, lecture slides, and exercises we've covered so far
- Tip: a good way to learn/study design patterns is to focus on:
    - The intent of the pattern or the design problem it intends to solve
    - The structure of the solution (eg, the classes, interfaces and relationships that play different roles and why each is part of the solution)
    - Examples of when and how you might use the pattern and what the benefits of doing so might be.
    - How is this pattern similar and different from other patterns (e.g., template method vs strategy)

## Topics

The final exam is *cumulative*, covering the entire semesters' topics. The exam heavily emphasizes post-midterm topics, but some questions do cover pre-midterm material. For a summary of pre-midterm topics, see: [MIDTERM - Preparation help](../midterm-review/midterm-review.md).

Here are the post-midterm topics:

Topic: Dependency Inversion Principle 

Topic: DynamoDB
- How does DynamoDB differ from a relational database?
- Why must a DynamoDB query include a value for the "partition key"?
- What does a sort key do and when do you need one?
- What is the purpose of indexes in DynamoDB (particularly, Global Secondary Indexes)?

Topic: Data access object (DAO) pattern
- How have you used the DAO pattern in your project? What did your interfaces look like?

Topic: Data transfer object (DTO) pattern

Topic: Abstracting external dependencies

Topic: Designing for Testability, Abstract Factory 

Topic: Asynchronous Messaging, SQS
- How did you use this in your M4 design? Why?
- What are the different queue types and why would you choose one over the other?
- What are the different configuration options for an SQS queue?

Topic: Inheritance vs Delegation/composition
- Explain how object composition/delegation is used in the the proxy pattern, the strategy pattern, and the decorator pattern? Why is that used?
- The relative advantages and disadvantages of class/implementation inheritance (a.k.a. "extends") vs composition?

Topic: Façade pattern

Topic: Proxy pattern
- How are the Facade and Proxy patterns similar?
- How are they different?

Topic: Strategy Pattern
- Notice that the strategy and template method patterns may be used to solve the same problems. How would the solutions differ?  How would you decide which to use?

Topic: Adapter pattern
How would you implement the following stack interface using an existing ArrayList class?

>       interface Stack<T> {
>         void push(T t);
>         T pop();
>       }

Topic: Decorator pattern
- Suppose you wanted to extend your get followers endpoint to include an optional parameter to support different kinds of compression. How might the decorator pattern help?

Topic: Command pattern
- What are some uses beyond undo/redo stacks? What would be the elements of such a solution?
