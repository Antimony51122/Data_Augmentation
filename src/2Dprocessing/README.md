<h1 align="center">
	2D Image Processing Algorithms
</h1>

> IMPORTANT: this session shows a glimpse at the beginning, of a small part of real training set we have produced. No source code permitted to be provided. For the rest sessions, works using the same technique processing on other images from other resources other than the original data set will be provided instead.

<table>
  	<tr>
    	<th>Original License Plate</th>
  	</tr>
  	<tr>
		<td><img src="license_plate_total/original.jpg" width=265px></td>
  	</tr>
</table>

### Motion Blur

<table>
  	<tr>
    	<th>Motion Blur </br> size 9</th>
    	<th>Motion Blur </br> size 11</th>
    	<th>Motion Blur </br> extreme</th>
  	</tr>
  	<tr>
		<td><img src="license_plate_total/motion_blur_9.jpg" width=265px height=78px></td>
		<td><img src="license_plate_total/motion_blur_11.jpg" width=265px height=78px></td>
		<td>
			<img src="license_plate_total/motion_blur_extreme.jpg" width=265px height=78px>
		</td>
  	</tr>
</table>

### Reshaping

<table>
  	<tr>
    	<th>Affine Transformation </br> Null pixels filled with light gray</th>
    	<th>Affine Transformation </br> Null pixels filled with dark gray</th>
    	<th>Affine Transformation </br> Null pixels filled with black</th>
  	</tr>
  	<tr>
		<td>
			<img src="license_plate_total/affine_transformation1.jpg" width=265px height=78px>
		</td>
		<td>
			<img src="license_plate_total/affine_transformation2.jpg" width=265px height=78px>
		</td>
		<td>
			<img src="license_plate_total/affine_transformation3.jpg" width=265px height=78px>
		</td>
  	</tr>
</table>

### Pixelation 

<table>
  	<tr>
    	<th>Pixelation </br> reduced size 4 and restore</th>
    	<th>Pixelation </br> extreme</th>
  	</tr>
  	<tr>
		<td><img src="license_plate_total/pixelation.jpg" width=265px height=78px></td>
		<td><img src="license_plate_total/pixelation_extreme.jpg" width=265px height=78px></td>
  	</tr>
</table>

### Histogram Equalisation

<table>
  	<tr>
    	<th>Histogram</th>
  	</tr>
  	<tr>
		<td><img src="license_plate_total/histogram_equalisation.jpg" width=265px></td>
  	</tr>
</table>

### Brightness Variation

<table>
  	<tr>
    	<th>Brightness Variation </br> increase (daylight)</th>
    	<th>Brightness Variation </br> decrease (afternoon & night)</th>
    	<th>Brightness Variation </br> extreme (very dark night) </br> <b>loss of key features</b></th>
  	</tr>
  	<tr>
		<td>
			<img src="license_plate_total/brightness_increase.jpg" width=265px height=78px>
		</td>
		<td>
			<img src="license_plate_total/brightness_decrease.jpg" width=265px height=78px>
		</td>
		<td>
			<img src="license_plate_total/brightness_extreme.jpg" width=265px height=78px>
		</td>
  	</tr>
</table>

### Combinations

<table>
  	<tr>
    	<th>Gentle Gaussian Blur</th>
    	<th>Main Blur (combination of blurs)</th>
    	<th>Unachievable Blur </br> (need to be manually deducted </br> from the training set)</th>
  	</tr>
  	<tr>
		<td><img src="license_plate_total/gaussian_blur.jpg" width=265px height=78px></td>
		<td><img src="license_plate_total/main_blur.jpg" width=265px height=78px></td>
		<td><img src="license_plate_total/unachievable_blur.jpg" width=265px height=78px></td>
  	</tr>
</table>

</br>

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
	<img src="motion_blur/motionBlur_kernels.jpg" width=50% />
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
    	<td><img src="motion_blur/Hillary/Hillary.jpg"></td>
    	<td><img src="motion_blur/Hillary/Hillary_liangAlgorithm_horizontal_6.jpg"></td>
		<td><img src="motion_blur/Hillary/Hillary_motionBlur_5_118.jpg"></td>
		<td><img src="motion_blur/Hillary/Hillary_motionBlur_7_93.jpg"></td>
		<td><img src="motion_blur/Hillary/Hillary_motionBlur_11_4.jpg"></td>
  	</tr>
</table>

### Compensation:

#### Prototype: Sharpening with Emboss

**the original image**

<table>
	<tr>
		<th><img src="motion_blur/0_original.jpg"></th>
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
    	<td><img src="motion_blur/0_original.png"></td>
    	<td><img src="motion_blur/1_smart_sharpening.png"></td>
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
    	<td><img src="motion_blur/1_smart_sharpening.png"></td>
    	<td><img src="motion_blur/2_emboss_normal.png"></td>
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
    	<td><img src="motion_blur/2_emboss_normal.png"></td>
    	<td><img src="motion_blur/3_emboss_overlay.png"></td>
  	</tr>
</table>

</br>

<!-------------------- Motion Blur Session Finished -------------------->

> check more sharpening algorithms approaches in the [More Sharpening File](./more_shrapening.md)

</br>

## Pixelation in Photoshop

### Compensation: de-pixelation

> Bicubic Interpolation - Computerphile: <https://www.youtube.com/watch?v=poY_nGzEEWM>

1. --> Image Size (Alt + Ctrl + I) --> Resolution: change the resolution to 300 pixels per inch or higher if you want, then make sure **Bicubic Automatic** option is selected
2. --> Filter --> Noise --> Reduce Noise --> play around with the settings:
    
	- Strength: 10
    - Preserve Details: 0
    - Reduce Colour Noise: 86%
    - Sharpen Details: 52%
    - tick Remove **JPEG Artifact**

<table>
  	<tr>
    	<th>Image with very Low Resolution</th>
    	<th>Image after De-pixelation</th>
  	</tr>
  	<tr>
    	<td><img src="de-pixelate/Trump_resLowLow.png" width=500px></td>
    	<td><img src="de-pixelate/Trump_resHigh_sharpen.png" width=500px></td>
  	</tr>
</table>

<br/>

<!-------------------- Pixelation Session Finished -------------------->

## Affine Transformation

#### 1. Lens Correction

1. rename and convert to smart object 
2. check:
    - Geometric Distortion
    - Chromatic Aberration
    - Vignette
        > Vignette is darkening around the corners of the image essentially caused by lens element
        > to get rid of it, that's brighten up the colour



#### 2. Distortion, Aberration and Vignette
    
1. choose `Custom` setting tag of `Lens Correction`
2. go to upper-left corner of the window, start with the `Remove Distortion Tool`:
    - dragging outward: add barrel distortion
    - dragging inward: add pincushioning distortion
3. play around with the settings in `Chromatic Aberration` and `Vignette`

#### 3. Adjusting Angle and Perspective  

1. navigate to the `Transform` tag, play around with the `Angle`
2. choose `Move Grid Tool`, change in the bottom, the grid `Color` to make the grid contrastively visible and `Size` to make it align better with the image features.  
3. go back to the `Transform` tag, play around with the `Horizontal Perspective` and `Vertical Perspective`.
    
#### 4.1 Using the Perspective Warp Command

1. set 4 `Guidelines`
2. go to `Edit`, navigate to `Perspective Warp`
    
	- establish a base setting for our boundaries that the vertices should align to the four points you want to use as reference points
	- click `Enter` entering the edit mode in step 2 to drag and adjust the image

<table>
  	<tr>
    	<th>Original Image</th>
    	<th>Image after Perspective Wrap</th>
  	</tr>
  	<tr>
    	<td><img src="lens_correction/0_original.png" width=500px></td>
    	<td><img src="lens_correction/1_perspective_wrap.png" width=500px></td>
  	</tr>
</table>
       
#### 4.2 Fine-Tuning the Perspective Adjustment

1. gradually improve the image by playing around the 4 vertices
2. after the four vertices of the perspective warping box aligned to the 4 boundary vertices, check if there is still geometric distortions by checking whether there are slightly curved lines. Go back to step 1 and adjust the `Geometric Distortion` value for further compensation.
    
> Beware that the last step has no preview effect influenced by the later action of perspective warp

<table>
  	<tr>
    	<th>Image after Perspective Wrap</th>
    	<th>Image after Lens Correction (solve fish-eye)</th>
  	</tr>
  	<tr>
    	<td><img src="lens_correction/1_perspective_wrap.png" width=500px></td>
    	<td><img src="lens_correction/2_lens_correction.png" width=500px></td>
  	</tr>
</table>

#### 5. Evening out Colour and Lighting

> This section has nothing to do with the `Lens Correction` filter but everything to do with quality image editing.

1. make sure the image layer has been selected and go to the bottom of the image click the first icon and select `Color Overlay`. 
2. darken the image by navigate to `Layer`, choose `Brightness/Contrast...` 
    - select the blend mode to `Multiply`
    - set the `Opacity` to `50%`
3. choose the layer mask, choose `Brush` and draw on the image's dark spots and lighten them.

<table>
  	<tr>
    	<th>Image after Lens Correction</th>
    	<th>Image after Lens Colour/ Brightness/ Contrast Tuning</th>
  	</tr>
  	<tr>
    	<td><img src="lens_correction/2_lens_correction.png" width=500px></td>
    	<td><img src="lens_correction/3_effects.png" width=500px></td>
  	</tr>
</table>

## Facial Landmark Manipulations

<img src="face_swap/algorithm1.png" width=100%>
<img src="face_swap/algorithm2.png" width=100%>

### Facial Landmark Detection



<table>
  	<tr>
    	<th>Facial Landmark Detection on Obama</th>
  	</tr>
  	<tr>
    	<td><img src="face_swap/Obama_with_landmarks.jpg"></td>
  	</tr>
</table>

### Facial Swap according to Landmark Detection

A little fun has been created using the facial landmark detection algorithms. 

<table>
  	<tr>
    	<th>Face Swap on Trump and Hillary (Creepy)</th>
  	</tr>
  	<tr>
    	<td><img src="face_swap/faceswap_trump_hilliary.JPG"></td>
		<!---------- be careful the source of the image has .JPG as format, it might still show in the markdown editor, but the github page is case sensitive and will return 404 on the image ---------->
  	</tr>
</table>

> From this little fun here we can see the drawbacks of facial landmark detection algorithms that it works only fine with front view. As long as there is tilting, the algorithm starts to messing about. Therefore, 2D to 3D approaches will be needed. (shown in latter section)

</br>

