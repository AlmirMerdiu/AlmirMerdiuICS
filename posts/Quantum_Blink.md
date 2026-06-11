---
title: "Quantum Blink Learning Logs"
date: 2026-06-08
---

# Quantum Blink - Final Project Learning Logs

## Learning Log 1: Variables & Data Tracking

### a) Concept Implemented
I have implemented Variables and Data Tracking across my game using data types (`color`, `int`, `float`, and `boolean`) to store, update, and manage game in real-time.

```java
// Snippet of variable tracking in the global configuration and loop states
color WALL_DARK_BLUE = color(25, 38, 52);
color NEON_CYAN      = color(0, 240, 255);
color HAZARD_ORANGE  = color(255, 120, 0);

boolean isHovering = (mouseX > x - 150 && mouseX < x + 150 && mouseY > y - 35 && mouseY < y + 35);
float rightBubble1 = 500 - (frameCount % 400);
```

### b) Application & Purpose
--
Different variable types were needed to handle specific tasks within program:

color (Theme): Stores color data globally. This keeps the it consistent and lets me update the game's entire color palette by changing a single line of code.

boolean (Tracking): States if its a simple true or false based on mouse coordinates. It acts as a trigger to tell buttons exactly when to highlight themselves.

float (Animations): Decimal numbers track positions for moving visual. This prevents the choppy rendering that happens when using rigid, whole integers for continuous movement.

int : Whole numbers calculate pixel-perfect grid locations on the screen, preventing decimal rounding errors during screen math.

### c) Challenges & Fixes
--
Challenge 1: Global Variable Overwriting

The Issue: I tried using one global boolean variable for isHovering inside the main draw() loop. Because the program draws buttons one after the other, the last button always overwrote the variable. This meant only the bottom button changed colors when hovered, while the others did nothing.

The Resolution: I moved the variable out of the global scope. Instead, I made isHovering a local variable inside the custom drawMenuButton() function. This separate calculation isolates the math for each button so they work independently.

## Learning Log 2: Selection Structures

### a) Concept Implemented
I implemented **Selection Structures (Conditional Statements)** using `if/else` structures to control how the user interface reacts to the player's mouse movements.

```java
// Snippet from drawMenuButton() handling hover states
if (isHovering) {
  fill(0, 150, 180); // Highlight color when mouse is over button
  stroke(NEON_CYAN);
} else {
  fill(WALL_DARK_BLUE); // Default color when mouse is away
  stroke(0, 150, 180);
}
```

###b) Application & Purpose
This particular if statement lies within my own drawMenuButton() function. The primary intention behind this logic structure is for it to immediately provide the user some visual confirmation that they have moved their mouse over a certain menu option. If the boolean isHovering is true, then the program will run through the first set of commands in which it will change the fill and stroke colors of the button in order to light it up. However, if the isHovering variable is not true, the second part of the code will be executed, leaving the button its default dark blue color.

###c) Challenges & Fixes
Challenge 1: Conflicts of Overlapping Buttons

Problem: Initially, I had an if condition that only checked the mouse coordinates with respect to a line crossing horizontally. This was due to the overlapping range because of multiple buttons positioned above each other vertically. As a result, movement of the mouse triggered multiple buttons to be highlighted at the same time.

Solution: I resolved this issue by using a compound conditional statement with the help of the AND operator (&&). I combined checking of both the horizontal and vertical ranges using if (mouseX > x - 150 && mouseX < x + 150 && mouseY > y - 35 && mouseY < y + 35). This created a closed boundary box that ensures only one button is highlighted at a time.

Challenge 2: Missing State Control Blocks

The Problem: When I first added the animated background grid loops and floating bubble math from drawLabBackground(), the graphics rendered continuously. Because I didn't wrap this code inside a screen selection structure, the grid layout and moving bubble variables kept drawing directly over my main menu text, making the game title completely unreadable when the program launched.

The Fix: I resolved this by adding a global integer variable to track screens and wrapping my background functions inside an explicit conditional structure. By putting the main gameplay drawings inside an if (gameState == 1) selection block, I made sure the code only runs when the active player changes the screen away from the menu.

