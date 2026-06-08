Entry Log #1
Discuss the types of variables you used, why different variables were needed.
--------------------------------------------------------------------------------------------------
// Snippet of variables used
color WALL_DARK_BLUE = color(25, 38, 52);
color NEON_CYAN      = color(0, 240, 255);
color HAZARD_ORANGE  = color(255, 120, 0);

boolean isHovering = (mouseX > x - 150 && mouseX < x + 150 && mouseY > y - 35 && mouseY < y + 35);
float rightBubble1 = 500 - (frameCount % 400);

To build an fun user experience in Quantum Blink, our group used multiple variable types that were necessary to perform roles within the program:

color Data Type (Theme Consistency): Variables like WALL_DARK_BLUE and NEON_CYAN were needed to store specific hexadecimal color data globally. Using a named variable instead of hardcoded RGB numbers ensures visual harmony across the entire menu and gameplay environment. If I want to change the aesthetic of the facility later, I can update a single global variable rather than tracking down hundreds of isolated color commands.

boolean Data Type (Conditional State Tracking): The isHovering variable evaluates whether a conditional statement is true or false based on the player's realtime cursor coordinates (mouseX and mouseY). This data type is crucial because it acts as a binary trigger to let the button know exactly when it needs to shift colors or highlight itself.

float Data Type (Smooth Animated Physics): Floating-point decimal numbers like rightBubble1 were used to track positions that require smooth, continuous movement over time. Because animations rely on mathematical steps and frame divisions, using a decimal-friendly type prevents choppy rendering gaps that usually occur with rigid whole numbers.

int Data Type (Discrete System States): Whole integer tracking variables (such as coordinate arguments int x and int y passed to buttons) are needed to calculate pixel-perfect grid locations across the screen without losing mathematical precision to decimal rounding errors.

