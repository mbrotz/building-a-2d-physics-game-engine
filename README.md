# Overview

**Chapter 1: Getting Started with HTML5 Canvas**

*   **Abstract:** This chapter introduces the foundational setup for the physics engine, focusing on creating the canvas element and providing basic drawing functionalities. The goal is to establish a visible drawing area and allow for simple user interaction.
*   **Chapter 1.1: Initial Canvas Setup**
    *   `Core.js`:
        *   Initializes the `gEngine` namespace or avoids re-initialization if it exists.
        *   Retrieves the HTML5 canvas element with the ID `canvas`.
        *   Obtains the 2D rendering context for the canvas.
        *   Sets the canvas dimensions (width to 800 and height to 450).
        *   Provides access to canvas width, height, and 2D context via the `mPublic` object.
    *   `index.html`:
        *   Sets up the basic HTML document structure.
        *   Includes the canvas element with the ID `canvas`.
        *   Links to the `UserControl.js` and `Core.js` JavaScript files.
    *   `UserControl.js`:
        *   Adds a `userControl` function which handles keyboard events to draw shapes on the canvas.
        *   Uses `keycode` of 70 to draw a rectangle and 71 to draw a circle at random positions on the canvas.
        *   Randomizes position, width, and height (for rectangles), and position and radius (for circles) when drawing.

**Chapter 2: Implementing Rigid Shapes**

*   **Abstract:** This chapter focuses on creating the basic building blocks of a 2D physics engine: rigid shapes. It introduces the concept of vector mathematics and defines classes to represent circles and rectangles, laying the groundwork for simulating their behavior. The chapter also implements a basic game loop and user interface.
*   **Chapter 2.1: Creating Rigid Shapes**
    *   `Core.js`:
        *   Adds an empty array (`mAllObjects`) to store all the rigid body objects.
        *   Introduces an `updateUIEcho` function to dynamically update information in the HTML about selected objects.
        *   Creates a `draw` function to clear the canvas and draw all objects in the `mAllObjects` array.
        *   Implements a basic game loop using `requestAnimationFrame` for rendering, UI updates and object drawing.
        *   Provides access to the `mAllObjects`, width, height, and 2D context, and initializes the game loop.
    *   `Vec2.js`:
        *   Defines a `Vec2` class for 2D vector representation.
        *   Implements basic vector operations like `length`, `add`, `subtract`, `scale`, `dot`, `cross`, `rotate`, `normalize`, and `distance`.
    *   `index.html`:
        *   Adds a `div` with id `uiEchoString` to display object information and instructions.
        *   Loads new script files: `Vec2.js`, `RigidShape.js`, `Circle.js`, `Rectangle.js`, and `MyGame.js`.
    *   `MyGame.js`:
        *   Creates four `Rectangle` objects which are located at the screen borders to serve as static boundaries in the scene.
    *   `RigidShape.js`:
        *   Defines a `RigidShape` base class for all rigid bodies.
        *   Manages the center position (`mCenter`) of the rigid body.
        *   Adds each instance of a `RigidShape` to the `mAllObjects` array.
    *   `Circle.js`:
        *   Defines a `Circle` class that inherits from `RigidShape`.
        *   Adds a `mRadius` property to store the circle's radius.
        *   Adds a `mStartpoint` property which will be used for drawing a line from the circle's edge to the center.
        *   Implements a `draw` function to draw a circle to the canvas using the canvas context.
    *   `Rectangle.js`:
        *   Defines a `Rectangle` class that inherits from `RigidShape`.
        *   Adds `mWidth` and `mHeight` properties to store the dimensions of the rectangle.
        *   Calculates the initial vertices (`mVertex`) and face normals (`mFaceNormal`) of the rectangle.
        *   Implements a `draw` function to draw the rectangle to the canvas, utilizing transformations for rotation.
    *   `UserControl.js`:
        *   Implements the user interaction logic with `userControl`.
        *   Adds object selection via numerical keys (0-9) or up/down arrow keys.
        *   Adds functionality to spawn rectangle (f) or circle (g) objects at random locations using mouse clicks.

*   **Chapter 2.2: Core Engine Loop**
    *   `Abstract`: This section focuses on implementing the core engine loop responsible for managing time, updating the game state, and rendering graphics at consistent framerates. It introduces the concept of a fixed timestep and basic gravity simulation.
    *   `Core.js`:
        *   Implements a fixed-time-step game loop with a target of 60 FPS using `requestAnimationFrame`.
        *   Calculates elapsed time (`mElapsedTime`) and manages lag time (`mLagTime`) to ensure consistent updates.
        *   Adds an `update` function that iterates over all objects in the `mAllObjects` array and calls their respective `update` functions.
        *   Updates UI information to display object angle, fixes objects, and reset options.
        *   Provides access to canvas, objects array, and update interval.
    *   `index.html`:
        *   Loads JavaScript files in the correct order to ensure proper execution.
    *   `MyGame.js`:
        *   Creates an initial rectangle object in the middle of the screen.
    *   `RigidShape.js`:
        *   Adds an `update` function to simulate a basic gravitational force on objects, making them move downwards if they're not fixed.
    *   `Circle.js`:
        *   Implements the `move` function for the `Circle` object using vector addition.
        *   Implements the `rotate` function for the `Circle` object.
    *   `Rectangle.js`:
        *   Implements the `rotate` function for the `Rectangle` object using matrix transformations.
        *   Implements the `move` function for the `Rectangle` object using vector addition.
    *   `UserControl.js`:
        *   Implements movement for the selected object using WASD keys.
        *   Implements rotation for the selected object using Q and E keys.
        *   Adds 'h' key to fix (or unfix) objects for gravity control.
        *   Adds 'r' key to reset the system and remove all the spawned rigid bodies.
        *   Adds more UI control elements to the control section.

**Chapter 3: Collision Detection**

*   **Abstract:** This chapter focuses on collision detection, which is crucial for any physics engine. It implements a broad-phase collision detection system to optimize performance, followed by more accurate collision tests for circles, rectangles and circle-rectangle pairs.
*   **Chapter 3.1: Broad Phase Collision Detection Method**
    *    `Core.js`:
        *   Changes the update logic so that collision detection occurs before calling the update function.
    *    `index.html`:
        *   Includes the `Physics.js` script.
    *   `Physics.js`:
        *   Implements a broad-phase collision detection system.
        *   Tests each object in the `mAllObjects` array for potential collisions based on bounding circles.
        *   Draws the outline of objects in green if their bounding circles overlap.
    *   `RigidShape.js`:
        *   Implements the `boundTest` function to test for overlapping bounding circles.
        *   Adds a `mBoundRadius` variable to store the radius of the object's bounding circle.
        *   Removes the gravity implementation.
    *   `Circle.js`:
        *   Adds the `mBoundRadius` property to store circle radius.
        *   Initializes the `mBoundRadius` with the circle's radius in the constructor.
    *   `Rectangle.js`:
        *   Adds the `mBoundRadius` property and initializes it using the rectangle's dimensions.
    *   `UserControl.js`:
        *   Makes the spawned objects move downwards, but not stop when hitting the ground.

*   **Chapter 3.2: Circle Collision Detection**
    *   `Physics.js`:
        *   Implements a more accurate circle-versus-circle collision detection using the `CollisionInfo` class.
        *   Adds the function `drawCollisionInfo` to visualize the collision normal.
    *   `index.html`:
        *   Includes the `CollisionInfo.js` script.
    *   `CollisionInfo.js`:
        *   Defines a `CollisionInfo` class to record collision information such as depth, normal, and start and end points.
        *   Includes setter and getter functions for depth and normal.
        *   Includes a function `setInfo` which set the depth, normal, start and end points.
        *   Includes a `changeDir` method to change the collision normal direction.
    *   `Circle.js`:
        *   Updates the `Circle` class to inherit from `RigidShape`.
    *   `Circle_collision.js`:
        *   Implements the `collisionTest` function to handle collisions with different objects.
        *   Implements the `collidedCircCirc` method to determine if two circles are colliding, compute collision information, and updates the collisionInfo object.
    *   `Rectangle.js`:
        *   Updates the `Rectangle` class to inherit from `RigidShape`.
    *   `Rectangle_collision.js`:
        *   Adds a stub for collision detection of rectangles with other objects.

*   **Chapter 3.3: Rectangle Collision Detection**
    *   `Rectangle_collision.js`:
        *   Implements a method `collidedRectRect` to determine if two rectangles collide using Separating Axis Theorem.
        *   Implements a helper function `findSupportPoint` which finds the support point on the other rectangle that is furthest in the given direction.
        *   Implements a helper function `findAxisLeastPenetration` to find the axis of least penetration for a given rectangle.

*   **Chapter 3.4: Rectangle-Circle Collision Detection**
    *   `Circle_collision.js`:
        *   Implements a new `collisionTest` method which calls `collidedCircCirc` if the other object is a circle, otherwise calls the other object's `collidedRectCirc` function.
    *   `Rectangle_collision.js`:
        *   Implements `collidedRectCirc` method for rectangle vs circle collision detection.
        *   Checks if the circle's center is outside of the rectangle or inside, then calculates the collision information.

**Chapter 4: Implementing Rigid Body Movements**

*   **Abstract:** This chapter introduces the concept of rigid body dynamics by including linear and angular motion, forces, friction, and restitution, and the implementation of positional correction and collision impulse to make the collisions react more realistically.
*   **Chapter 4.1: Rigid Shape Movements**
    *   `Core.js`:
        *   Adds a boolean variable `mMovement` to allow toggle of gravity on/off.
        *   Adds UI controls for linear and angular velocity, mass, friction and restitution.
        *   Adds a basic gravitational force for all rigid bodies using `mGravity`.
    *   `RigidShape.js`:
        *   Adds `mVelocity`, `mAngularVelocity`, `mInertia`, `mInvMass`, `mFriction` and `mRestitution` properties to all rigid bodies.
        *   Adds functions `updateMass`, `updateInertia` and `update` to the class.
        *   Implements a basic linear motion with gravitational pull.
    *   `Circle.js`:
        *   Adds `updateInertia` function for `Circle` objects.
    *   `Rectangle.js`:
        *   Adds `updateInertia` function for `Rectangle` objects.
    *   `UserControl.js`:
        *   Adds user controls for changing linear velocity, angular velocity, mass, friction, and restitution of objects.
        *   Adds functionality to excite all objects on the scene by giving them random velocities and toggles the simulation with `,` key.

*   **Chapter 4.2: Positional Correction**
    *   `Core.js`:
        *   Adds a UI element to toggle the positional correction and changes the movement to be a variable instead of a constant.
    *   `Physics.js`:
        *   Implements a function `positionalCorrection` to reduces the interpenetration between two colliding objects after they overlap, by moving the objects in opposite directions using the collision normal and collision depth.
    *   `UserControl.js`:
        *   Adds the "M" key as a control to enable or disable positional correction.

*   **Chapter 4.3: Collision Impulse**
    *   `Physics.js`:
        * Implements the `resolveCollision` method which calculates the collision impulse by computing relative velocity, and friction force to change the velocity of each object in the collision.

*   **Chapter 4.4: Angular Impulse**
    *   `Physics.js`:
        *   Extends `resolveCollision` function by including angular impulse in collision resolution using vectors, masses, angular velocities, and inertias of the colliding objects.

**Chapter 5: A Cool Demo**

*  **Abstract:** This chapter culminates in a demonstration of all the implemented features, showcasing a more dynamic and engaging environment using multiple objects.
*   **Chapter 5.1: A Cool Demo**
    *   `Core.js`:
        *   Sets movement on by default.
        *   Increases the strength of gravity.
        *   Removes some UI controls.
    *   `RigidShape.js`:
        *   Removes objects when they go off the screen, also remove the explicit call to `move` to let velocity changes affect the position.
    *   `MyGame.js`:
        *   Creates a set of static rectangles in the middle of the screen.
        *   Spawns some random moving rectangles and circles to generate a more dynamic demonstration.
    *   `UserControl.js`:
        *   Makes the spawned objects move downwards.
        *   Changes the key mapping so that the 'h' key excites all the objects by giving them random velocities.

# Disclaimer

The overview was generated with Google AI Studio using Gemini 2.0 Fast Experimental. It may or may not be accurate.

# Apress Source Code

This repository accompanies [*Building a 2D Physics Game Engine*](http://www.apress.com/9781484225820) by Michael Tanaya, Huaming Chen, Jebediah Pavleas, and Kelvin Sung (Apress, 2017).

[comment]: #cover

Download the files as a zip using the green button, or clone the repository to your machine using Git.

## Releases

Release v1.0 corresponds to the code in the published book, without corrections or updates.

## Contributions

See the file Contributing.md for more information on how you can contribute to this repository.
