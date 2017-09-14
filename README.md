# Real Time Circle Detection and Tracking for Android OS
This repository focuses on real time circle detection with OpenCV for Android OS. It works very successful even if under adverse lighting conditions.

<p align="center">
  <img src="https://user-images.githubusercontent.com/22610163/30448081-7009626a-9996-11e7-8f01-2684755aa5e2.gif">
</p>

---
**What does this program do?**
1. Gets the camera frames and turn each frame to gray.
2. Applies the Hough Circle Transform to the grayed image.
3. Turn each frame to RGB and display the detected circle in a window.
---

## Theory
[Hough Circle Transform](http://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/hough_circle/hough_circle.html) was used for circle detection in this project. It works in a roughly analogous way to the [Hough Line Transform](http://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/hough_lines/hough_lines.html).

**The flow diagram of this application;**

<p align="center">
  <img src="https://user-images.githubusercontent.com/22610163/29189064-7a16aa8e-7e15-11e7-9fca-3e796c298b07.png">
</p>

*OnCameraFrame* method which is located at [MainActivity.java](https://raw.githubusercontent.com/ahmetozlu/real_time_circle_detection_android/master/CircleDetection/CircleDetection/src/main/java/src/main/MainActivity.java);
    
    public Mat onCameraFrame(CameraBridgeViewBase.CvCameraViewFrame inputFrame) {
            Mat input = inputFrame.gray();
            Mat circles = new Mat();
            Imgproc.blur(input, input, new Size(7, 7), new Point(2, 2));
            Imgproc.HoughCircles(input, circles, Imgproc.CV_HOUGH_GRADIENT, 2, 100, 100, 90, 0, 1000);

            Log.i(TAG, String.valueOf("size: " + circles.cols()) + ", " + String.valueOf(circles.rows()));

            if (circles.cols() > 0) {
                for (int x=0; x < Math.min(circles.cols(), 5); x++ ) {
                    double circleVec[] = circles.get(0, x);

                    if (circleVec == null) {
                        break;
                    }

                    Point center = new Point((int) circleVec[0], (int) circleVec[1]);
                    int radius = (int) circleVec[2];

                    Imgproc.circle(input, center, 3, new Scalar(255, 255, 255), 5);
                    Imgproc.circle(input, center, radius, new Scalar(255, 255, 255), 2);
                }
            }

            circles.release();
            input.release();
            return inputFrame.rgba();
    }

## Project Demo
- The demo video of this project is available on YouTube: https://www.youtube.com/watch?v=eWxtt1411Xs
- The screenshots;

![38](https://user-images.githubusercontent.com/22610163/29189984-c845b03a-7e18-11e7-9f96-4a1564747684.jpg) ![13](https://user-images.githubusercontent.com/22610163/29190037-f6bb5ac8-7e18-11e7-829b-97ef38a1ff04.jpg) ![54](https://user-images.githubusercontent.com/22610163/29190038-f6c4e048-7e18-11e7-941b-1cc99fb6c0a2.jpg)

## Installation
Dependencies:
- Android Studio
- OpenCV ([version 3.1.0](https://sourceforge.net/projects/opencvlibrary/files/opencv-android/3.1.0/OpenCV-3.1.0-android-sdk.zip/download))

**How to build and run this source?**

  1. Clone project by using Android Studio
  2. Select and build module *CircleDetection*

## Citation
If you use this code for your publications, please cite it as:

    @ONLINE{vdtc,
        author = "Ahmet Özlü",
        title  = "Real Time Circle Detection and Tracking for Android OS",
        year   = "2017",
        url    = "https://github.com/ahmetozlu/real_time_circle_detection_android"
    }

## Author
Ahmet Özlü

## License
This system is available under the MIT license. See the LICENSE file for more info.
