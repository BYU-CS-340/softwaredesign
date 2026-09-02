# Command Exercise

> **AI usage for this assignment is Level 1 — Reference** (see the [AI Usage Level Definitions](https://claude.ai/public/artifacts/b1e35a55-f03f-4120-ba4f-b83a1ee3fc83)), the course default: AI helps you learn, but it never sees your work.
  
The provided code in [command-starter-code.zip](./command-starter-code.zip) implements a simple console-based text editor program which lets the user edit text documents. A text document is simply a sequence of characters. The program lets the user perform the following operations:

1. Start a new, empty document
1. Insert a string at a specified index in the document
1. Delete a sequence of characters at a specified index
1. Replace a sequence of characters at a specified index with a new string
1. Display the current contents of the document
1. Save the document to a file
1. Open a document from a file

Your task is to use the Command pattern to add Undo and Redo operations to the program. Your implementation must use the Command pattern, and not some alternate approach. More specifically, do the following:

1. Define a Command interface or abstract class
1. Write an UndoRedoManager class that contains the undo and redo Command stacks, and provides operations such as: execute, undo, redo, canUndo, canRedo
1. Modify the TextEditor class to implement undo/redo using Command and UndoRedoManager. Undo/Redo must work for the following operations:
    - Start
    - Insert
    - Delete
    - Replace
    - Open

## Submission

Submit files containing the following
- The modified TextEditor class now utilizing the Command Pattern
- A command interface/abstract class
- All necessary concrete implementations of the command interface
- The UndoRedoManager class
