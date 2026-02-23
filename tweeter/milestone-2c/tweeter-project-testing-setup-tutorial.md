# Tweeter Project Testing Setup Tutorial

## Overview

This tutorial will help you setup the Tweeter project to support testing of the code in all layers of the project, including the React components, by adding and configuring several testing libraries. Specifically, we will add ts-mockito to support mocking and React Testing Library to support testing React components. We will also configure Jest as the test runner and Babel to transpile the Typescript tests to Javascript so Jest can run them.

## Dependencies

Several dependencies need to be added to support testing. Run the following commands from within the tweeter-web module/folder to add the required libraries for Jest, Babel, React Testing Library and ts-mockito:

```bash
npm install --save-dev jest@29.7.0

npm install --save-dev jest-environment-jsdom@29.7.0

npm install --save-dev ts-jest@29.1.1

npm install --save-dev @types/jest@29.5.8

npm install --save-dev babel-jest@29.7.0

npm install --save-dev @babel/core@7.23.3

npm install --save-dev @babel/preset-env@7.23.3

npm install --save-dev @babel/preset-typescript@7.23.3

npm install --save-dev @testing-library/react@14.1.0

npm install --save-dev @testing-library/jest-dom@6.1.4

npm install --save-dev @testing-library/user-event@14.5.1

npm install --save-dev @typestrong/ts-mockito@2.7.12
```

## Configuration Changes in package.json

Make the following changes to the package.json file **in the tweeter-web module** (not to the package.json file at the root of your project):

Add the following to the scripts section of package.json:

```json
"test": "jest --passWithNoTests"
```

**Note:** This and any other element can be added at any location in it's section. If added as the last item, it cannot be followed by a comma. If not added as the last item, it must be followed by a comma.

Add the following as a new section to package.json:

```json
"jest": {
    "verbose": true,
    "testEnvironment": "jsdom",
    "moduleNameMapper": {
        "\\.(css|less)$": "<rootDir>/test/__mocks__/styleMock.js"
    },
    "testMatch": [
        "<rootDir>/test/**/*.test.tsx",
        "<rootDir>/test/**/*.test.ts"
    ],
    "transform": {
        "^.+\\.tsx?$": "ts-jest",
        "\\.js$": "babel-jest"
    }
}
```

Your package.json should look about like this, although you may have different versions of some packages and various other differences:

```json
{
    "name": "tweeter-web",
    "version": "0.1.0",
    "private": true,
    "dependencies": {
        "@fortawesome/fontawesome-svg-core": "^6.2.1",
        "@fortawesome/free-brands-svg-icons": "^6.2.1",
        "@fortawesome/free-solid-svg-icons": "^6.2.1",
        "@fortawesome/react-fontawesome": "^0.2.0",
        "@types/react": "^18.2.5",
        "@types/react-dom": "^18.2.3",
        "bootstrap": "^5.2.3",
        "buffer": "^6.0.3",
        "node": "^20.5.0",
        "react": "^18.2.0",
        "react-bootstrap": "^2.7.4",
        "react-dom": "^18.2.0",
        "react-infinite-scroll-component": "^6.1.0",
        "react-router-dom": "^6.11.1",
        "tweeter-shared": "../tweeter-shared",
        "typescript": "^4.9.5",
        "vite-plugin-commonjs": "^0.8.2",
        "web-vitals": "^2.1.4"
    },
    "scripts": {
        "start": "vite",
        "build": "tsc && vite build",
        "serve": "vite preview",
        "reload": "npm run build && npm run serve",
        "test": "jest"
    },
    "eslintConfig": {
        "extends": [
            "react-app",
            "react-app/jest"
        ]
    },
    "browserslist": {
        "production": [
            ">0.2%",
            "not dead",
            "not op_mini all"
        ],
        "development": [
            "last 1 chrome version",
            "last 1 firefox version",
            "last 1 safari version"
        ]
    },
    "devDependencies": {
        "@babel/core": "^7.23.3",
        "@babel/preset-env": "^7.23.3",
        "@babel/preset-typescript": "^7.23.3",
        "@testing-library/jest-dom": "^6.1.4",
        "@testing-library/react": "^14.1.0",
        "@testing-library/user-event": "^14.5.1",
        "@types/jest": "^29.5.8",
        "@types/node": "^20.4.6",
        "@vitejs/plugin-react": "^4.0.0",
        "babel-jest": "^29.7.0",
        "jest": "^29.7.0",
        "jest-dom": "^4.0.0",
        "jest-environment-jsdom": "^29.7.0",
        "ts-jest": "^29.1.1",
        "@typestrong/ts-mockito": "^2.7.12",
        "vite": "^4.3.9",
        "vite-plugin-svgr": "^3.2.0",
        "vite-tsconfig-paths": "^4.2.0"
    },
    "jest": {
        "verbose": true,
        "testEnvironment": "jsdom",
        "moduleNameMapper": {
        "\\.(css|less)$": "<rootDir>/test/__mocks__/styleMock.js"
        },
        "testMatch": [
            "<rootDir>/test/**/*.test.tsx",
            "<rootDir>/test/**/*.test.ts"
        ],
        "transform": {
            "^.+\\.tsx?$": "ts-jest",
            "\\.js$": "babel-jest"
        }
    }
}
```

## Babel Configuration

Add a babel.config.js file to the tweeter-web module/folder with the following contents:

```javascript
module.exports = {
    presets: [
        ["@babel/preset-env", { targets: { node: "current" } }],
        "@babel/preset-typescript",
    ],
};
```

## Additional Configuration

1. Add a `test` folder to the tweeter-web module (directly under the `tweeter-web` folder)
1. Add a `__mocks__` folder under the `test` folder

   **Note:** the use of double underscores in the name is a convention to indicate that this is a configuration folder that should not be removed or renamed. You can rename it anything you like as long as you update the reference to it in the `jest` section of `package.json`.

1. Add a `styleMock.js` file to the `__mocks__` folder with the following contents:

```javascript
module.exports = {};
```

**Note:** this file is referenced in the `jest` section of `package.json` and allows Jest to ignore .css files.

## Validate Jest Configuration

To validate that Jest is configured properly do the following:

1. Run `npm run build`. Your project should build as before.
1. Run `npm run test`. You should see output like the following:

```txt
tweeter-web@0.1.0 test
jest --passWithNoTests

No tests found, exiting with code 0
```
