# MIDTERM 2 - Preparation Help

- Taken in the testing center
    - Thursday, October 29th 8:00 am - Tuesday, November 3rd.
        - Checkout closes at 9 pm on November 3rd with tests collected at 10 pm
    - Late fees start on Tuesday, November 3rd at 8:00 am
- Question types: T/F, M/C and open-ended essay questions
    - Enter T/F and M/C answers on bubble sheet
    - Write open-ended essay answers on the exam 
- No time limit. Give yourself 2 - 3 hours, although some students will finish in closer to one hour
- Scratch paper is allowed but you must bring your own, have it stamped at the testing center, and turn it in with your exam
- If there is code in a question, it will be in TypeScript. If you are to write code for a response, it is expected to be in TypeScript
- General preparation tip: review the reading, lecture slides, and exercises we've covered so far
- Tip: a good way to learn/study design patterns is to focus on:
    - The intent of the pattern or the design problem it intends to solve
    - The structure of the solution (e.g. the classes, interfaces and relationships that play different roles and why each is part of the solution)
    - Examples of when and how you might use the pattern and what the benefits of doing so might be.
    - Be prepared to write code on the exam that implements the pattern.

## Topics

Midterm 2 covers the second third of the course, from Code Reuse through API Gateway. Earlier topics are covered on [Midterm 1](../midterm-1-review/midterm-1-review.md), and later topics on the [Final Exam](../final-review/final-review.md).

*The following is a list of topics covered in this portion of the course with questions from quizzes and a few study tips.*

Topic: Code Reuse
- What are the primary ways to achieve code reuse?
    - Parameterization (e.g., Generic Types)
    - Implementation Inheritance
    - Composition/Delegation (wrapping)
- Generic Programming with Typescript Generics
    - Know how to write generic classes and methods using Typescript syntax.

Topic: Open-Closed Principle
- What should be open?
- What should be closed?

Topic: Template-Method Pattern

Topic: Design principles
- Topic: Simplicity
- Topic: Avoid code duplication
    - In addition to making code more maintainable, name another benefit of avoiding code duplication.
- Topic: Orthogonality
    - In the context of software design, what is the principle of "Orthogonality"? What design principles discussed in this course can help us achieve orthogonality in our designs? 
- Topic: Single responsibility principle
- Topic: Minimize Dependencies
- Topic: Decomposition
    - The reading suggests three specific ways to determine whether a module has been decomposed sufficiently.  Name two of them.
- Topic: High-quality abstraction
    - In situations where a primitive type (int or string, say) could be used to represent a concept, when might it be better to create a new class instead of using that primitive? Give at least two reasons.
- Topic: Information hiding
    - List 2 specific ways to achieve information hiding. 
- Topic: Depend on abstractions
    - In the following method which design principle is violated? How would you fix it?

>       void f(ArrayList<String> names) {
>           for (int i = 0; i < names.size(); i++) {
>               names.set(i, names.get(i).toUpperCase());
>           }
>       }

- Topic: Isolated change principle
- Topic: Error handling
- Topic: Algorithm and data structure selection

Topic: Testing with Mocks and Spies (jest, ts-mockito, react testing library)
- What is the role of "fake" objects in unit and integration testing? What useful things can "fake" objects do in test cases?
- What is the difference between "mocks" and "spies"?
- What is the difference between "when" and "verify" in ts-mockito?

Topic: AWS console, CLI and SDK

Topic: Lambda (and IAM)

Topic: API Gateway
