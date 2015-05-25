---
layout: post
category : Mobiles 
tagline: "Designing for touch"
tags : [development]
---
{% include JB/setup %} 
I made an Android egg timer app that shows how to make custom touch controlled views.

You basically turn the dial by touch and then it gradually counts down.

### Touch input sequence
* First you need to get the right touch inputs from inside the graphic.
* Then you create a vector from coordinates based on a center in the middle of the circle graphic.
* Use trigonometry and then pythagoras to calculate the degrees that the clock should rotate based on the movement vector.

![TeggTimer Demo Screenshot]({{ site.url }}/assets/TeggTimerDemo.png)

### Trig code:

{% highlight java %}

    //Distance to center from last touch using trig
    double lastSide = getLastTouchX()/Math.cos(Math.atan2(getLastTouchY(), getLastTouchX()));
    double oppoSide = dx/Math.cos(Math.atan2(dy, dx));
    double firstSide = latestX/Math.cos(Math.atan2(latestY, latestX));

    // Do the pythagoras for the triangles sides
    double a = lastSide;
    double b = firstSide;
    double c = oppoSide;
    double degrees = Math.toDegrees(Math.acos((Math.pow(a,2)+Math.pow(b, 2)-Math.pow(c, 2))/(2*a*b)));
{% endhighlight %}

### Code
You can find the rest of the code [here](https://github.com/chrilyng/tegg-timer-android)
