## Lighting and Shading
### <a href="https://www.ole.bris.ac.uk/webapps/blackboard/content/launchLink.jsp?course_id=_260093_1&tool_id=_5824_1&tool_type=TOOL&mode=cpview&mode=reset" target="_blank"> Weekly Briefing ![](../../resources/icons/briefing.png) </a>
### Task 1: Introduction
 <a href='01%20Introduction/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='01%20Introduction/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='01%20Introduction/audio/segment-2.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='01%20Introduction/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

In this workbook we explore some of the more advanced aspects of model rendering. In particular, we will focus on the use of various lighting and shading effects to make our renders look more realistic and aesthetically appealing.

In the slides and audio narration above, we introduce three distinct types of light as a convenient _approximation_ to real-world lighting:

- Diffuse lighting: "general" surface lighting (which we later decomposed into Proximity and Angle of Incidence)
- Specular highlighting: a special effect to give objects the appearance of a "glossy" or "shiny" surface
- Ambient lighting: background lighting as an approximation to "global illumination"

To aid your understanding of this topic, the animation linked to above provides an illustration of the appearance and effect of each type of light. The following sections explore the implementation of these different types of light in more detail. It is important however that you have an understanding of the overall concepts before moving on to the practical tasks in this workbook.  


# 
### Task 2: Proximity Lighting
 <a href='02%20Proximity%20Lighting/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='02%20Proximity%20Lighting/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='02%20Proximity%20Lighting/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

Proximity lighting takes into account the distance of a surface from a light source. Intuitively, the closer a point on a surface is to a light, the brighter it will appear. Take a look at the slides, audio commentary and animation above to gain an understanding of how light dissipates and an appreciation of how we might go about calculating this effect.

Once you have an understanding of the concepts in theory, add code to your renderer that will adjust the brightness of each pixel to take into account proximity of the surface to the light source. Your calculated brightness values should be within the range 0.0 (total darkness) to 1.0 (fully illuminated). These values can then be used as a multiplier to adjust the brightness of a particular pixel. Simply multiply each of the three colour channels (RGB) by the brightness float, before you pack them into the final 32 bit pixel colour.

Your algorithm should be _based_ on the 1/4&pi;r<sup>2</sup> approach outlined in the slides - however, feel free to experiment with different multipliers (e.g. 1/3&pi;r<sup>2</sup>, 1/5&pi;r<sup>2</sup>, 2/7&pi;r<sup>2</sup> etc.) in order to produce an effect that you think looks good (this is were aesthetics rule over science !)  


**Hints & Tips:**  
In order to store the position of your light, you should add a new `vec3` variable to your code to keep track of the coordinates of the light in 3 dimensional space.

To find the distance between the light and a point on a triangle, you might like to make use of the `length` function from the GLM library - this will give you the magnitude of a `vec3` variable. Make sure you don't normalise your `vec3` in this task - otherwise you'll always get a length of 1 (remember that in this situation, we _need_ the distance !).

To help you test this form of lighting and debug your code, you may wish to add some additional key event handlers that allow you to interactively shift the position of your light source around the model. Due to the fact that Ray Tracing is a computationally intensive activity, you will probably find that moving the light is a slow and laborious process - this is where the ability to switch between the different types of rendering becomes really useful !  


# 
### Task 3: Angle of Incidence Lighting
 <a href='03%20Angle%20of%20Incidence%20Lighting/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='03%20Angle%20of%20Incidence%20Lighting/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='03%20Angle%20of%20Incidence%20Lighting/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a> <a href='03%20Angle%20of%20Incidence%20Lighting/animation/segment-2.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

Your next task is to implement "Angle-of-Incidence" lighting. This lights a surface by considering the _orientation_ of that surface relative to the position of a light source. View the two animations linked to above for a high-level explanation of "Angle-of-Incidence" lighting.

In order to implement this kind of lighting, we must first calculate a "normal" for each triangle (a vector that is perpendicular to the surface of the triangle). View the slides and audio narration above to find out how to calculate the normal for a particular surface. Using the knowledge gained, calculate the normal of each triangle in the Cornell Box model. To help achieve this, you might like to use the GLM `cross` function to calculate the cross product of two edges of a triangle.

Note that you can safely calculate and store the normals of all triangles when you first load in the OBJ geometry file (since these normals will not change during the operation of the renderer). It is worth noting that the `ModelTriangle` class has a `normal` attribute that can be used to store the calculated normal for each triangle. 

Once the surface normals have been calculated, you can then use these to calculate the angle of incidence of light falling onto the surfaces. This is done by taking the dot product of the normal and light direction vectors (as shown in the slides and audio narration above). To calculate dot products, you might like to use the `dot` function provided by GLM.

Once calculated, use these angles of incidence to adjust the brightness of each pixel (in a similar way to that which you did with proximity in the previous task). You should end up with a render that looks like the image shown below. Note that we have removed the shadow feature from this render in order to more clearly illustrate the appearance of diffuse lighting. If you have implemented event handlers to move the position of your light point, use this feature to test that your lighting looks realistic from a number of different light positions.


  


![](03%20Angle%20of%20Incidence%20Lighting/images/diffuse-lighting.png)

**Hints & Tips:**  
Note that we need to be a little bit careful - each triangle actually has TWO normals (the front face and the back face !). Calculating the _correct_ normal requires us to pick the appropriate two triangle edges to cross product. However _which_ two edges we use will depend on the "winding order" used when the OBJ model was created. View <a href="resources/winding.mp4" target="_blank">this video</a> for an explanation of the concept of "winding order". You won't always know which winding was used when creating a particular model - the best advice is to just to pick two edges and try it. You will soon know if you were wrong (all your surfaces will be dark) so you can then just switch to using alternative edges. This might seem like trial and error, but this is often the way with exploratory rendering !

Note that when considering the angle that the light hits the surface...  
you **MUST** use: _the direction vector of the LIGHT from the POINT ON THE SURFACE_  
and **NOT**: _the direction vector of the POINT ON THE SURFACE from the LIGHT_  
If you use the wrong one, you will end up lighting the back surface of your triangles !
  


# 
### Task 4: Specular Lighting
 <a href='04%20Specular%20Lighting/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='04%20Specular%20Lighting/slides/segment-2.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='04%20Specular%20Lighting/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='04%20Specular%20Lighting/audio/segment-2.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

Your next task is to implement a "Specular" lighting effect (as illustrated in the introductory animation to this workbook). The slides and audio narration in this current section provide a description of specular light and how to calculate the brightness of points on a specularly illuminated surface.

This process involves calculating the outgoing angle of reflection of an incoming beam from the light. This in itself is more complex than you might initially think. For this reason, we have included an additional slide and audio narration in order to help you calculate this particular element. View these now if you haven't done so already. As with the previous tasks, you should again make use of the GLM `dot` function, as well as the surfaces normals that you calculated previously. 

The image below provides some examples of the kinds of specular highlighting effect you might expect to see for different exponent values. You should use a relatively large exponent in the calculation of specular spread (for example 256). This will ensure that you get a tightly focused spot of light, which will be easier to see than a specular highlight with a wider spread. It can be difficult to distinguish between the various different lighting effects if the parameters used are too subtle (for example, proximity lighting can look a lot like a specular spot with a wide spread !). You might even like to switch off the other forms of lighting in your code so that you can clearly see the specular effect.  


![](04%20Specular%20Lighting/images/various-exponents.jpg)

**Hints & Tips:**  
Note that you may need to move the light and/or camera to an appropriate location in the scene in order to be able to see the specular highlight effect. Remember that specular lighting represents the reflection of the light source, so the camera needs to be in a position where the reflection should be visible.  


# 
### Task 5: Ambient Lighting
 <a href='05%20Ambient%20Lighting/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='05%20Ambient%20Lighting/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

In the real world, light bounces off objects (potentially multiple times) in order to illuminate "hard to reach" areas of a model (even areas that are in shadow). View the slides and audio narration for more insight into this notion of "Indirect Illumination". As you might imagine, calculating this kind of lighting (which would involve multiple bounces) would be computationally very expensive. It is possible however to implement a plausible approximation to such lighting with very little additional effort (both in terms of computational power and lines of code ;o)

One approach to creating such "ambient" lighting is to implement a "minimum threshold" as illustrated in the image below. Compare the shadow regions of this image with those of the previous week's workbook (which included no ambient lighting). This involves the use of an IF statement (or call to the `max` function) to ensure that the brightness of any point never falls below a certain threshold level.  


![](05%20Ambient%20Lighting/images/shadows-with-ambient.png)

# 
### Task 6: Gouraud Shading
 <a href='06%20Gouraud%20Shading/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='06%20Gouraud%20Shading/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='06%20Gouraud%20Shading/audio/segment-2.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='06%20Gouraud%20Shading/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

We finish this workbook by considering some rendering enhancements that will improve the aesthetics of your renders, with only a minimal additional performance overhead.

Due to the fact that we represent models as a series of triangles, any artefacts within a model will have a finite resolution. When rendering a model (unless we have a huge number of triangles) the surfaces may well appear "blocky" and "low-res". But what if we could shade triangles more "intelligently" and blend their edges together ? That would "smooth off" the sharp corners and provide an _apparently_ higher resolution final rendering !

_Gouraud shading_ in one such intelligent approach to surface rendering. View the slides and audio narration above to gain an understanding of the principles of Gouraud shading and then using this knowledge, implement this approach in your renderer. In order to achieve this, you will need to make use of _Barycentric coordinates_ to interpolate vertex brightnesses across the face of each triangle. The operation of Barycentric coordinates is illustrated in the animation at the top of this section.

The image below illustrates the smoothing effect that can be achieved using Gouraud shading - it is important to note that both spheres shown in the diagram have the _same_ geometry (the same number of triangles). The surfaces in the sphere on the left are shaded with a flat colour, whereas the sphere on the right is rendered using Gouraud shading. Clearly the right hand sphere _looks_ a lot smoother !  


![](06%20Gouraud%20Shading/images/gouraud.jpg)

**Hints & Tips:**  
One of the trickiest parts of this task is figuring out _which_ weighting to apply to _which_ vertex brightness. Watch the Barycentric coordinates animation closely in order to work out which vertices you need to apply `u`, `v` and `w` to in order to calculate the correctly weighted brightness.

The Cornell Box isn't the most useful model for testing Gouraud shading (it's hard to "smooth off" the edges of a cube !). For this reason, we have provided a simple <a href="resources/sphere.obj" target="_blank">additional OBJ model</a> for you to use when testing your code.  


# 
### Task 7: Phong Shading
 <a href='07%20Phong%20Shading/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='07%20Phong%20Shading/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

Gouraud shading is fine, but it only goes so far in creating smoothly rendered model surfaces - if you look closely at the image of the red sphere, you can see a bright cross shape that marks the boundaries of the surface faces. _Phong shading_ is an even more sophisticated approach that takes shading one step further (although it can only achieve this at the cost of additional computation !). Take a look at the slides and audio narration above to gain an understanding of the operation of Phong Shading. Adapt your renderer to make use of this more sophisticated approach.

Note that the audio narration makes reference to the concept of "bump maps" (which you may wish to explore in the coursework assignment - if you are doing the coursework variant of this unit). We mention bump maps here in order to link this concept into the discussion on Phong shading (in case you need it later).

The diagram below illustrates the different results of flat, Gouraud and Phong shading. Again, all three spheres have exactly the same geometry and number of triangles. It is just the alternative rendering approaches that make them appear different.  


![](07%20Phong%20Shading/images/phong.jpg)

# 
### End of workbook
