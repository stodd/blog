---
layout: post
title:  "Papervision Beginnings"
date:   2009-06-30 14:12:00
categories: papervision 3d
---
I have been experimenting with papervision lately and it's great fun. I tried a sample project just to get me started thinking about 3D programming and graphics programming.
<object width='250' height='250'>
	<param name='movie' value='images/PV3DTest01.swf'>
	<embed src='/images/PV3DTest01.swf' width='250' height='250'/>
</object>
While it isn't amazing it marks my beginning in this topic. To get the plane to turn I perform a little math for its rotation every frame:
{% highlight actionscript %}
angle %= 360;
               
plane.rotationY = angle;
plane.rotationX = angle;
{% endhighlight %}