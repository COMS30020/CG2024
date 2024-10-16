## Raytracing and Shadows
### <a href="https://www.ole.bris.ac.uk/webapps/blackboard/content/launchLink.jsp?course_id=_260093_1&tool_id=_5824_1&tool_type=TOOL&mode=cpview&mode=reset" target="_blank"> Weekly Briefing ![](../../resources/icons/briefing.png) </a>
### Task 1: Introduction
 <a href='01%20Introduction/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='01%20Introduction/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='01%20Introduction/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

As we have seen previously, rasterising is a fast and efficient approach to rendering a 3 dimensional scene. However it has limitations - there are a number of phenomenon that are difficult (or even impossible) to achieve using this approach. In particular, rendering "light" and "shadow" is not well suited to rasterisation. For this reason, we now turn our attention to an alternative form of rendering: _ray tracing_. Take a look at the slides, audio narration and animation linked to above in order to gain a high-level understanding of the operation of _ray tracing_. Once you have grasped the basic principles, move on to the practical tasks in the rest of this workbook.

It is advisable to continue working with your current project and augment it with additional ray tracing functions. This will not only allow you to switch between different rendering modes (wireframe, rasterised, ray-traced) but will also permit "hybrid" rendering - where different elements of a scene are rendered using different rendering approaches.  


**Hints & Tips:**  
The code that you will write for this workbook will be VERY resource intensive. If you find that your renderer is running too slowly to allow you to navigate the camera around the scene effectively, then you might like to try using the optimised `make speedy` build rule. Note however that this build rule uses "unsafe" mathematic optimisations - although fast, the resultant code can behave in unusual and predictable ways. If you encounter any unexpected behaviour, test your code using one of the _other_ build rules - the problem might not be your code, but the optimisations in the `speedy` build rule.

Ray tracing is a challenging activity that will exercise all your abilities as a programmer. Due to the nature of many of the tasks in this workbook, traditional approaches to debugging will typically prove ineffective. To aid you in your work, we have provided a <a href="../../Debugging" target="_blank">debugging guide</a> - the techniques outlined in this document will allow you to more effectively detect and isolate errors in your code.  


# 
### Task 2: Closest Intersection
 <a href='02%20Closest%20Intersection/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='02%20Closest%20Intersection/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='02%20Closest%20Intersection/audio/segment-2.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='02%20Closest%20Intersection/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a> <a href='02%20Closest%20Intersection/animation/segment-2.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

The first objective we need to achieve when attempting to perform ray tracing is to be able to detect _if_ (and more importantly _where_) a projected ray intersects with a model triangle. Watch the narrated slides and animations above to gain a theoretical understanding of how to perform this operation.

With the knowledge gained, write a function called `getClosestIntersection` that given:
- the _position_ of the camera in 3 dimensional space (represented as a `vec3`)
- the _direction_ of a ray being cast from the camera into the scene (also as a `vec3`)

will search through the all of the triangles in the current scene and return details of the _**closest**_ intersected triangle (if indeed there is an intersection !).

We appreciate that this is a complex task, so to help you achieve this, the code below is the C++ equivalent of the ray/triangle intersection equation that was shown in the slides above:  

``` cpp
glm::vec3 e0 = triangle.vertices[1] - triangle.vertices[0];
glm::vec3 e1 = triangle.vertices[2] - triangle.vertices[0];
glm::vec3 SPVector = cameraPosition - triangle.vertices[0];
glm::mat3 DEMatrix(-rayDirection, e0, e1);
glm::vec3 possibleSolution = glm::inverse(DEMatrix) * SPVector;
```

It is important to recognise that the `possibleSolution` calculated by the above code is NOT the `(x,y,z)` coordinates of a point in 3 dimensional space, but rather a three-element data structure that consists of:

- `t` the _absolute_ distance along the ray from the camera to the intersection point
- `u` the _proportional_ distance along the triangle's first edge that the intersection point occurs
- `v` the _proportional_ distance along the triangle's second edge that the intersection point occurs

Calculating the actual position of the intersection point in 3 dimensional space can be done in two different ways. Both approaches (described below) _should_ give the same location (once you transpose them into the same coordinate system !) In fact it might be a useful double-check to see if they agree on the same intersection point. These two approaches are:

- use the direction of the projected ray and distance `t` to find the intersection point relative to the camera _or_
- insert `u` and `v` into the formula below, to find the intersection point relative to the world origin


  


![](02%20Closest%20Intersection/images/formula.jpg)

**Hints & Tips:**  
Your function is going to need to return various details about the intersection point found. To help you deal with this, we have provided a class in the `libs/sdw` folder called `RayTriangleIntersection` that you might like to use to store and return all of the intersection details.

You should bear in mind that a single ray fired into a scene may well intersect with multiple different triangles (the front of the blue box, the back of the blue box, the back wall of the room etc). Remember that we are interested in the intersection that is _**closest**_ to the camera (i.e. has the smallest `t`).  


# 
### Task 3: Validating Intersections
 <a href='03%20Validating%20Intersections/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

We need to be a little bit careful when searching for ray/triangle intersections since the `getClosestIntersection` function can often return false-positive results ! The reason for this is that the formulae provided previously allow us to calculate the intersection between a line and a _plane_. We must however remember that a triangle is a discrete bounded segment of a plane. The code that you wrote in the previous task may well find solutions that are _on the same plane_ as the triangle, but not actually _within the bounds_ of the triangle itself (as illustrated by the X in the diagram below).

In order to solve this problem, we must validate the coordinates of any _potential_ intersection _before_ we can accept it as a solution. Watch the video linked to at the top of this task for an explanation of how this is achieved. Once you have grasped the basic principles, implement a validation check in your `getClosestIntersection` function so that it checks a **possible** solution to make sure it is valid _before_ returning it as an **actual** solution. If the _closest_ intersection is NOT valid, your function should then check the _next closest_ intersection to see if that is valid (and so on until it finds a valid intersection). You might like to rename your function to `getClosestValidIntersection` in order to provide a clearer indication of the operation of the code.

To help you in this task, integrate the following three tests into your code:

```
(u >= 0.0) && (u <= 1.0)
(v >= 0.0) && (v <= 1.0)
(u + v) <= 1.0
```

You should also check that the distance `t` from the camera to the intersection is positive - otherwise you may end up rendering triangles that are actually _behind_ the camera !
  


![](03%20Validating%20Intersections/images/outside-bounds.jpg)

**Hints & Tips:**  
Test your function by passing in some ray direction vectors with a _known_ intersection points (e.g. the ray that is fired through the dead centre of the image plane _should_ intersect with the front of the blue box in the test model). If you print out the colour of the closest valid intersected triangle, this should give you a rough idea of whether or not your function is working.

Think carefully about what your function should do if there is NO valid intersection to be found. It would be useful to return some kind of recognisable "no intersection found" value (so that the code calling your function can take appropriate action).  


# 
### Task 4: Ray Tracing the Scene
 <a href='04%20Ray%20Tracing%20the%20Scene/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='04%20Ray%20Tracing%20the%20Scene/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

Now that you have a function that can identify the closest valid intersection of a single ray, we can use this to render an entire scene ! Write a new `draw` function that renders the Cornell Box model using Ray Tracing. Don't throw away your old "rasterising" `draw` function - you are going to need this later. Instead you should rename it to something appropriate (such as `drawRasterisedScene`).

In your new `draw` function, loop through each pixel on the image plane (top-to-bottom, left-to-right), casting a ray from the camera, _through_ the current pixel and into the scene (remembering to normalise any direction vectors to avoid scaling effects). Using your previously written `getClosestValidIntersection` function, determine if the ray intersects with a triangle in the model. When a valid intersection has been identified, paint the image plane pixel with the colour of the closest intersected triangle. 

Remember that in this task, you are converting _*from*_ a 2D canvas pixel position _*into*_ a direction vector to fire into three dimensional space. In many ways this is the opposite of your previously written `getCanvasIntersectionPoint` function, where you converted _*from*_ a 3D vertex position _*into*_ a 2D canvas position.

When everything is working correctly, you should end up with a render that looks similar to the one shown in the diagram below.  


![](04%20Ray%20Tracing%20the%20Scene/images/basic-ray-tracing.jpg)

**Hints & Tips:**  
Your code needs to be careful and considerate when calling the `getClosestValidIntersection` function. Weird things can happen if the code calling this function assumes that a valid intersection is _always_ returned. Check to make sure it isn't returning a "no intersection found" value before attempting to colour a pixel.  


# 
### Task 5: Casting Shadows
 <a href='05%20Casting%20Shadows/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='05%20Casting%20Shadows/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

At this stage, it probably doesn't seem like you have made any progress - if anything, this is a _slower_ way of achieving the same results as with the rasteriser. In this next task (and in the tasks tackled next week as well), we will be doing things that are either _hard_ or _impossible_ to do with a rasteriser - so stick with it, it will all to be worth it in the end !

Shadows are a key element of 3D rendering that we have until this point not addressed. We see shadows all the time in the real world (just take a look around you now !). If we want our renders to look realistic, we are going to need to simulate them somehow. For this task, you are going to need a new `vec3` variable to hold the position of a single-point light source. A location in the middle of the room, above the origin, somewhere near the ceiling (but still inside the room) would seem a sensible place for it.

Determining whether or not a particular point on a surface should be drawn in shadow is conceptually relatively straight-forward. All we have to do is to ask ourselves the question: can a particular point "see" the light ? However, there are some additional complexities that we have to deal with when implementing a consistent shadow effect. Review the slides and audio narration above and then implement the concepts discussed in your Ray Tracer.

It is worth noting that your already-implemented `getClosestValidIntersection` function does a lot of the work required in order to check for the visibility of the light from a point on a surface. You may however need to invest a bit of time refactoring it to make it versatile enough to be used for this purpose.

Once you have implemented a shadow feature, your render of the Cornell Box should look something a little bit like the image below (depending on where you position your light source !). Note that your shadow may well appear a little bit "speckly" and you might have shadow pixels appearing where you wouldn't expect to see them. Don't worry, this is not unusual and is a common occurrence when first attempting to render shadows. In the following task we explore this phenomenon in more depth and introduce a technique to (hopefully) fix it.  


![](05%20Casting%20Shadows/images/without-ambient.jpg)

# 
### Task 6: Shadow Acne
 <a href='06%20Shadow%20Acne/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='06%20Shadow%20Acne/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='06%20Shadow%20Acne/audio/segment-2.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

When implementing your shadow you may notice that is has a "speckled" appearance and/or spills out of the area where the shadow should appear (as illustrated in the image below). This effect is known as "shadow acne" and it is a commonly encountered phenomenon in Ray Traced shadows. View the slides and audio narration linked to above for a discussion of the causes of this effect and an explanation of one possible solution to this problem. Once you have gained this understanding, add some additional code to your project to remove this phenomenon from your render.  


![](06%20Shadow%20Acne/images/center.jpg)

**Hints & Tips:**  
If you need to compare two triangles for equality, make sure you use their **index** values as the basis for comparison (i.e. their positions in the triangle vector/array). If you try to compare two instances of the `ModelTriangle` class using the `==` operator, you may inadvertently be testing the memory address of those variables (rather than checking to see if those two variables actually hold the same triangle !)

The `RayTriangleIntersection` class that was provided for you has an attribute called `triangleIndex` that can be used to store the index number of a triangle being intersected. Note that you'll have to fill this attribute yourself with a suitable value when your `getClosestValidIntersection` function identifies a valid intersection.   


# 
### Task 7: Interactive Renderer Switching


As a final task in this workbook, add some additional key event handlers to your program that allows the user to interactively switch between the three main modes of rendering:

- Wireframe: Scene drawn using simple stroked/outline triangles
- Rasterised: Scene drawn using filled rasterised triangles
- Ray Traced: Scene drawn using filled ray traced triangles

Being able to switch between these different modes will prove very useful later on in the unit when attempting manual interactive testing: it will allow you to navigate the camera around the scene using the FAST wireframe or rasterised renderers. Then, once the camera is in the desired position, you can switch to the (much slower) Ray Traced renderer to view the fully lit scene from the current camera position.  


# 
### End of workbook
