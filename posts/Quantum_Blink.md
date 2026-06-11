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

## Learning Log 2: Repetition Structures (Loops)

### a) Concept Implemented
I used **Repetition Structures (Loops)** specifically using `for` loops to draw repetitive patterns like background grids and safety hazard lines instead of copy-pasting the same code over and over.

```java
// Loops used for drawing the background grid lines
for (int i = 0; i < width; i = i + 40) {
  line(i, 0, i, height); // Vertical grid lines
}

// Loop used for drawing safety stripes on the platform ledge
for (int i = 0; i < 400; i = i + 20) {
  stroke(HAZARD_ORANGE, 180);
  line(i, 400, i + 10, 410);
}
```
###b) Application & Purpose
Background Grid Lines: Instead of manually calculating and writing out hundreds of separate line() coordinates to fill the screen, I used two for loops. The loops initialize a counting variable at 0 and step forward by 40 pixels on each go. On every turn of the loop, it draws a line across the canvas relative to the system's width and height. It stops executing automatically as soon as the index variable passes the screen.

Hazard Stripes: Inside drawMapForegroundLevel1(), I used a for loop to draw caution stripes on the platform edges. It starts at pixel 0 and stops at pixel 400, drawing a small slanted line every 20 pixels. This lets me generate a long pattern instantly using just a few lines of code.

My reasoning for using loops here was to keep the code organized and easy to change. If I ever want to change the grid tile size from 40 pixels to 20 pixels, I only have to change the increment value in the loop header instead of rewriting the entire background script.

###c) Challenges & Fixes

Challenge 1: Infinite Loop Game 

The Problem: When I first wrote the background grid for loop, I made a mistake in the update expression by writing i + 40 instead of i = i + 40. Because I didn't actually reassign the new value to the counter variable i, it stayed stuck at 0 forever. This created an infinite loop where the program tried to draw a vertical line at coordinate 0 forever, causing the software window to completely freeze and crash when launched.

The Fix: I fixed this by correcting the loop to properly update the loop control variable with i = i + 40 (or i += 40). This ensured that the counter actually moves closer to the condition boundary (i < width) on every single time so the loop ends.

Challenge 2: Hardcoded Loop Boundaries Breaking Map Scale

The Problem: In my vertical grid line loop, I originally set the conditional stop boundary to a fixed number like if (i < 1200). However, when I changed the resolution size of the game canvas in my setup window, the lines stopped rendering halfway across the screen because the loop would exit before reaching the new edge of the wider canvas.

The Fix: I resolved this by replacing the hardcoded number with Processing's built-in dynamic variable width. By rewriting the boundary condition as i < width, the repetition structure automatically resizes itself to match whatever scale the window is running at.

## Learning Log 3: Arrays & Data Structures
### a) Concept Implemented
I used Arrays to bundle related sets of elements together toether. We applied this both to PImage[] arrays for my character's sprite animations, and to object arrays like Deepling[] and Aspid[] to store and manage multiple active enemies on the screen.

```java
// Examples of image sprite arrays and custom class data structures
PImage walking[] = new PImage[6];
PImage aspid[] = new PImage[4];

Deepling[] Deepling; // Object arrays that hold multiple active enemies
Aspid[] Aspid;
```

### b) Application & Purpose
Using arrays was necessary to keep my project structured.

PImage[]: For Bill Nylon’s movement animations, instead of creating individual variables like walk1, walk2, and walk3, we put them all inside the walking[] array. By tracking an integer index pointer called frame, I can easily swap the display image using a single command: image(walking[frame], SX, SY).

### c) Challenges & Fixes
Challenge 1: NullPointerException

The Problem: When I first tried to loop through my enemy array setups inside the main draw() loop, my program would instantly crash with a NullPointerException error on specific levels. This happened because on simple levels, I hadn't instantiated any flying enemies (Aspid = new Aspid[0]), so the loop tried to read the length of a completely empty null reference.

The Fix: I resolved this by adding a  validation block before running the loops. By wrapping the array processors inside a quick selection check—if(Aspid != null)—the program safely ignores the loop block if the array structure hasn't been instantiated for that layout yet.

## Learning Log 5: Custom Functions & Error Checking/Restrictions

### a) Concept Implemented
I implemented **Custom Functions** to bundle repetitive tasks into reusable blocks, and used **Error Checking and Restrictions** through conditional structures to keep variables inside safe visual limits.

```java
// Definition of a custom function that uses restriction math inside it
void drawExitSign(int x, int y, int w, int h) {
  rectMode(CORNER);
  rect(x, y, w, h);
  
  // Restriction formula keeping the scanline strictly inside the box height
  int scanLineY = y + 6 + (frameCount % (h - 16));
  line(x + 10, scanLineY, x + w - 10, scanLineY);
}
```
### b) Application & Purpose

Custom Functions: Instead of jamming all my level geometry directly into the main drawing loop, I built custom functions like drawExitSign(), drawLabBackground(), and drawMapForegroundLevel1(). This lets me clean up the program layout. I can draw a complete, fully animated exit sign anywhere on the map with a single line of code by passing in custom parameters, like drawExitSign(900, 200, 100, 50);.

Data and Movement Restrictions: Inside my drawing routines, I used mathematical restrictions to keep visual components under control. For example, the moving scanline inside the exit sign needs to cycle continuously without escaping the box. By using a modulo statement tied to the box height (frameCount % (h - 16)), I restricted the coordinate data so it rolls over automatically.

### c) Challenges & Fixes
Challenge 1: rectMode Conflicts

The Problem: Inside my custom function drawMenuButton(), I set the environment state to rectMode(CENTER). Because custom functions modify Processing's shared global canvas state, this setting leaked out when the function finished executing. When the program went to draw my level layouts using functions like drawLabBackground(), every single wall, liquid tank, and platform layer rendered warped, shifted, and completely broken because they rely on rectMode(CORNERS).

The Fix: I fixed this by adding strict reset checkpoints. At the very end of my button function, I forced a reset with rectMode(CORNER);, and at the absolute top of my foreground functions, I explicitly overrode the state by calling rectMode(CORNERS);. This ensures the rendering configurations stay restricted to their own specific functions.

Challenge 2: Hardcoded Local Constraints 

The Problem: When creating the restriction formula for the moving scanline inside drawExitSign(), I originally subtracted a fixed number from the frame math (like frameCount % 34). This worked fine for the first sign, but when I called the custom function again elsewhere with a much taller height parameter (h), the scanline would get stuck looping near the top of the sign, leaving the bottom half completely blank.

The Fix: I fixed this by replacing the hardcoded number with a dynamic restriction expression based on the function's parameter variables: frameCount % (h - 16). By tying the math directly to the height parameter (h), the restriction boundary automatically scales itself to fit whatever dimensions are passed into the function.
