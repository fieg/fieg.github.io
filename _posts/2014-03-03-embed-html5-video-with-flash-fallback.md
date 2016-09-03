---
layout: post
title: Embed HTML5 Video with Flash Fallback
---

There are these days that you are presented with a very interesting assignment. Let’s say you have to embed some videos into the design of a website. The videos should automatically start when the page is loaded and should be looping. Also there should be no controls visible and it should properly fallback from html5 to flash to eventually a static image, when one of these methods isn’t available.

The process consists from the following steps:

* Encode video to the proper codecs for all browsers (OGV, M4V and FLV)
* Create a Flash file (SWF) that can play a Flash Video (FLV)
* Create fallback images
* Implement html5 video
* Implement Flash with SWFObject

Disclaimer: There are probably hundred ways to do this and I am sure there are way more efficient ways then mine, so feel free to use your own preferred method.



## 1. Encode video to video to proper codecs for all browsers

To get maximum compatibility with all modern browsers (Chrome, Firefox and Safari) we have to encode the video to Ogg for Firefox 3.5, Chrome 4 and Opera 10.5. For Safari, iProducts and IE9 we have to encode the video to H.264.

### Encode to Ogg with firefogg.org

Firefogg is a plug-in for Firefox, you can install it by navigating to http://firefogg.org and click on the install button.



When firefogg is installed click on “Make Ogg Video” and select your source video file. You are presented some options, which I will not explain here. The only options I really used were the video and auto quality and I selected “No audio” as I didn’t need it.

Now click “Encode to File” to start the encoding.

### Encode to M4V with HandBrake

Download and install HandBrake. Select your source video file. From the right-hand side you can choose the “iPhone & iPod Touch” preset, this will set most of the options you need. I suggest that you check the option “Web Optimized”. The rest of the options I will leave up to you. Also for my project I did not need an audio track, so on the audio tab I selected “None” by the track.

Now click “Start” to start encoding.

## Encode to FLV with Adobe Media Encoder

For the Flash Video I used Adobe Media Encode CS4. Start AME and add the source video file to the queue. Select the preset “FLV same as source”. When needed you can customize more options, but I will leave that up to you.



Click “Start Queue” to start the encoding.

## 2. Create a Flash file (SWF) that can play a Flash Video

Start Flash. Make the document to same size as the video. Add the FLVPlayback component to the scene with the same size as the scene itself. Give it an instance name like “comFlvplayer”.

Create a new ActionScript 3.0 file and add the following contents.

```
package {
  import flash.display.Sprite;
  import fl.video.*;

  public class FLVPlayer extends Sprite {
    public function FLVPlayer() {
      var parameters=this.loaderInfo.parameters;
      comFlvplayer.source=parameters.src;
      comFlvplayer.addEventListener(VideoEvent.COMPLETE, this.completeHandler);
    }

    // (optional) Loop the video when it completes
    function completeHandler(event:VideoEvent):void {
      comFlvplayer.seek(0);
      comFlvplayer.play();
    }
  }
}
```

This makes your Flash file read a variable called “src” so you can dynamically set which FLV file it has to play.

Save the ActionScript file as “FLVPlayer.as”. Save the regular Flash document as “FLVPlayer.fla”. Now, publish it as “FLVPlayer.swf”.

## 3. Create a fallback image

Now that we have created all the video files, it is time to create a nice image, for browser that don’t support both html5 and flash.

What I basically did was open the video file in a media player and made a screenshot. Next I cropped the screenshot in Photoshop to fit the video and saved it.

## 4. Implement html5 video

Ok, all the necessary video files have been created, now we can start implementing them into our design. We first start with the html5 video tag.

Copy and paste the following code into your webpage.

```html
<div id="demo-video-flash"><!-- wrapped in a div for the flash fallback -->
   <video id="demo-video" height="300" width="270" autoplay loop>
      <source src="path/to/your/video.m4v" type="video/mp4" /> <!-- MPEG4 for Safari -->
      <source src="path/to/your/video.ogv" type="video/ogg" /> <!-- Ogg Theora for Firefox 3.1b2 -->

      <!-- this is the image fallback in case flash is not support either -->
      <img src="path/to/your/poster.jpg"/>
   </video>
</div>
```

What this does is add a video player into your webpage, with two video file formats. Each browser will pick the video file they support or prefer.

Because of the autoplay and loop attribute, it should automatically start and loop when the video is finished (by ignoring the controls attribute, the browser should not display any controls for play, pause, rewind, etc.).

Note that Firefox currently doesn’t support the loop attribute, but we will tackle that in the next step.

## 5. Implement Flash with SWFObject

The final step. In this step we will implement the flash fallback for browsers that don’t support html5 and also the fallback image in case the browser doesn’t support flash either.

Download and install swfobject and jQuery into your website.

Now copy and paste the following code into your webpage.

```js
jQuery(document).ready(function() {
   var v = document.createElement("video"); // Check if the browser supports the video tag
   if ( !v.play ) { // If no, use Flash.
      var params = {
         allowfullscreen: "false",
         allowscriptaccess: "always",
         wmode: "transparent"
      };

      var flashvars = {
         src: "path/to/your/video.flv"
      };

      swfobject.embedSWF("path/to/your/FLVPlayer.swf", "demo-video-flash", "270", "296", "9.0.0", "path/to/your/swfobject/expressInstall.swf", flashvars, params);
   }
   else {
      // fix for firefox not looping
      var myVideo = document.getElementById('demo-video');

      if (!(typeof myVideo.loop == 'boolean')) { // loop supported
         myVideo.addEventListener('ended', function () {
            this.currentTime = 0;
            this.play();
         }, false);
      }
   }
});
```

This code checks if the video tag is supported by the browser. When it’s not support the flash video will be embedded with swfobject. Swfobject automatically checks if flash is installed, if not, it will do nothing and the fallback image is shown.

Then there is the fix for Firefox to make it loop the video. This is done by adding a event handler on the ‘ended’ event. When the event is triggered, the video is rewind and played again.

You now should have a working video with proper fallbacks. Well done.
