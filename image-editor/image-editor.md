
# Image Editor - Convert the TypeScript

The purpose of this assignment is to give you hands-on experience creating and running a TypeScript program.

Image Editor is a programming assignment that was formerly used in CS 240. Image Editor performs transformations on PPM image files (PPM is a text-based image file format). As one example, Image Editor can take a color image and convert it to a black-and-white (i.e., grayscale) version of the same image.  On Image Editor's command-line, the user provides the name of an input image file, the name of the output (i.e., transformed) image file, and the kind of transformation to be performed on the image (e.g., "grayscale"). In addition to "grayscale", Image Editor also supports three other kinds of image transformations: "invert", "emboss", and "motion blur".

For this assignment you will translate a provided Java implementation of Image Editor to an equivalent TypeScript implementation. Specifically, you will:

1. Use NPM to create a TypeScript project
2. Write a TypeScript version of the Image Editor code
3. Use NPM and Node.js to compile and run your program
4. Verify that your program produces the same output as the provided Java implementation

The files required to complete this assignment are available in a zip file located [HERE](ImageEditorFiles.zip). This file contains the following items:

1. The specification for the CS 240 Image Editor assignment
2. A Java solution to the CS 240 assignment
3. Several PPM image files you can use to test your program (i.e., "source images")
4. Transformed versions of the source images (called "key images") you can use to verify the output of your TypeScript program. These files were created by running the Java implementation on the source images, and are provided for your convenience. You could always re-create them by running the Java version yourself (For motion blur, we used a value of 10 to generate the key images).

PPM image files can be viewed with Adobe Photoshop or free programs such as [GIMP](https://www.gimp.org/downloads/).

## Setup Instructions

1. On your computer, install the most current version of Node.js located [HERE](https://nodejs.org/en/download). This will install both the Node.js runtime as well as the NPM "Node package manager" which is used to manage your project's external library dependencies. If you already have an older version of Node.js installed, we recommend that you update to the latest version.

2. If you intend to use VS Code as your TypeScript development environment, and you do not already have it installed, install the most current version of VS Code located [HERE](https://code.visualstudio.com/download).

3. Open a new terminal window, create a directory for your Image Editor project, and cd into the new directory.

>       mkdir image-editor
>       cd image-editor

4. Create a project configuration file by running the terminal command `npm init --yes`. This will create a file named `package.json` in the current directory. Among other things, `package.json` contains a list of all the external libraries your project depends on (including the TypeScript compiler itself).

5. Install the TypeScript compiler in your project.

>       npm install typescript --save-dev

6. Install the TypeScript type definitions for Node.js.

>       npm install @types/node --save

7. Initialize your TypeScript compiler configuration by running the following command. This command will create a file named `tsconfig.json` in the current directory. This file contains all of the configuration options for `tsc` (the TypeScript compiler). Notice that this command says `npx`, not `npm`.

>       npx tsc --init

8. In your `tsconfig.json` file, make sure the following properties in the `compilerOptions` object are uncommented and have the following values:

>       "target": "ESNext",
>       "module": "commonjs",
>       "sourceMap": true,
>       "outDir": "dist",

9. Create one or more `.ts` files that will contain your Image Editor implementation.

10. Add the following `scripts` property to your `package.json` file. These script commands will let you compile and run your program using NPM.

>       "scripts": {
>           "build": "tsc",
>           "start": "node dist/ImageEditor.js"
>       },

Replace `ImageEditor.js` with the name of the file that contains your program's entry point.

11. To compile your program, run the terminal command `npm run build`. To run your program, run the terminal command `npm run start` (add any additional command-line parameters you want to pass to the program).

12. You can also use VS Code to compile, run, and debug your program by creating a run configuration in VS Code, as follows:
<ol type="a">
     <li>In VS Code, in the file tree on the left, select the name of the `.ts` file that contains your program's entry point (e.g., `ImageEditor.ts`)</li>
     <li>Select the `Run -> Add Configuration...` menu option</li>
     <li>Select `Node.js: Launch Program` as the configuration type</li>
   </ol>

This will create a run configuration file named `launch.json` that will enable you to run or debug your program in VS Code. In this file you can also specify command-line arguments VS Code should use when running or debugging your program, by adding the following line inside `"configurations"` in your `launch.json` file:

>       "args": [ "source_images/one-does-not-simply.ppm", "key_images/one-does-not-simply.ppm", "emboss" ]

Having created a run configuration, you can compile your program in VS Code by selecting the `Terminal -> Run Build Task...` menu option, and then selecting the `tsc:build` task. You can run your program in VS Code by selecting the `Run -> Start Debugging` menu option, or the `Run -> Run Without Debugging` menu option.

More details on configuring VS Code as a TypeScript development environment can be found [HERE](https://code.visualstudio.com/docs/typescript/typescript-tutorial).

## Assignment Tips

Your program will need to read and write PPM image text files. To do this file I/O you will need to use APIs that are specific to the Node.js environment. Documentation for the Node.js APIs can be found [HERE](https://nodejs.org/docs/latest/api/). The specific APIs you will need are found in the `File system` module. Use any APIs you like to read and write files. For example, you might use the [readFileSync](https://nodejs.org/docs/latest/api/fs.html#fsreadfilesyncpath-options) function to read the entire contents of a file as a string, and you might use the [openSync](https://nodejs.org/docs/latest/api/fs.html#fsopensyncpath-flags-mode), [writeSync](https://nodejs.org/docs/latest/api/fs.html#fswritesyncfd-string-position-encoding), and [closeSync](https://nodejs.org/docs/latest/api/fs.html#fsclosesyncfd) functions to write data to an output file (i.e., open the file, write data to the file, and close the file).

Your program will also need to access its command-line arguments. In the Node.js environment, command-line arguments can be accessed using the `process.argv` property. Note that the actual command-line arguments start at `process.argv[2]`.

The provided Java code uses integer arithmetic in all of its algorithms. The grayscale and motionblur algorithms use division. The fractional part of any integer division result is automatically dropped by Java. In TypeScript, since all numbers are represented as floating point numbers, the fractional parts of division results are not automatically dropped, and you will need to do this yourself by using the `Math.floor` function.

It's tempting to use the TypeScript's `myArray.shift()` method to simulate `Scanner.nextInt()` in Java. This can technically work, but is not recommended: `shift()` copies the entire array every time it's called, so your code may run incredibly slowly on the larger images.

## Submission

When you are confident that your TypeScript Image Editor is working, submit the following files on Canvas:

- [ ] Your TypeScript source files
- [ ] Your `package.json` file
- [ ] Your `tsconfig.json` file
- [ ] Please submit individual files, not a zip file. This will make it easier to grade your work.
