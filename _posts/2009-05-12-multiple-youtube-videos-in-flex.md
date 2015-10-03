---
layout: post
title:  "Playing Multiple YouTube Videos in Flex"
date:   2009-05-12 21:58:00
categories: flex youtube
---
Recently, a friend of mine had a relatively simple problem that I told him I would look into. He was building a website in Flex and wanted a video gallery that would play YouTube videos in the main content area when a video thumbnail was clicked. Well that seems simple enough! A YouTube video, player actually, is just a SWF file so it is possible to load in a video by way of the SWFLoader component in Flex. You can get the url to the YouTube player for the video by looking in the embed code that YouTube give you. It looks like this: <http://www.youtube.com/v/Q5im0Ssyyus&hl=en&fs=1>

So after throwing together a quick sample I see that this works fine and the video plays in the Flex application. Remember that this is a movie gallery and we want to display multiple movies. Second movie added and no work! Yeah, it doesn't work. The first movie never gets fully unloaded and so the second movie never loads in the SwfLoader.

After a little research I stumbled on this article which solved the multiple video issue: <http://apiblog.youtube.com/2009/01/flex-embedded-player-christmas-story.html>

The issue is that the YouTube video player is an AS2 SWF while the Flex application is AS3 and the interaction between the two has some problems as we saw earlier. The solution is to have a AS2 SWF object that acts as "bridge" between your application and the YouTube video player. This bridge SWF file responds to two external function calls, load and dispose. So all you need to do is include the youtubebridge.swf in your deployment and make the two calls, load or dispose, to the bridge using LocalConnection.
