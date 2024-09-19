## Pixels and Interpolation
### <a href="https://www.ole.bris.ac.uk/webapps/blackboard/content/launchLink.jsp?course_id=_260093_1&tool_id=_5824_1&tool_type=TOOL&mode=cpview&mode=reset" target="_blank"> Weekly Briefing ![](../../resources/icons/briefing.png) </a>
### Task 1: Introduction


Before commencing with this week's workbook, make sure you have first watched the "Weekly Briefing" linked to above to gain an overview of the practical tasks involved. Note that you will will need to be <a href="https://www.ole.bris.ac.uk/auth-saml/saml/login?apId=_183_1&amp;redirectUrl=https%3A%2F%2Fwww.ole.bris.ac.uk" target="_blank">logged into Blackboard</a> already in order for the link to the weekly briefing video to work (because, you know - Blackboard).

This week we will take a look at the fundamentals of computer graphics: basic drawing and pixel manipulation. Everything that we cover this week will be in 2 dimensions. In future weeks we will move on to begin working in 3D, it is however important that we walk before we try to run !

You will hopefully find this workbook fairly straight-forward and should be able to complete it relatively quickly. This is intended to be a gentle week in order to provide some flexibility in the workload - in case you are still having difficulties installing SDL2, compiling and running your code. Next week the workbooks start to get more challenging !  


# 
### Task 2: Single Element Numerical Interpolation


Interpolation (informally: "filling in the gaps between known values") is an essential operation in Computer Graphics. We will use this operation numerous times throughout this unit (for various different purposes). It is essential that we become familiar with the basic principles - we are going to need to interpolate some pretty complex constructs later on !

In your `RedNoise` project, add a new function called `interpolateSingleFloats` that takes 3 parameters:
- `from` - a floating point number to start from
- `to` - a floating point number to go up to
- `numberOfValues` - the number of steps required from the start to the end

This function should return an **evenly spaced** list of size `numberOfValues` that contains floating point numbers between `from` and `to`. In order to store this list we will make use the `vector` dynamical data structure from the `std` library. By way of example, if we call our new function like so: `interpolateSingleFloats(2.2, 8.5, 7)` it should return a vector containing the following values: `2.2, 3.25, 4.3, 5.35, 6.4, 7.45, 8.5`.

Once written, test out your function to make sure it correctly returns the values from the above example. You can do this by adding the following code to your project's main function:

``` cpp
std::vector<float> result;
result = interpolateSingleFloats(2.2, 8.5, 7);
for(size_t i=0; i<result.size(); i++) std::cout << result[i] << " ";
std::cout << std::endl;
```

Note that `std::cout` refers to the character output object of the standard library and that `std::endl` is a flushed newline. It is also worth highlighting that `size_t` is an unsigned integer type provided by C++ that is useful for representing the size of things (such as vectors).  


**Hints & Tips:**  
You might like to take a look at 
<a href="https://www.tutorialspoint.com/cpp_standard_library/cpp_vector_push_back.htm" target="_blank">this documentation</a> on C++ vectors in order to get you started.  


# 
### Task 3: Single Dimension Greyscale Interpolation
 <a href='03%20Single%20Dimension%20Greyscale%20Interpolation/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='03%20Single%20Dimension%20Greyscale%20Interpolation/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

Interpolating numbers is fine, but this unit is supposed to be about graphics - so let's draw something ! Using the floating point interpolation function that you wrote in the previous task, draw a greyscale gradient across the `DrawingWindow`, from left to right (as shown in the image below). 

For each pixel in the window, you will need to create a suitable colour by packing RGB channel data into a `uint32_t` (32 bit integer) variable. To find out more about about pixels and pixel colour packing, take a look at the slides and audio narration for this section (the blue buttons above).  


![](03%20Single%20Dimension%20Greyscale%20Interpolation/images/grey-scale.jpg)

**Hints & Tips:**  
You might have to think a bit to figure out how to adapt the pixel looping code inside the `draw` function of `RedNoise` in order to achieve the above gradient. Don't worry about the transparency/alpha channel of the pixels - just set it to 255 (fully opaque) as we did in the original `RedNoise` template.  


# 
### Task 4: Three Element Numerical Interpolation


The previous task was relatively straight-forward - now let's try something a little more complex.
Write a new function called `interpolateThreeElementValues` that calculates a vector of interpolated values. This time however, rather than just single floating point values, this new function should operate on three-element compound data types (specifically, instances of the <a href="https://en.wikibooks.org/wiki/GLSL_Programming/Vector_and_Matrix_Operations" target="_blank">vec3</a> class provided by GLM). If we have a function that can interpolate these types, then we can deal with a wide range of entities including colours, 3D coordinates, directions (in fact anything with 3 numerical elements !)

By way of example, if we have the following two `vec3` variables:
``` cpp
glm::vec3 from(1.0, 4.0, 9.2);
glm::vec3 to(4.0, 1.0, 9.8);
```
And pass them into our new interpolation function (along with a `numberOfValues` of 4), then we should get the following results back:

``` cpp
(1.0, 4.0, 9.2)
(2.0, 3.0, 9.4)
(3.0, 2.0, 9.6)
(4.0, 1.0, 9.8)
```

Write your function and then test it with the above values in order to check that it operates as intended.  


**Hints & Tips:**  
In this task we encounter a slightly annoying and potentially confusing naming problem - two different types of vector. It is important to distinguish between the following:
- `vector` (from the `std` library) which we use as a dynamic list structure
- `vec3` (from the `glm` library) which we use to store 3 element entities

In order to use the `vec3` class, you will need to import the GLM header file with `#include <glm/glm.hpp>`. Note that you don't need to install the GLM library - this is already embedded within the `RedNoise` template (it's in the `libs` folder, if you are interested).

It is wise to explicitly reference the package when using classes and methods (e.g. `glm::vec3`). You might see examples of C++ code that import entire namespaces with `using namespace` (so that you don't keep having to specify the `glm::` bit). In this unit, it is wise to avoid importing entire namespaces - there are some duplicate method names in some of the packages we use later on, which can lead to confusion and ambiguity.  


# 
### Task 5: Two Dimensional Colour Interpolation
 <a href='05%20Two%20Dimensional%20Colour%20Interpolation/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

In the earlier grayscale interpolation task, we interpolated a single floating point value in just one dimension (the x axis). In this next task we will interpolate three-element float values (RGB colours) in _two_ dimensions (x _and_ y). Click on the blue button above to view an animation (and audio narration) that provides more details about interpolating RGB colour values. Once you are confident with the concepts covered, implement 2D RGB interpolation in order to produce the effect shown in the image below. 

To help you in this task, you can use the `interpolateThreeElementValues` function that you wrote in the previous task. It might seem a bit strange to use a vector (`vec3`) to store and manipulate a colour (rather than using a specifically designed `Colour` class). However, since you have already implemented a `vec3` interpolation function, we might as well make use of that. Don't worry though - we will introduce a more appropriate `Colour` class later on in this unit.
  


![](05%20Two%20Dimensional%20Colour%20Interpolation/images/colour-spectrum.jpg)

**Hints & Tips:**  
A good way to tackle this problem is to start by creating variables for the four corners of the window and initialising them with the following primary colours:

``` cpp
glm::vec3 topLeft(255, 0, 0);        // red 
glm::vec3 topRight(0, 0, 255);       // blue 
glm::vec3 bottomRight(0, 255, 0);    // green 
glm::vec3 bottomLeft(255, 255, 0);   // yellow
```

Then, using your previously written `vec3` interpolation function to help:
1. Calculate the colour of all the pixels in the first (left-most) column of the window
2. Calculate the colour of all the pixels in the last (right-most) column of the window
3. For each row, calculate the colour of each pixel in that row by interpolating between the first and last pixels  


# 
### End of workbook
