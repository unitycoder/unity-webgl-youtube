# unity-webgl-youtube
Add youtube embeds to a Unity WebGL build.

## What?
Videos are played in iframes, just like normal youtube embeds. The iframes are transformed with 3D CSS, and are then visible behind transparent areas of the Unity player.

There's a demo here: https://tylergregg.itch.io/unity-webgl-youtube. I included a [simple FPS controller](https://sharpcoderblog.com/blog/unity-3d-fps-controller) that was the second google result for "unity fps controller."  

Built with Unity 2023.1.13f1. It may very well work with earlier versions, but I haven't tried.

## Huh?
Ingredients:  

This shader, which masks everything behind it except the camera background: https://stackoverflow.com/questions/72477399/occluding-parts-of-an-object-behind-a-transparent-object  

This Unity package, which makes the WebGL canvas background transparent: https://forum.unity.com/threads/how-can-i-make-the-canvas-transparent-on-webgl.831580/#post-8310864  

[three.js](https://threejs.org/) and its [CSS 3D Renderer](https://threejs.org/docs/#examples/en/renderers/CSS3DRenderer).

Some quaternion magic from this thread, which turned out not to be entirely correct, OR (more likely) I've made a mistake somewhere: https://stackoverflow.com/questions/18066581/convert-unity-transforms-to-three-js-rotations  

## How?
Extract all the files into your project. There's a sample implementation in the `YoutubeDemo` scene.

In Project Settings under Resolution & Presentation, select the "Youtube" WebGL template.  

Set your main camera clear flags to `Solid Color` with alpha = 0. Add the `WebCameraSync` and `WebClickHandler` components to the camera.

You can either use the provided `YoutubeVideoPlayer` prefab, or create a quad, add the `CSS3DIframe` component, and set its xy scale to 4.8 x 3.6. This will result in a 480px x 360px iframe.
Note that only y-axis rotation is supported at the moment.

Set the iframe's `id` to something unique (like "youtubePlayer1").  
You can either add video IDs to the `videoIdList` OR select a `listType` and set `listId` to a playlist ID or youtube username. Doing the latter will override the former.

Drag the `YoutubeManager` prefab into your scene. This script sets the video titles, and also delays loading youtube's API until after Unity tells the browser to create the iframes. I don't know why this is necessary, but it doesn't work otherwise with dynamically added iframes.  

While you can run this in editor, the video players only work in a WebGL build.  
In Firefox, you may need to turn off "enhanced tracking protection" for the current site. Click the shield on the left side of the address bar.

## What else?
Interaction is limited to click to play/pause/unpause and playlist next/previous buttons. Interacting with the iframes directly would require using the mouse cursor and temporarily disabling pointer events on the Unity player. This is possible, but awkward.

Fullscreen mode doesn't work yet.  

The iframes lag slightly when the camera moves, even on a fast machine. I *think* I alleviated this somewhat by delaying Unity input handling and camera movement until the three.js scene has finished rendering, but it could also be my imagination.

My quaternion math is a mess. Help.  
