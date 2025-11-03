# Proxy Exercise

**Write a Proxy that implements lazy loading**

1. Create a new typescript project for this exercise
    - **Note:** You can follow [these instructions](../typescript-fundamentals-1/standalone-typescript-project-setup-instructions.md) to setup a standalone Typescript project
1. Create an interface named Array2D that represents a two-dimensional array of numbers and contains the following methods:
    - A method that sets the value of an array element: set(row, col, value)
    - A method that returns the value of an array element: get(row, col)
1. Create a class that implements the Array2D interface and that also has the following methods:
    - A constructor that lets the caller specify the dimensions of the array or a file name from which a saved array can be read.
        - **Note:** Make all three parameters optional
    - A method that saves the object’s state to a file: save(fileName)
        - **Note:** Use `JSON.stringify` to convert the array to a Json string before saving it to the file.
        - **Note:** To save a string to a file, first import the fs package (`import * as fs from 'fs'`) then use the `fs.writeFileSync` method.
    - A method that loads the object’s state from a file: load(fileName)
        - **Note:** Use `JSON.parse` to convert the file contents back to a 2d array.
        - **Note:** To read text from a file, use the `fs.readFileSync` method, specifying 'utf-8' as the second paramter.
1. Write a program that creates an instance of the Array2D implementing class you created in step 2, populates it with values, and saves it to a file.  This file will be used in the next step.
1. Write a proxy class that implements lazy loading for your 2-D array class.
    - What is lazy loading?  Suppose you have an object stored in a file or database. If the object is large, it will be time-consuming to load, and you might not want to incur the expense of loading the object into RAM until the program actually uses the object. This way, if the program never uses the object, you will not waste time loading it.
    - An elegant way to implement this idea is with the Proxy pattern.
        1. Create a proxy class that implements the same interface as the large object (Array2D)
        1. The proxy class should also have a constructor that accepts the name of the file that stores the object
        1. When the program begins, create a proxy object that represents the large object.  Initially, the proxy will contain a null reference to the “real object”, because it hasn’t been loaded yet.
        1. The first time a method is called on the proxy, it should load the large object from the file into RAM, and store a reference to the loaded object.
        1. After loading the object, all method calls on the proxy should be delegated to the “real object” that was loaded.
1. Write a small program to test your proxy and demonstrate that it works. You need not write automated test cases. Just write a very short program you can use to test whether your proxy works.
 
## Submission 

- On Canvas submit your .ts files as individual files. DO NOT SUBMIT A ZIP FOLDER.
    - If one file contains several classes/interfaces, there may be fewer file uploads than listed below.
- Files
    - File containing the interface "Array2D"
    - File containing the implementation of the "Array2D" interface
    - File containing the lazy-loading proxy
    - Main file containing your tests for the proxy
