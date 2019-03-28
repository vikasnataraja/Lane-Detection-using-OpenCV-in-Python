# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Identify spaces for improvement and make important observations

## **Reflection**

### **1. Overview of the pipeline and draw_lines() function**

The software pipeline I designed consists of the following steps:
* Read the image input and convert it to grayscale. This is done by invoking the `grayscale(image)` function. This function basically uses OpenCV to convert the RGB colorspaced image to grayscale and returns the grayscale image.
* Next, define the kernel size for the filter and apply Gaussian smoothing filter over the image.
* This is follwed by the implementation of the Canny Edge Detection algorithm. The parameters, namely the low_threshold, high_threshold are defined. The Canny algorithm takes the grayscale image and returns an image with identified edges. The threshold values can be changed to get better edges.
* Now, define the region of interest. Here, it is the lane. A 4-sided polygon is defined to fit the lane space. It should be noted that while the vertices for the polygon can be given in any order, the y-coordinate is defined top to bottom, meaning the origin of the image is at the top-left corner instead of the conventional bottom-left corner.
* The next step is to define the parameters for Hough transform. These parameters include rho, theta, threshold, minimum line length, maximum line gap.
* Run the Hough transform by calling the `hough_lines()` function. This function performs the Hough transform and also draws the lines on the lane boundaries or lines using the `draw_lines()` function inside the `hough_lines()` function.
    * The `draw_lines()` function has several parts to it as it performs multiple jobs.
    * Because there are only line segments on the image now, we need to identify what line segment corresponds to what side of the lane. This is done by calculating slope of the line segments and therefore separating them (slope < 0 is left lane because origin of image is at top-left corner. But this confusion is avoided by using absolute value).
    * Next, calculate the average values of the slopes and intercepts using the straight line formula y = mx + c, where m is the slope, c is the intercept.
    * Find the initial and final points of the line segments. This would be the x and y coordinates of the start and finish of the lane segments.
    * Join the identified points and the lane markings can be completed by applying weights to change color and thickness.
* Display the final image after applying `addWeighted()` function.
    

![Original image]['C:/Users/Vikas HN/Desktop/whiteCarLaneSwitch.jpg']
![Image with lanes identified] ['C:/Users/Vikas HN/Desktop/whiteCarLaneSwitch_output.jpg']


### **2. Potential shortcomings**

While there are some good takeaways from this project, ther are alos some shortcomings:

* First, the program and the algorithm are designed in such a way that they only work if the position of the camera isn't changed. That is a big assumption made. The region of interest depends on this assumption that the camera is fixed. If the camera moves a little, the polygon for the region of interest would not fit the lane anymore.
* Second, we make another assumption in that lanes are of the same width and size in general. The region of interest and the polygon are designed in such a way to fit one size of lanes with very little room for error. Therefore, if the lane width changes, then the algorithm would fail to identify the lanes.
* Third, the algorithm works only on straight roads. If the road becomes even a little curved, which happens towards the end of solidYellowLeft video, the algorithm fails to adjust to the curved roads.
* Fourth, the algorithm works only when there are lane markings on a road. In rural areas, where there are often no lane markings or unclear lane markings, the algorithm would struggle or just fail.


### **3. Possible improvements**

Here are some improvements that can be made to my pipeline:

* Firstly, make the polygon and the region of interest dynamic. More specifically, the polygon should keep changing with the lane width and the road ahead as they are not always constant. Making a dynamically changing polygon would make the algorithm work on different sized roads.
* Secondly, change the `draw_lines()` function to draw curved lines instead of straight lines. This can be achieved by substituting the y = mx + c formula with a partial ellipse, hyperbola, parabola or a partial circle. This would enable the algorithm to work on curved lanes, which would be hugely important.
* Thirdly, introduce a width paramter for the lane. This way, if there is an obstacle on some part of the lane, the algorithm still knows to identify the lanes using the width difference.
* Lastly, use one uniform library to read and write images. This is because the matplotlib library works on BGR colorspace and the OpenCV library works with RGB colorspace. Using one of those to perform read and write operations on images would prevent any confusion
