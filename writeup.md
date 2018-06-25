# **Finding Lane Lines on the Road** 
The goal of this project is to create a pipeline that finds lane lines on the road.Finding lane lines is critical for autonomous vehicles since it defines a boundary or region within which the vehicle is allowed to be driven.
### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline consists of different stages of image processing.This is achieved by using OpenCV techniques.Images from the test_images folder are used to test the developed pipleline.The different stages of the pipeline is as follows:

1. Grayscale : The image was first converted to Grayscale.This makes it easier to extract the white and yellow lines.The pixel intensity around these lines is brighter than rest of the pixels in the image.When we convert the picture to Grayscale,these bright pixels are white while the rest of the image is black.

2. Gaussian Blur : This is needed to help the next stage of edge detection. By blurring the image,the image noise and image details is reduced.This helps us to retain only the relevant/significant lines in the image.This stage requires kernel size input.The kernel size determines the degree of blurring.We need to find a number which retains the relevant lines but helps discard insignificant ones.

3. Canny Edge Detection : The output image from the Gaussian Blur stage is passed to the Edge Detection stage as input.This processing technique is needed to find the edges of the objects in the image. It uses the change in the strength of the gradient of the adjacent pixels to detect edges.The input required is low and high threshold values.Since we have an 8 bit grayscaled image,the low and high thresholds can have values between 0-255.The recommended ratio is 1:3.This algorithm will detect and keep the strong edge pixels above the high threshold and discard pixels below low threshold.It will also retain pixels that are between low and high threshold if they are connected to pixels with high threshold.The low and high threshold values used are 70 and 210 respectively.

4. Region of Interest : The canny edge detects a lot of edges in the image.We are interested only in certain area of the image(Lane lines area).We define the Region of interest which discards all the edges outside of the polygon.This helps us concentrate only on the edges within the region of interest.

5. Hough Lines Transform : The input to this stage is the edges within the region of interest.This stage will return the endpoints of all the detected line segments.

6. Draw Lines : The endpoints of the detected lines from the hough lines are the input to the draw lines stage. In order to draw the lines,we need to separate the right and left lines.This is done by calculating the slope of each and every line.
If the slope is between positive min_slope and max_slope values its added to the left slope list else if its between negative min_slope and max_slope its added to right slope list.This filtering is used to discard lines having too small or big slope.Filtering out slopes less than 0.5 makes sure we discard line sthat are almost vertical.Filtering values also helps in getting smooth average of slopes.Additional step taken to get smooth detection is to take an average of few slopes.I have calculated average of 15 slopes.

7. Line Extrapolation : Based on the average slope calculated for left and right list and the known 'y' co- ordinate, we calculate the 'x' co-ordinate.Using these x and y co ordinates we draw lane lines.These lones are then merged with original image.The output images are saved to test_images_output.

This pipeline was then tested on the videos in test_videos.The output can be found in test_videos output. It worked fine on both yellow and white line videoes.The pipeline did not work well on the challenge video.Below are the output images and video folder links.

[Test Output Images](./test_images_output)
[Test Output Videos](./test_video_output)



### 2. Identify potential shortcomings with your current pipeline

This current pipeline did not work well for the challenge video.The lane lines are not smooth for patch of cement road/dark raod area.


### 3. Suggest possible improvements to your pipeline

Add HSL filter to the pipeline to make the lane lines smooth for the cement/dark road area.