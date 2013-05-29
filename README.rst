Java 2D Physics Engine
======================

A simple physics engine written in Java.

Background
----------

I initially started writing code for the purpose of tying together my knowledge on topics I learned throughout this semester in my Computer Science class. As I programmed, I continued to add more and more functionality until I just decided to thumb through my Physics textbook and just make a simple 2D physics simulation.

About this project
------------------

**See it in action!**

`Project after 16 hrs <http://www.youtube.com/watch?v=NFiMOjA8Vcc>`_
`Project after 9 hrs <http://www.youtube.com/watch?v=TlX4m44eFzM>`_

**Project brief:**

This program is a simple physics simulation in two dimensions. It revolves around the kinematic equations for calculating position and velocity based off of acceleration. Physics calculations and animations are handled in separate threads. Each object in the simulation maintains an ArrayList of accelerations acting on it. Accelerations can be added to each object individually, or in the case of constant accelerations such as gravity, they can be applied to all objects in the simulation. With each cycle of the physics engine, accelerations are summed for each object, and the resulting total acceleration vector is returned. The time constant, which is (milliseconds elapsed since last cycle/1000), is used as "t" in the kinematic equations for velocity. After velocity is calculated, the position for each object is updated based on (velocity * time constant).

**A note about resolving collisions:**

When a collision is detected, the first thing that is calculated is the angle of collision. We then examine the x and y components of velocity for each object in a coordinate system tilted to match the angle of collision (meaning that the objects lie upon the x-axis). This way, we can isolate the x- and y-velocity components, and apply simple equations for elastic collisions in one dimension along the x-axis. After applying the equations, we now have the new x-components of velocity for each object (remember, the y-components are unchanged, since we've tilted our coordinate system). By tilting the coordinate system back to its original position, we can obtain the x- and y-components of velocity for each object in screen coordinates.

The above procedure will provide the needed resulting velocity for each object, but there's one more step. Since there is a time delay between each cycle of the physics engine (remember that our time constant is a measure of how long it has been since the last cycle), it is likely that a collision also means that two objects have overlapped. The way we resolve this is to first calculate the following:


``Minimum Distance - The minimum distance allowed between the two objects. This is the sum of the two objects' radii.``
``Distance Between - This is the current distance between the centers of the objects.``
``Overlap Distance - This is (Minimum Distance - Distance Between).``
``Distance to Move - (Overlap Distance / 2). This is the amount of distance each needs to move in the opposite direction from the other in order to restore a "touching" scenario, as opposed to an "overlapping" scenario.``

So now we have Distance to Move, a magnitude in two dimensions, and we know the collision angle. The way the angle is calculated is such that the current object is on the x-axis to the left, and the other object is on the x-axis to the right. We simply move the current object in the negative direction along the collision coordinate system's x-axis by a magnitude of (-Distance to Move), and the other object along the same axis by a magnitude of (+Distance to Move). We now have not only the correct velocity, but the corrected position as well.