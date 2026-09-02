# Adapter and Decorator Exercise

> **AI usage for this assignment is Level 1 — Reference** (see the [AI Usage Level Definitions](https://claude.ai/public/artifacts/b1e35a55-f03f-4120-ba4f-b83a1ee3fc83)), the course default: AI helps you learn, but it never sees your work.
  
## Adapter Pattern Exercise

The [adapter-starter-code.zip](./adapter-starter-code.zip) for the Adapter Pattern exercise contains the following classes and files:

1. A class named Contact that contains information about a contact.
1. A class named ContactManager that contains a list of contacts.
1. A class named Table that can be used to display a table of data in the console (i.e., shell).  Table can display any data/object that implements the TableData interface.
1. An interface named TableData that defines the structure of the data the Table class can display.
1. A file named Main that you will modify to display your contact manager data in a Table. 
Modify the provided Main function in Main.ts to use the Table class to display the contents of a list of contacts in the console.  This will require you to write an adapter class that wraps the ContactManager and implements the TableData interface.

## Decorator Pattern Exercise

1. Define an interface named StringSource that represents an object that produces strings.  Your interface should look something like this:
    ```
    export interface StringSource { next(): string; }
    ```
1. Write at least two classes that implement the StringSource interface, and that return some interesting strings.  The strings may be hard-coded, come from a file, come from the keyboard, or wherever you like.
1. Implement the Decorator Pattern by creating at least three decorator classes each of which performs some kind of transformation on a StringSource.  For example, a decorator might reverse strings or manipulate whitespace or add punctuation or whatever you can think of.  Be creative.
1. Write a program that demonstrates your string decorators in action, including an example of two decorators at the same time.

## Submission

On Canvas submit the following files:
- Adapter Exercise
    - The ContactManager wrapper class.
    - The main function that uses the wrapper class to display the Contact info using the table class
- Decorator Exercise
    - All files created including a main file showing the use of the different decorators
