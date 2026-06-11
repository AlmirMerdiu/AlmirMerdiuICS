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

b) Application & Purpose

Different variable types were needed to handle specific tasks within program:

color (Theme): Stores color data globally. This keeps the it consistent and lets me update the game's entire color palette by changing a single line of code.

boolean (Tracking): States if its a simple true or false based on mouse coordinates. It acts as a trigger to tell buttons exactly when to highlight themselves.

float (Animations): Decimal numbers track positions for moving visual. This prevents the choppy rendering that happens when using rigid, whole integers for continuous movement.

int : Whole numbers calculate pixel-perfect grid locations on the screen, preventing decimal rounding errors during screen math.

c) Challenges & Fixes
Challenge 1: Global Variable Overwriting

The Issue: I tried using one global boolean variable for isHovering inside the main draw() loop. Because the program draws buttons one after the other, the last button always overwrote the variable. This meant only the bottom button changed colors when hovered, while the others did nothing.

The Resolution: I moved the variable out of the global scope. Instead, I made isHovering a local variable inside the custom drawMenuButton() function. This separate calculation isolates the math for each button so they work independently.

Challenge 2: Integer Division Errors

The Issue: When trying to position buttons evenly across the screen, I used fractions like 1/3 and 2/3 with my int variables. In programming, dividing integers like 1/3 rounds straight down to 0. This made my layout coordinates multiply by zero, causing the text to disappear off-screen.

The Resolution: I fixed this by using decimals (float) like 0.33 instead of fractions. I then converted the final calculation back into a whole number using int() so the pixels lined up perfectly.

