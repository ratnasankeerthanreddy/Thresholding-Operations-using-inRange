# Thresholding-Operations-using-inRange
Goal
In this tutorial you will learn how to:

Perform basic thresholding operations using OpenCV cv::inRange function.
Detect an object based on the range of pixel values in the HSV colorspace.
Theory
In the previous tutorial, we learnt how to perform thresholding using cv::threshold function.
In this tutorial, we will learn how to do it using cv::inRange function.
The concept remains the same, but now we add a range of pixel values we need.
HSV colorspace
HSV (hue, saturation, value) colorspace is a model to represent the colorspace similar to the RGB color model. Since the hue channel models the color type, it is very useful in image processing tasks that need to segment objects based on its color. Variation of the saturation goes from unsaturated to represent shades of gray and fully saturated (no white component). Value channel describes the brightness or the intensity of the color. Next image shows the HSV cylinder.

Threshold_inRange_HSV_colorspace.jpg
By SharkDderivative work: SharkD [CC BY-SA 3.0 or GFDL], via Wikimedia Commons
Since colors in the RGB colorspace are coded using the three channels, it is more difficult to segment an object in the image based on its color.

Threshold_inRange_RGB_colorspace.jpg
By SharkD [GFDL or CC BY-SA 4.0], from Wikimedia Commons
Formulas used to convert from one colorspace to another colorspace using cv::cvtColor function are described in Color conversions
Explanation

C++JavaPython
Let's check the general structure of the program:

Capture the video stream from default or supplied capturing device.
cap = cv.VideoCapture(args.camera)
Create a window to display the default frame and the threshold frame.
cv.namedWindow(window_capture_name)
cv.namedWindow(window_detection_name)
Create the trackbars to set the range of HSV values
cv.createTrackbar(low_H_name, window_detection_name , low_H, max_value_H, on_low_H_thresh_trackbar)
cv.createTrackbar(high_H_name, window_detection_name , high_H, max_value_H, on_high_H_thresh_trackbar)
cv.createTrackbar(low_S_name, window_detection_name , low_S, max_value, on_low_S_thresh_trackbar)
cv.createTrackbar(high_S_name, window_detection_name , high_S, max_value, on_high_S_thresh_trackbar)
cv.createTrackbar(low_V_name, window_detection_name , low_V, max_value, on_low_V_thresh_trackbar)
cv.createTrackbar(high_V_name, window_detection_name , high_V, max_value, on_high_V_thresh_trackbar)
Until the user want the program to exit do the following
    ret, frame = cap.read()
    if frame is None:
        break
    frame_HSV = cv.cvtColor(frame, cv.COLOR_BGR2HSV)
    frame_threshold = cv.inRange(frame_HSV, (low_H, low_S, low_V), (high_H, high_S, high_V))
Show the images
    cv.imshow(window_capture_name, frame)
    cv.imshow(window_detection_name, frame_threshold)
For a trackbar which controls the lower range, say for example hue value:
def on_low_H_thresh_trackbar(val):
    global low_H
    global high_H
    low_H = val
    low_H = min(high_H-1, low_H)
    cv.setTrackbarPos(low_H_name, window_detection_name, low_H)
static void on_low_H_thresh_trackbar(int, void *)
{
    low_H = min(high_H-1, low_H);
    setTrackbarPos("Low H", window_detection_name, low_H);
}
For a trackbar which controls the upper range, say for example hue value:
def on_high_H_thresh_trackbar(val):
    global low_H
    global high_H
    high_H = val
    high_H = max(high_H, low_H+1)
    cv.setTrackbarPos(high_H_name, window_detection_name, high_H)
It is necessary to find the maximum and minimum value to avoid discrepancies such as the high value of threshold becoming less than the low value.
Results
After compiling this program, run it. The program will open two windows
As you set the range values from the trackbar, the resulting frame will be visible in the other window.

Threshold_inRange_Tutorial_Result_input.jpeg
Threshold_inRange_Tutorial_Result_output.jpeg



this whole code is taken from opencv offical site 
https://docs.opencv.org/master/da/d97/tutorial_threshold_inRange.html
