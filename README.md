# <center>**Industrial Placement Project Work and Outputs**</center> 

<img src="images/cover_page/video_surveillance_cartoon.jpg" width=100% height=75%>

> **IMPORTANT**: mind that most of work I have done during my placement has been specified containing sensitive contents, therefore, after negotiation with the industry tutor, it has been agreed that, project works cannot be shown directly, but, I am allowed to use the same technique learnt from the placement and produce something else that is not straightly related to the interest of the company with providing very little portion of the source code. For the Data Acquisition session, photos of the scenes shown are taken by my phone and my colleagues only for the purpose of explanation of considerations for video capturing, none of the screenshots from the real surveillance camera are allowed to be provided.

# Data Acquisition

As the idiom says "One can't make bricks without straw". Investigation on image processing algorithms needs image/video data as crude resources. The following session introduces the process we proceeded to capture video sources as dataset for future algorithm tuning.

Obtaining video that captures that fits the needs for the training set is not simply placing a camera alongside the pedestrian and let it record. In order to obtain the video capture that maximises effective data input, trials and adjustments have to be done before figuring out the best shooting angle and height. Besides, even after we have found the appropriate location and wide angle for shooting we acquired for, essential "1 centimetre" micro-adjustments have been proceeded sometimes to find the just adequate position to mitigate an unwanted covering object such as branches of trees or a traffic sign. (Arguing with traffic police is certainly another issue that costs much effort during shootings but this is not within the technical discussions here :) )

<table>
  	<tr>
    	<th>Renke Setting up and Adjusting Surveillance Camera Angle</th>
  	</tr>
	<tr>
		<th><img src="images/videoData_acquisition/renke_adjustCamera.jpg" /></th>
	</tr>
</table>

## Scene 1: Tunnel Exit

### Main Features:

- Front-lighting / Noon
- Round Corner
- Vehicles mainly middle and smaller car, motors
- Mere pedestrians
- Tree branches and barricades presenting

In order to capture as many angles of views as possible, especially the gradual transition from front view to side view, we priorly chose road with round corners due that you cannot place the camera just facing in front of the cars in the middle of the street to 

## Scene 2: Interception of Tunnel Exit with Urban Traffic Arterial

### Main Features:

- Back-lighting / Afternoon
- Large Round Corner with huge turn
- All Vehicles presenting
- Frequent pedestrians
- Tree branches, Traffic signs and barricades presenting but not covering key information

## Scene 3: A bridge in front of the Exit of a Housing Estate

### Main Features:

- Partial Back-lighting / Sunset
- Round Corner with a half complete turn
- Mostly motors and bicycles presenting
- All kinds of pedestrians
- Tree branches presenting but not covering key information

# Rapid Prototypes of 2D Image Processing

Before directly aimlessly going into coding, it was more targeted to start with applying design engineering agile
approaching idea of rapid prototyping. I firstly try to investigate how environmental effects on camera lens can
influence the imaging. Then produce a rapid prototyping with Photoshop filters, After Effect 3Ds or GIMP tools
and try to deeply understand the logic behind parameters in each layer of filter and the influence of order of
layers.

## Motion Blur - Uniform & Linear

### Principle:

Motion blur is the apparent streaking of moving objects in a photograph or a sequence of frames, such as a film or animation. It results when the image being recorded changes during the recording of a single exposure, due to rapid movement or long exposure.

### Generation:

#### Algorithms:

In general there are two parameters which need to be considered when dealing with such a blur. Intuitively, these are the magnitude and the direction of the blur. To replicate motion blur using a blur kernel we start by constructing a matrix of zeroes. We then replace the entries of a specific row with 1's (usually the middle row), to imitate the effect of the blur. Finally we rotate the matrix by a specified angle and normalise it.

<div align=center>
	<img src="images/2Dprocessing/motion_blur/motionBlur_kernels.jpg" width=50% />
</div>

**Randomly generated motion blur orientation and extent**

<table>
  	<tr>
    	<th>Original</th>
    	<th>Horizontal  </br>  6 pixels shifting</th>
		<th>118 degrees </br>  5 pixels shifting</th>
		<th>93 degrees  </br>  7 pixels shifting</th>
		<th>4 degrees   </br> 11 pixels shifting</th>
  	</tr>
  	<tr>
    	<td><img src="images/2Dprocessing/motion_blur/Hillary/Hillary.jpg"></td>
    	<td><img src="images/2Dprocessing/motion_blur/Hillary/Hillary_liangAlgorithm_horizontal_6.jpg"></td>
		<td><img src="images/2Dprocessing/motion_blur/Hillary/Hillary_motionBlur_5_118.jpg"></td>
		<td><img src="images/2Dprocessing/motion_blur/Hillary/Hillary_motionBlur_7_93.jpg"></td>
		<td><img src="images/2Dprocessing/motion_blur/Hillary/Hillary_motionBlur_11_4.jpg"></td>
  	</tr>
</table>

### Compensation:

#### Prototype: Sharpening with Emboss

**the original image**

<table>
	<tr>
		<th><img src="images/2Dprocessing/motion_blur/0_original.jpg"></th>
	</tr>
</table>

##### 1) Smart Sharpening:

No pre-set settings, carry out experiments, due that their angles, pixels are very different to each other

If we crop the image to the place most significantly shows the effect of motion blur (in this case: around eyes)

<table>
  	<tr>
    	<th>Original</th>
    	<th>After Smart Sharpening</th>
  	</tr>
  	<tr>
    	<td><img src="images/2Dprocessing/motion_blur/0_original.png"></td>
    	<td><img src="images/2Dprocessing/motion_blur/1_smart_sharpening.png"></td>
  	</tr>
</table>

##### 2) Further compensating with Emboss

- Emboss with Angle opposite to the angle of removing sharpening, if the same angle as above, will compound the halos
    
	- Height: 1px
	- Amount: 500%
	- Blending Option: Overlay (or try other modes in the overlay category)

<table>
  	<tr>
    	<th>After Smart Sharpening</th>
    	<th>After Emboss</th>
  	</tr>
  	<tr>
    	<td><img src="images/2Dprocessing/motion_blur/1_smart_sharpening.png"></td>
    	<td><img src="images/2Dprocessing/motion_blur/2_emboss_normal.png"></td>
  	</tr>
</table>

Emboss Algorithm: <https://www.packtpub.com/mapt/book/application_development/9781785283932/2/ch02lvl1sec23/embossing>

**Blend Mode: Overlay:**

<table>
  	<tr>
    	<th>Emboss Normal Blend Mode</th>
    	<th>Emboss Overlay Blend Mode</th>
  	</tr>
  	<tr>
    	<td><img src="images/2Dprocessing/motion_blur/2_emboss_normal.png"></td>
    	<td><img src="images/2Dprocessing/motion_blur/3_emboss_overlay.png"></td>
  	</tr>
</table>