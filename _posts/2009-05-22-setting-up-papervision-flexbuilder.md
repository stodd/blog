---
layout: post
title:  "Setting Up Papervision3d in FlexBuilder"
date:   2009-05-22 14:14:00
categories: papervision 3d
---
Setting up your FlexBuilder environment for papervision3d isn't too difficult as long as you familiar with SVN and eclipse. First I assume you are using the latest FlexBuilder and have completed the [Flex in a Week tutorial series from Adobe](http://www.adobe.com/devnet/flex/videotraining/).

First, before we begin, a quick overview of what we are going to do:
* Get SVN in Eclipse.
* Checkout Papervision3D from SVN.
* Build Papervision3D.
* Create a new Flex Project and reference Papervision3D.

To begin you need to get SVN installed in Eclipse, if you already have SVN installed you can skip this step. Open up Eclipse/FlexBuilder and click on 'Help' in the top right, click on Software Updates. Once that screen is open click on the 'Available Software' tab and then click on 'Add Site.' Add this url in the dialog that pops up: http://subclipse.tigris.org/update_1.4.x. The SVN plug in site is available in the update sites list now. Check the site and install. After the install process SVN should be available to you. If you need more help installing SVN you can read different instructions [here](http://subclipse.tigris.org/servlets/ProjectProcess?pageID=p4wYuA)

After your up and running with SVN add the following repository to the SVN view: ```http://papervision3d.googlecode.com/svn/trunk```

First expand the repository then as3 folder and right click and checkout the 'trunk' folder. Checkout the project as papervision3d. It will probably take a minute or two to download all the files into the project. Once it completes right click on your new project and go to 'Flex Project Nature' and then select 'Add Flex Library Project Nature.'

Now all you have to do is build the project to get the swc file that you will include in your projects, the flex equivalent to a jar file. Open the build file, build/build.xml, in the project and change the location of the flex3dir property to the location of your flex SDK. My default location was ```C:/Program Files/Adobe/Flex Builder 3 Plug-in/sdks/3.2.0``` but yours might be different. Now you can run the generateSWC ant task and the build script should compile all the code into a swc file in the bin directory.

Now create a new Flex project called whatever you want. With the project selected hit <kbd>alt</kbd> + <kbd>enter</kbd> to bring up the project properties and click on 'Flex Build Path.' Go to the 'Library Path' tab and Add Project. Select your 'papervision3d' project you just checked out and built. Now you should be ready to use papervision3d in your project.

If your still with me after all that we can get your application ready for the many papervision tutorials out there. Open up your MXML file and add a creationComplete event that calls an init() function, a script element, and a Canvas element with an id of pv3d. Add the init function to your script section and another function called render. You should have something like this:
{% highlight xml %}
<mx:Application xmlns:mx="http://www.adobe.com/2006/mxml" layout="absolute"

	creationComplete="init(event)">

	

	<mx:Script>

		<![CDATA[

			private function init(event:Event):void {



			}

			

			private function render(event:Event):void {

				

			}

		]]>

	</mx:Script>

	<mx:Canvas id="pv3d" height="100%" width="100%"/>

</mx:Application>
{% endhighlight %}
In your init method you are going to set up the papervision scene. The scene is where you later will place all your objects, lights, and cameras.

The idea is to create a UIComponent as a child of the canvas and add a papervision scene to UIComponent. If you are curious about the papervision object used here you should reference the documentation as you program. Your init function should look like this: 
{% highlight actionscript %}
private var renderer:BasicRenderEngine = new BasicRenderEngine();

private var scene:Scene3D = new Scene3D();

private var camera:Camera3D = new Camera3D();

private var viewport:Viewport3D;

private function init(event:Event):void {

	viewport = new Viewport3D(pv3d.width, pv3d.height, true, true);

	

	var uiComp:UIComponent = new UIComponent();

	pv3d.addChild(uiComp);

	uiComp.addChild(viewport);

	

	camera.z = -500;

	pv3d.addEventListener(Event.ENTER_FRAME, render);

}
{% endhighlight %}
Now implement the render function that we use as an event handler for the ENTER_FRAME event. 
{% highlight actionscript %}
private function renderFrame(event:Event):void {

	renderer.renderScene(scene, camera, viewport);

}
{% endhighlight %}
Thats it! You are good to go with a base papervision3D project. Now when you render it you won't see any 3d object, just an empty scene. But this is a good place to get you started for all the papervision tutorials that already exist out there.
