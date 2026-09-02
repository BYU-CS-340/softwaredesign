# Abstract Factory Exercise

> **AI usage for this assignment is Level 1 — Reference** (see the [AI Usage Level Definitions](https://claude.ai/public/artifacts/b1e35a55-f03f-4120-ba4f-b83a1ee3fc83)), the course default: AI helps you learn, but it never sees your work.
  
## Holiday Decorations

The code provided for this exercise in [factory-starter-code.zip](./factory-starter-code.zip) has a small and simple program that creates a string detailing the preparations for a holiday party. Unfortunately, it is hard-coded for Halloween decorations. The only thing that needs to change to make the code work for a different holiday is to change the decoration providers, but we don't want to replace the already working Halloween decoration providers. We'd like to instead break the hard-coded dependency on the Halloween decoration providers and make it so we can pass providers for any holiday we want.

## Working Project

Start by creating a new project and making sure the provided main class runs. You should see output like the following:

```
Everything was ready for the party. The jack-o-lantern was in front of the house, the spider-web was hanging on the wall, and the tablecloth with ghosts and skeletons was spread over the table.
```

Now that you have a working program, you will improve it by applying the Abstract Factory pattern.

## Dependency Inversion

To break the hard-coded dependencies on the Halloween decoration providers, we will implement dependency inversion. Dependency inversion is a way to invert (reverse the direction of) a dependency. It is done in four steps:

1. Create an interface that has the method(s) that are currently depended on in the class on which we want to break the dependency
1. Change the class that has the dependency to depend on the interface
1. Make the class that was depended on implement the interface from step 1
1. Pass an implementation of the interface into the class that had the original dependency

Implement dependency inversion for your DecorationPlacer to break its dependency on the Halloween decoration providers. You should have an interface for each of the decoration providers, and the DecorationPlacer should depend only on those interfaces, not on the Halloween implementations. You should make a constructor for the DecorationPlacer that receives implementations of each of the interfaces. The implementations can be passed from your Main class. Make sure your program still works as it did before.

## Abstract Factory

Dependency Inversion is great, but now, if we want to have our DecorationPlacer do Christmas or Easter decorations, we have to configure all three of the providers each time. If I'm using a Christmas wall hanging, I know already that I want a Christmas yard ornament and a Christmas tablecloth pattern, so passing all three providers into the DecorationPlacer separately seems redundant.

Implement the Abstract Factory Pattern so that the DecorationPlacer only takes a factory interface as a parameter in its constructor. You will need to create a Halloween factory implementation of the factory interface. Be sure the program still works as it did before.

## Extendibility

Now, our program can be easily extended to decorate for more holidays. Implement providers and a factory for another holiday of your choice. Instantiate a DecorationPlacer with your new factory in the main method and look at its output to see that your new factory works.

## Submission

Submit the following files through Canvas as individual files (NOT A ZIP FILE):

- modified Main
- modified DecorationPlacer
- The three provider interfaces
- The factory interface
- modified Halloween providers
- Halloween provider factory
- Your three additional providers
- Your additional provider factory
