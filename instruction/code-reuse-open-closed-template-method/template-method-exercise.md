# Template Method Pattern Exercise

> **AI usage for this assignment is Level 1 — Reference** (see the [AI Usage Level Definitions](https://claude.ai/public/artifacts/b1e35a55-f03f-4120-ba4f-b83a1ee3fc83)), the course default: AI helps you learn, but it never sees your work.
  
Complete the exercise and submit your work on Canvas under the Template Method Pattern assignment.

In the provided source files at [template-starter-code.zip](./template-starter-code.zip), the LineCount class implements a program that counts the number of lines contained in the files in a directory (recursively, if desired).  Similarly, the FileSearch class implements a program that searches the files in a directory for a pattern (i.e., regular expression), and prints out all of the lines that contain the specified pattern.  The README.txt file contains more detailed documentation for both of these programs.

There is a lot of code duplication between the LineCount and FileSearch classes because both programs do almost the same thing. Of course, these classes also contain many differences, as well.  Your task is to eliminate the code duplication by refactoring the LineCount and FileSearch classes to implement the Template Method pattern.  Your solution should contain three classes: a base class that contains the template method, along with modified LineCount and FileSearch classes that inherit from the new base class and override its abstract methods.

**Note:** After unzipping the provided source code, open a terminal and run `npm install` to download and install the required dependencies into your project.

**Note:** In the ImageEditor assignment we provided instructions for you to setup the project to transpile your typescript code to Javascript and run it with node. You can run typescript code directly (without transpiling it to Javascript) using ts-node if you install ts-node as either a global or local dependency. The provided code contains a local dependency for ts-node so you can run it with `npx ts-node` after running `npm install`, as shown in the Readme file.

## Submission

Submit the following .ts files. DO NOT SUBMIT A .zip FILE.
- Modified FileSearch.ts file
- Modified LineCount.ts file
- File containing the template (parent) class
