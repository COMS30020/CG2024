## Coursework Advanced Topics
### <a href="https://www.ole.bris.ac.uk/webapps/blackboard/content/launchLink.jsp?course_id=_260093_1&tool_id=_5824_1&tool_type=TOOL&mode=cpview&mode=reset" target="_blank"> Weekly Briefing ![](../../resources/icons/briefing.png) </a>
### Task 1: Introduction


This workbook does not contain any explicit practical tasks for you to complete. Rather, it provides a "playlist" of slides, audio narrations and animations of advanced rendering topics. Some of these techniques you may wish to implement in your rendering engine, others you may wish only to gain an appreciation of.

If you are taking the coursework assessed variant of this unit, implementing a selection of these in your final render _should_ result in a higher grade (depending on how well you implement them !). Check the "Indicative Marking Guide" to see how each feature may impact your grade - some techniques are more difficult to implement than others, so consequently carry more marks.

If you are taking the examination variant of this unit, you will not be assessed on the topics outlined in this workbook. However you may wish to explore them purely for your own interest.  


# 
### Task 2: Texture Mapping
 <a href='02%20Texture%20Mapping/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

During the rasterisation workbooks, we encountered difficulties when attempting to texture map 3D objects using 2D drawing functions. The problem was that, due to the fact that our texture drawing functions did not take into account z depth of model artefacts, it was not possible to incorporate the effects of perspective in our renders. It is _possible_ to "correct" the perspective of rasterised textures (by using quite a complex formula). However, rendering perspective correct textures using a ray tracer is MUCH easier (once you understand Gouraud or Phong shading, it's actually almost trivial !)

The basic principle is to use the `u`, `v` and `w` proportional distances derived from calculating the closest intersection in order to calculate the position of the required pixel from the texture map. Just as it is possible to calculate a weighted average pixel colour (as shown in the animation above), it is also possible to determine the location of the "donor" pixel from the texture map - by simply calculating a weighted average of vertex texture points.

As one student who previously took this unit once observed: "Barycentric coordinates are great - once you have implemented them, you get perspective corrected texture mapping for free !"  


![](02%20Texture%20Mapping/images/mapping.jpg)

# 
### Task 3: Soft Shadow
 <a href='03%20Soft%20Shadow/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='03%20Soft%20Shadow/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='03%20Soft%20Shadow/audio/segment-2.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='03%20Soft%20Shadow/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

Up until this point, we have taken a simplistic approach to shadows. By only considering single point light sources we can only produce naive and simplistic "hard" shadows (as illustrated in the left hand image below). If we want to implement more realistic "soft" shadows (as illustrated in the right hand image below) we are going to need a more sophisticated approach. View the animation linked to above for a deeper understanding of what we are trying to achieve.

The simplest (but by no means most efficient) way to implement soft shadows is to use multi-point light sources. View the slides and audio narration linked to above for a high-level explanation of how to implement this approach. If you decide to implement soft shadows using this technique, be prepared for a significant increase in rendering time !  


![](03%20Soft%20Shadow/images/soft-shadow.jpg)

# 
### Task 4: Mirrored Surfaces
 <a href='04%20Mirrored%20Surfaces/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='04%20Mirrored%20Surfaces/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='04%20Mirrored%20Surfaces/audio/segment-2.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='04%20Mirrored%20Surfaces/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

Rather than rendering triangles as just opaque, matt surfaces, it would be nice if we could simulate more interesting and complex materials. The slides, audio narration and animation above provide details of how to render triangles as reflective surfaces. We start out by performing a standard "closest intersection" calculation. However, rather than shading the image plane pixel the colour of the intersected triangle, we "bounce" the ray off the surface - using the formula shown in the above slides. The colour of the triangle that is _subsequently_ hit is the one that is then used to shade the pixel. In this way, we can achieve the rendering of mirror-like reflective surfaces.  


![](04%20Mirrored%20Surfaces/images/reflective.jpg)

# 
### Task 5: Environment Maps
 <a href='05%20Environment%20Maps/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='05%20Environment%20Maps/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a>

The problem with rendering mirrored surfaces is that they reflect _everything_ that surrounds them ! The large number of resultant calculations can make rendering complex scenes particularly costly. We can however use flat images in the form of "Environment Maps" to capture a view of the surrounding area from the perspective of a 3D object. Due to the fact that these maps are flat images (rather than 3D models in their own right) this can make the computation of reflections much more efficient. The slides and audio narration above provide a high-level introduction of this approach to rendering.
  


![](05%20Environment%20Maps/images/env-map.jpg)

# 
### Task 6: Metallic Surfaces
 <a href='06%20Metallic%20Surfaces/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

Metallic surfaces behave in a very similar way to mirrors (in fact early mirrors were often made from highly polished sheets of metal). We can however adjust a number of parameters in order to achieve the appearance of different types of metal in our renders. Firstly, we can "tint" a ray of light when it bounces off a surface, depending on the colour of the metal we want to render (for example gold, brass, copper etc).

Metal surfaces are typically reflective, but are often not perfectly smooth (their surfaces contain microscopic imperfections). In order to represent this in your render, you will need to apply minor deviations (often called "perturbations") to the direction of the reflection vectors. 

You will need to investigate an appropriate mechanism to apply these perturbations - implementing this features is tricker than you might think ! If done incorrectly, surfaces often appear "speckled" rather than the intended matt/frosted texture. You may even end up with a surface that appears to "swirl" as the perturbations alter between frames.   


![](06%20Metallic%20Surfaces/images/metals.jpg)

# 
### Task 7: Refractive Materials
 <a href='07%20Refractive%20Materials/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='07%20Refractive%20Materials/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='07%20Refractive%20Materials/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a> <a href='07%20Refractive%20Materials/animation/segment-2.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

Rather than rendering only opaque surfaces it would be nice if we could simulate transparent materials. The slides, audio narration and animations above provide high-level details of how to render triangles as refractive surfaces. This technique can be used in order to represent materials such as glass or water.

Correctly implementing transparent materials is a **difficult** task. This is because we must not only deal with _refraction_ of light, but ALSO the _reflection_ of rays. When the angle of incidence is very shallow (as shown on the right hand side in the image below) the refracted ray does not escape the block. In this situation, the surface of the block acts as a mirror - resulting in internal reflection of the ray.

When attempting an implementation of refractive materials, you may find <a href="https://ricktu288.github.io/ray-optics/" target="_blank">this online optics tool</a> and 
<a href="resources/reference.jpg" target="_blank">this physical Cornell box reference image</a>
both useful in checking and validating your results.
&nbsp;
&nbsp;  


![](07%20Refractive%20Materials/images/internal-reflection.jpg)

# 
### Task 8: Normal Maps
 <a href='08%20Normal%20Maps/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='08%20Normal%20Maps/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='08%20Normal%20Maps/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

Finding the closest intersection between a ray and a set of triangles is a costly activity. This is especially true for complex models where each ray being cast must be checked against a potentially very large number of model triangles. There would be a considerable computational cost saving if we could limit the number of triangles in a model, but still render the surfaces with complex surface geometry. Take a look at the slides, narration and animation linked to above to find out how we can "fake" complex geometry (using "light & shadows"), whilst at the same time keeping the number of model triangles to a minimum.  


![](08%20Normal%20Maps/images/normal-map.jpg)

# 
### Task 9: Photon Maps
 <a href='09%20Photon%20Maps/slides/segment-1.pdf' target='_blank'> ![](../../resources/icons/slides.png) </a> <a href='09%20Photon%20Maps/audio/segment-1.mp4' target='_blank'> ![](../../resources/icons/audio.png) </a> <a href='09%20Photon%20Maps/animation/segment-1.mp4' target='_blank'> ![](../../resources/icons/animation.png) </a>

Previously we used ambient lighting as an approximation to global illumination. However, this is a very big approximation that prevents us from rendering certain types of specialist lighting effects. _Photon maps_ offer a much more sophisticated approach that allow us to precompute patterns of light dispersal in order to (relatively) efficiently render more realistic indirect illumination. Take a look at the slides, audio narration and animation linked to above to gain an insight into this technique.  


![](09%20Photon%20Maps/images/photon-map.jpg)

# 
### End of workbook
