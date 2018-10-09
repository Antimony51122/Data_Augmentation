<h1 align="center">
	3D Construction Algorithms Approaches
</h1>

## Firework Particles Simulations

After the confirmation of applying OpenGL for the next stage of data augmentation works, in order to reinforce my
C++ skill which I have just scratched the surface in Robotics module and accomplish a higher programming standard, on the other hand, prepare for learning the OpenGL package, I have followed another online tutorial of manipulating linear algebra functions to create a particle fire simulation program in C++. The brought me deeper interpretation in 3D linear space and the actualisation via low-level SDL programming.

The GIFs looking like randomly generated firework are actually following rigorous and precise functions that based on 3D linear transformations and time. 

- Definition of boundary conditions let enables the particles to bound back when colliding with the frame wall of the window. 
- The colour is a 4-dimension vector consists of RGB and opacity
- The smoothing effect is created by applying instant gaussian blur of 3 x 3 kernel on particles.

<table>
  	<tr>
	    	<th>Beginning, particles gradually grow</th>
	    	<th>Big Contrast Tone Change</th>
	    	<th>Ending, particles gradually fade out</th>
  	</tr>
  	<tr>
	    	<td><img src="fire_particles/beginning_402px.gif"></td>
	    	<td><img src="fire_particles/tone_change_402px.gif"></td>
	    	<td><img src="fire_particles/ending_402px.gif"></td>
  	</tr>
</table>

The completed project can be found with this link: [https://github.com/Antimony51122/CPP/tree/master/Udemy_Fundamental_Tutorials/chap08_particle_fire_simulation_project](https://github.com/Antimony51122/CPP/tree/master/Udemy_Fundamental_Tutorials/chap08_particle_fire_simulation_project)

</br>

## OpenGL Approaches

> IMPORTANT: once again, I am not permitted to show the data set I have augmented for the real training set due to sensitivity reasons. However, I show materials of various texture and surface appearances which can show similar effects. 

### Low lighting intensity: mimicking the appearance in dark night, 

For the first scene, I have placed various models regarding different texture and surface appearances to mimic a range of scenarios:

<table>
  	<tr>
	    	<th>Camera Movement Forwards and Backwards</th>
	    	<th>Camera Movement left and right</th>
  	</tr>
  	<tr>
	    	<td><img src="gl_scenes/forwards_backwards.gif"></td>
	    	<td><img src="gl_scenes/left_right.gif"></td>
  	</tr>
</table>

- Ceramic Rabbit: a clean pure white surface of that responds to relatively sharper specular lighting. shiny spot towards the camera corresponding to the shooting angle of the specular light
- Wood Crate & Barrel: a mixture relatively more dirty turbid colour with a texture that specular lighting contributes to a much lower extent. The diffuse reflection take control of the appearance here.
- Rusty Metal Robot: metal has a very sharp specular lighting, however the rust makes the surface appearance turbid. Thus the comparing with the pure reflection for the ceramic one, the reflection in this case is a mixture of the light colour and the colour of the surface texture.

**Firstly Let's have a clear look at the models under a directional lighting (sunlight for instance)**

<table>
  	<tr>
    	<th>Directional Shadow</th>
  	</tr>
  	<tr>
    	<td><img src="gl_scenes/dirLight.png"></td>
  	</tr>
</table>

**Appearance under dark night**

<table>
  	<tr>
    	<th>Ceramic Rabbbit</th>
    	<th>Wood Crate</th>
    	<th>Rusty Metal Robot</th>
  	</tr>
  	<tr>
    	<td><img src="gl_scenes/rabbit_specular.gif"></td>
    	<td><img src="gl_scenes/crate_specular.gif"></td>
    	<td><img src="gl_scenes/robot_specular.gif"></td>
  	</tr>
</table>

For the environment build up, I setup a relatively darker scene to imitate the late night traffic scene:

- low lighting leads to low diffuse light that comes from scatter and bounce in many directions reaching spots that aren't in its direct vicinity. 
- a torch light source regarding phong lighting, mimicking the flashlight of surveillance cameras when trying to capture key features from the objective. The angle of the torch light has been set as slightly looking down to imitate the looking down shooting angle of the camera.

<table>
  	<tr>
    	<th>Flashing Torchlight focusing on rabbit</th>
    	<th>Flashing Torchlight on the whole scene</th>
  	</tr>
  	<tr>
    	<td><img src="gl_scenes/torch_rabbit.gif"></td>
    	<td><img src="gl_scenes/torch_whole.gif"></td>
  	</tr>
</table>

Investigation of Shadows: 

<table>
  	<tr>
    	<th>Directional Shadow</th>
  	</tr>
  	<tr>
    	<td><img src="gl_scenes/helicopter_shadow.gif"></td>
  	</tr>
</table>

### Moving Point Light Source (Coloured)

<table>
  	<tr>
    	<th>Moving Point Lighting</th>
    	<th>Yellow Reflection of Point Lighting on the Rabbit</th>
  	</tr>
  	<tr>
    	<td><img src="gl_scenes/moving_point_lighting.gif"></td>
    	<td><img src="gl_scenes/rabbit_yellow_reflection.gif"></td>
  	</tr>
</table>

> As perceived from the first GIF, the reflection of specular light is a property of the material on the model object, the if we specify the colour (yellow, in this case), we can see yellow reflections mixing with the object's original colour being shown.

> From the second GIF, we can perceive yellow reflections orienting towards different angle due to the shoot in angle of the light from the point light. 

</br>

# 2D to 3D Reconstruction

Firstly, let's look at how lighting might affect the facial contour perception: the recognition of facial features is based on the contours, however, contour isn't an abstract thing, instead, it can be defined as the contrast between brightness and colour features that we perceive to build the "edges" on faces. Therefore, as shown in below GIFs changing the light direction might vastly change the contour features hence the facial looking.

<table>
  	<tr>
	    	<th>whirling light source</th>
	    	<th>whirling light source with tone change</th>
	    	<th>Swing light source</th>
  	</tr>
  	<tr>
	    	<td><img src="2D_to_3D/lady_green.gif"></td>
	    	<td><img src="2D_to_3D/lady_purple.gif"></td>
	    	<td width=27%><img src="2D_to_3D/man_swing_brown.gif" height=165px></td>
  	</tr>
</table>







Unlike license plates, or even car body, human face is of a much more complicated shape that cannot be reconstructed with simple geometrical shapes. However, by utilising deep learning, a program that reconstruct human faces from 2D images to form 3D models that based on mapping human face to a series of simplified common facial models by anchor point recognition algorithms has been developed.

> check the project github: https://github.com/AaronJackson/vrn

<table>
  	<tr>
    	<td><img src="2D_to_3D/obama.gif"></td>
    	<td><img src="2D_to_3D/curie.gif"></td>
  	</tr>
  	<tr>
    	<th>Obama</th>
    	<th>Curie</th>
  	</tr>
</table>



<h1 align="center">
	<img src="yaw_pitch_roll_humanFace.png" alt="Awesome">
</h1>

## Rapid Prototyping using Ae on Depth Image

From the Robotics Module, we have scratched the surface of depth image using KINECT depth camera. The outcome is mapping of numbers from 0 to 1 indicating the how far a certain pixel is from the camera. Being inspired by the knowledge about how to demonstrate depth information, I decided to prototype through Ae utilising the depth perception technology. 

> Instead of making up some features when reconstructing 3D facial models, the prototype doesn't create any new information. It only extracts existing information and create the 3D effect by emphasising on some features while suppressing the others. 

<table>
  	<tr>
    	<th>Exaggerated Prototype before Moderation</th>
    	<th>Mapping Facial Features onto Depth Map</th>
  	</tr>
  	<tr>
    	<td><img src="Ae_proto/lincoln_AeUI.gif"></td>
    	<td><img src="Ae_proto/lincoln_depth.gif"></td>
  	</tr>
</table>

<table>
  	<tr>
    	<th>Final Output</th>
  	</tr>
  	<tr>
    	<td><img src="Ae_proto/lincoln_main.gif"></td>
  	</tr>
</table>