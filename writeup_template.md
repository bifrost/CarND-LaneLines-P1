# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[segmented]: ./test_images_output/segmented_solidWhiteCurve.jpg

[lane]: ./test_images_output/lane_solidWhiteCurve.jpg

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. 

1. Resize image to (960, 540), this step is needed to handle the challenge movie and make sure that region of interest fits the images.

2. Convert to grayscale image, so we can do edge detection.

3. Blur the gray scale image to remove noise. A gaussian noise kernel of size 7 has been chosen.

4. Detect edges by using canny edge detection, with parameters `low_threshold = 50` and `high_threshold = 150`.

5. Apply region of interest, shaped as a trapets.

6. Find lanes by using a hough transformation, with parameters `rho = 1`, `theta = np.pi/180`, `threshold = 30`, `min_line_length = 5` and `max_line_gap = 5`.

7. Extrapolate lane lines.
⋅⋅*Each line segments is splitted into two points, start point and end point.
⋅⋅*The points are then divided into left lane points and right lane points, by using a cut `width/2`.
⋅⋅*A first order polyfit is then used to extrapolate left- and right lane line. The polyfit finds the `height` as a function of the `width`.
⋅⋅*As a result an annotated image with the lane lines is returned. 


I did not modify the draw_lines() function to drawing the extrapolate lane lines. The extrapolate lane lines image is returned in step 7. and superimposed with the resized image in the process_image() function.


I have included two images that shows the result of the pipeline. The first image is superimposed from the original images and the line segments from the hough transformation. The region of interest is shown as blue lines. 

![alt text][segmented]

The second image shows the founded lanes on top of the original image, for more images look in the notebook [P1](./P1.ipynb).

![alt text][lane]


### 2. Identify potential shortcomings with your current pipeline


The current pipeline has lot of shortcomings.

1. The extrapolated lane lines, should be defined as a function of the `height` and not the `width` to make it more robust for vertical lines. The current approach works fine with straight perspective lines, but fails when they bends and get vertical.

2. The right and left line points should be divided by an adaptive algorithm that takes into account the lane center is moving. The current simple approach works only for (almost) unbended lane lines.

3. A first order fit of lines is a bad approximation for real world lines that bends, therefore a second order should be considered. The current approach works fine for (almost) unbended lane lines.

4. Different light condition might give problems in the case of very light images without high contrasts. It might cause that the lane lines is not segmented and therefore not detected.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

1. Use second order fit to the lane lines as a function of the `height`.

2. Use an adaptive algorithm to divide left and right points for the second order fit of the lane lines. It can be done by process points decreasing by `height`. Then keep track of latest left and right lane point to estimate the center.

