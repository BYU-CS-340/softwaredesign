# Unit Testing with Mocks and Spies Exercise

> **AI usage for this assignment is Level 1 — Reference** (see the [AI Usage Level Definitions](https://claude.ai/public/artifacts/b1e35a55-f03f-4120-ba4f-b83a1ee3fc83)), the course default: AI helps you learn, but it never sees your work.

## Explanation

The code in [mock-starter-code.zip](./mock-starter-code.zip) has 3 important files:

### Client.test.ts

Open the directory and run `npm install` to install the needed packages. Run `npm test` and all files ending with the extension .test.ts will be run (only Client.test.ts in this case). Look at package.json to see how the test command was setup.

This class is supposed to run tests on the Client class. The Client class does not have any bugs, but if you run the tests you will see that one fails and the other passes but doesn't actually test anything. The failing test fails because the Client class has a hard coded dependency on the Service class, and the Service class does have bugs.

If you recall from the reading, a unit test should only test a single class so bugs in the Service class should not affect tests that we run on the Client class. Clearly, the tests in Client.test are not good unit tests. In this exercise, you will use ts-mockito to write good unit tests for the Client class.

### Client.ts

This file contains the code that we want to test. The code is correct, so unit tests on this code should pass. However, this class contains a hard-coded dependency on a class with many bugs. In order for this class to be unit tested, we will need to break that hard-coded dependency.

### Service.ts

This file contains code that is full of bugs. DO NOT FIX THE BUGS. The goal of this exercise is not to correct the code, but to write tests that do not run the buggy code at all.

## Steps

1. Break the hardcoded dependency that Client has on Service by creating a factory method and replacing all of the calls to new Service() with a call to the factory method.
    - There are TODOs in the Client class that tell you where you need to do this.
    - **Note:** A factory method is simply a method that creates and returns an instance of an object.
1. Modify the setup method in Client.test to use ts-mockito to mock the Service class and spy on the Client class.
    - There are TODOs that tell you what you need to do.
1. Modify the tests in Client.test to use the ts-mockito framework to write effective unit tests on the Client class.
    - Complete all of the TODOs in the tests.

## Submission

Submit Client.ts and Client.test.ts on Canvas as separate files. DO NOT SUBMIT A ZIP FILE.
