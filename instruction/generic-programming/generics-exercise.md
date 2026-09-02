# Generics Exercise

> **AI usage for this assignment is Level 1 — Reference** (see the [AI Usage Level Definitions](https://claude.ai/public/artifacts/b1e35a55-f03f-4120-ba4f-b83a1ee3fc83)), the course default: AI helps you learn, but it never sees your work.
  
For this exercise you will create some generic classes and generic methods. The files for this exercise can be downloaded [here](./generics-starter-code.zip). Please do the following:

1. Put your solution for this part of the exercise in a file named dresser.ts.

 A dresser (the kind you keep your clothes in) contains a number of drawers. Each drawer contains a particular type of clothing. For example, here are three types of clothing you might keep in your dresser:

```
type Socks = { style: string; color: string }
type Shirt = { style: string; size: string; }
type Pants = { waist: number; length: number; }
```

a. Write a generic Drawer class that contains one kind of clothing (we don't want to mix multiple kinds of clothing in the same drawer). Your Drawer class should have one type parameter specifying the kind of clothing in the drawer, and should have the following public methods:

- isEmpty - a get property method that returns true if the drawer is empty and false otherwise
- addItem - adds an item to the drawer on top of any other items already in the drawer
- removeItem - removes and returns the top item in the drawer, or returns undefined if the drawer is empty
- removeAll - removes and returns all items in the drawer as an array

b. Write a generic Dresser class that contains three drawers (top, middle, and bottom), with each drawer containing a particular kind of clothing. **Your Dresser class should have three type parameters that specify the kind of clothing in each drawer.** The drawers should be publicly accessible.

c. Write a function that demonstrates the functionality of your Dresser and Drawer classes.


2. Study the code in the file named Animal.ts. The output of this program is shown in the file named results.md. Notice the code duplication contained in the Cat and Dog classes. The getCatsSorted and getCatsTrainingPriorities methods in the Cat class are nearly identical to the getDogsSorted and getDogsPriorityList methods in the Dog class. Remove this duplication by replacing these methods with generic methods in the Animal class that work for both Cats and Dogs.

## Submission

Submit your dresser.ts file and your modified Animal.ts file on Canvas. Please submit the files individually (do not put them in a zip file).
 