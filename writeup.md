# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps: 

1. First, I converted the images to grayscale.
 
2. Apply canny edges algorithm to identify the edges in the road.

3. Crop the region of interest, this region is given by the vertices: ```[(0,img.shape[0]),(450, 290), (490, 290), (img.shape[1],img.shape[0])]```

4. Make use of Hough transform to obtain the lines geometry within the region of interest, I modified the helper function ```hough_lines```, to return both line coordinates and the image.

5. Apply what I called lines subdivision, which is basically classify the resulting lines obtained from the Hough transform, into left lines and right lines. This is done by checking the projected distance to two reference lines:
```    
la, lb = Vector2D(0, img.shape[0]), Vector2D(450, 290)
ra, rb = Vector2D(img.shape[1], img.shape[0]), Vector2D(490, 290)
```

6. The next step is to extrapolate the left lines and right lines to draw the lanes on both sides. I make use of the function ```np.polyfit```, which basically take a collection of points and find the line that best fit the data. Since points are already classified as left or right, we will obtain the desired lanes.

### 2. Identify potential shortcomings with your current pipeline


Below some of the potential shortcomings of this approach:

1. Curvature in the lanes will cause the algorithm to fail, since will not be possible to fit a line on top of the lane.

2. The Hough transform and the canny edges detection do not adapt to the environment, a better approach will need to consider whether is daytime, night, raining, snowing, etc. and adjust the lane detection parameter accordingly.

3. I assuming that both lanes are always visible, but that's not always the case in real life, the algorithm does not account for those situations.

### 3. Suggest possible improvements to your pipeline

Possibles improvement would be:

1. Fit a curve instead of a line for drawing the lanes.

2. Consider different colors channels (RGB, HSV, etc), the current algorithm does not take into account any color feature of the road, which will help the algorithm to better classify the lanes.

3. Adjust pipeline parameters according to the environment conditions.

4. Keep the results of previous detections so we can smooth the lane detection movement, and avoid sudden moves of the lines.