# Standalone Typescript Project Setup Instructions

For a standalone Typescript project (one that will simply be executed from the command-line) you can simplify the project setup steps and run your Typescript code using ts-node (instead of node) without transpiling your code to Javascript.

## Assumptions

npm is already installed on your computer.

## Setup

1. Create a directory for your project
1. CD into the new directory
1. Run `npm init -y`
1. Run `npm install typescript --save-dev`
    - This adds the most recent typescript (like ^7.0.2) into package.json devDependencies
1. Run `npm install @types/node --save`
    - This adds the most recent @types/node into package.json dependencies
1. Create an src directory
1. Create a typescript file in your src directory and write your typescript code
1. Run `npx tsc --init`
1. Edit compilerOptions in tsconfig.json to contain the following settings:
    ```
    "target": "ESNext",
    "module": "commonjs",
    "sourceMap": true,
    "outDir": "./dist",
    "rootDir": "./src",
    ```
1. Add other compilerOptions settings as desired
   - For example, if you want to use process.argv for arguments, you will want to make sure you have "types": ["node"] as well in tsconfig.json
   - If you are getting the error: `fs has no default export` when trying to import fs from "fs", then make sure you have Esmoduleinterop uncommented and set to true

**Note:** When you run the command `npx tsc --init`, it adds a lot of reasonable defaults. They are good, but force you to code in a specific way. By removing those settings and only leaving the 7 mentioned above, you can remove a lot of errors and squiggly lines that are forcing you to code in a specific way.

## Running Your Typescript Code

`npx tsc -p tsconfig.json && node dist/<Your Main File.js>`

## Debugging in VS Code

You can setup a standalone Typescript program for debugging in VS Code by following these steps:

1. Build your code first with npx tsc -p tsconfig.json
1. Select Run -> Start Debugging
1. Under "Run and Debug", click "create a launch.json file"
1. Replace the contents of the launch.json file with the following:
    ```
    {
      "version": "0.2.0",
      "configurations": [
        {
          "type": "node",
          "request": "launch",
          "name": "Debug Main",
          "program": "${workspaceFolder}/dist/Main.js",
          "outFiles": ["${workspaceFolder}/dist/**/*.js"],
          "sourceMaps": true
        }
      ]
    }
    ```    
      
**Note:** Change the name attribute value to a name that describes your program and change <File to Execute> inside the "args" attribute value with the name of the file you want to debug.

**Note:** You can have multiple configurations inside the configurations attribute array value. This is useful if your project contains multiple Typescript programs.
