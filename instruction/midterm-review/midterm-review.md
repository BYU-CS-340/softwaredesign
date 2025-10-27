# MIDTERM - Preparation Help

- Taken in the testing center
    - Tuesday, October 28th 8:00 am - Friday, October 31st.
        - Checkout closes at 7 pm on October 31st with tests collected at 10 pm
    - Late fees start on Friday, October 31st at 8:00 am
- Question types: T/F, M/C and open-ended essay questions
    - Enter T/F and M/C answers on bubble sheet
    - Write open-ended essay answers on the exam 
- No time limit. Give yourself 2 - 3 hours, although some students will finish in closer to one hour
- Scratch paper is allowed but you must bring your own, have it stamped at the testing center, and turn it in with your exam
- If there is code in a question, it will be in TypeScript. If you are to write code for a response, it is expected to be in TypeScript
- General preparation tip: review the reading, lecture slides, and exercises we've covered so far

## Topics

*The following is a list of topics covered in the course with questions from quizzes and a few study tips.*

Topic: Typescript
- Review the key concepts covered in class about Typescript.
- Understand the basic syntax, including generic types.
- Focus on how Typescript handles data types (variable and function types).

Topic: React.js (as an approach to UI programming)
- Review the key concepts that react.js is based on and the main facilities provided. Pay particular attention to those discussed/used in this course.

Topic: UML class and sequence diagrams
- Create a UML class or sequence diagram that represents specified information.
- Read/interpret a UML class or sequence diagram
- Tip: Remember that UML diagrams can be used at different levels. For example, we've seen conceptual and implementation level diagrams.

Topic: Software architecture, layers
- What is software architecture?
- Name some benefits of organizing software into layers.

Topic: Design patterns
- Tip: a good way to learn/study design patterns is to focus on:
    - The intent of the pattern or the design problem it intends to solve
    - The structure of the solution (e.g. the classes, interfaces and relationships that play different roles and why each is part of the solution)
    - Examples of when and how you might use the pattern and what the benefits of doing so might be.
    - Be prepared to write code on the exam that implements the pattern.

Topic: Observer pattern

Topic: Principles of user interface architecture: MVC, MVP and MVVM
- How is MVP structurally different from the MVC pattern? What are the advantages and disadvantages of that difference?
- How is MVVM structurally different from the MVP pattern? What are the advantages and disadvantages of that difference?

Topic: Testing with Mocks and Spies (jest, ts-mockito, react testing library)
- What is the role of "fake" objects in unit and integration testing? What useful things can "fake" objects do in test cases?
- What is the difference between "mocks" and "spies"?
- What is the difference between "when" and "verify" in ts-mockito?

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

Topic: AWS console, CLI and SDK

Topic: Lambda, API Gateway (and IAM)
