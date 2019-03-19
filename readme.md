**Eye blink detection with OpenCV, Python, and dlib**

To build our blink detector, we’ll be computing a metric called the *eye aspect
ratio* (EAR), introduced by Soukupová and Čech in their 2016 paper, [Real-Time
Eye Blink Detection Using Facial
Landmarks](http://vision.fe.uni-lj.si/cvww2016/proceedings/papers/05.pdf).

Unlike traditional image processing methods for computing blinks which typically
involve some combination of:

1.  Eye localization.

2.  Thresholding to find the whites of the eyes.

3.  Determining if the “white” region of the eyes disappears for a period of
    time (indicating a blink).

The eye aspect ratio is instead a *much more elegant solution* that involves
a *very simple calculation* based on the ratio of distances between facial
landmarks of the eyes.

This method for eye blink detection is fast, efficient, and easy to implemen

**In terms of blink detection, we are only interested in two sets of facial
structures — the eyes.**

Each eye is represented by 6 *(x, y)*-coordinates, starting at the left-corner
of the eye (as if you were looking at the person), and then working clockwise
around the remainder of the region:

![](<https://www.pyimagesearch.com/wp-content/uploads/2017/04/blink_detection_6_landmarks.jpg>)

Based on this image, we should take away on key point:

**There is a relation between the width and the height of these coordinates.**

Based on the work by Soukupová and Čech in their 2016 paper, [Real-Time Eye
Blink Detection using Facial
Landmarks](http://vision.fe.uni-lj.si/cvww2016/proceedings/papers/05.pdf), we
can then derive an equation that reflects this relation called the* eye aspect
ratio* (EAR):

![](<https://www.pyimagesearch.com/wp-content/uploads/2017/04/blink_detection_equation.png>)

Where *p1, …, p6* are 2D facial landmark locations.

The numerator of this equation computes the distance between the vertical eye
landmarks while the denominator computes the distance between horizontal eye
landmarks, weighting the denominator appropriately since there is only *one* set
of horizontal points but *two* sets of vertical points.

Why is this equation so interesting?

Well, as we’ll find out, the eye aspect ratio is approximately constant while
the eye is open, but will rapidly fall to zero when a blink is taking place.

Using this simple equation, we can *avoid image processing techniques* and
simply rely on the *ratio of eye landmark distances* to determine if a person is
blinking.

To make this more clear, consider the following figure from Soukupová and Čech

![](<https://www.pyimagesearch.com/wp-content/uploads/2017/04/blink_detection_plot.jpg>

On the *top-left* we have an eye that is fully open — the eye aspect ratio here
would be large(r) and relatively constant over time.

However, once the person blinks (*top-right*) the eye aspect ratio decreases
dramatically, approaching zero.

The *bottom* figure plots a graph of the eye aspect ratio over time for a video
clip. As we can see, the eye aspect ratio is constant, then rapidly drops close
to zero, then increases again, indicating a single blink has taken place.
