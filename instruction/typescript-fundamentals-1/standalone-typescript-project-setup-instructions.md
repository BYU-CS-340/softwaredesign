# Standalone Typescript Project Setup Instructions

For a standalone Typescript project (one that will simply be executed from the command-line) you can simplify the project setup steps and run your Typescript code using ts-node (instead of node) without transpiling your code to Javascript.

## Assumptions

npm is already installed on your computer.

## Setup

1. Create a directory for your project
1. CD into the new directory
1. Run `npm init -y`
1. Run `npm install typescript --save-dev`
1. Run `npm install @types/node --save`
1. Run `npm install ts-node --save-dev`
1. Create an src directory
1. Create a typescript file in your src directory and write your typescript code
1. Run `npx tsc --init`
1. Edit compilerOptions in tsconfig.json to contain the following settings:
    ```
    "target": "ESNext",
    "module": "commonjs",
    "sourceMap": true,
    "outDir": "dist",
    ```
1. Add other compilerOptions settings as desired 

**Note:** If you do not include these steps, ts-node will use reasonable defaults when running your code.

## Running Your Typescript Code

`npx ts-node src/<Your Typescript File.ts>`

## Debugging in VS Code

You can setup a standalone Typescript program for debugging in VS Code by following these steps:

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
                "name": "Debug <Name of Program>",
                "runtimeArgs": ["-r", "ts-node/register"],
                "args": ["${workspaceFolder}/src/<File to Execute>.ts"],
                "sourceMaps": true
            }
        ]
    } 
    ```    
      
**Note:** Change the name attribute value to a name that describes your program and change <File to Execute> inside the "args" attribute value with the name of the file you want to debug.

**Note:** You can have multiple configurations inside the configurations attribute array value. This is useful if your project contains multiple Typescript programs.
