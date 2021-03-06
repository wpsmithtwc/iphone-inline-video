# iphone-inline-video

> Make videos playable inline on Safari on iPhone (prevents automatic fullscreen)

[![gzipped size](https://badges.herokuapp.com/size/github/bfred-it/iphone-inline-video/gh-pages/dist/iphone-inline-video.browser.js?gzip=true&label=gzipped%20size)](#readme) [![iOS 8 and 9.3 supported](https://img.shields.io/badge/iOS%20Safari-8%20%E2%80%93%209.3-brightgreen.svg)](#no-link) [![Travis build status](https://api.travis-ci.org/bfred-it/iphone-inline-video.svg?branch=gh-pages)](https://travis-ci.org/bfred-it/iphone-inline-video)  [![npm version](https://img.shields.io/npm/v/iphone-inline-video.svg)](https://www.npmjs.com/package/iphone-inline-video) 

Try the [audio+video demo](http://bfred-it.github.io/iphone-inline-video/demo/) on your iPhone or the [silent autoplay demo](http://bfred-it.github.io/iphone-inline-video/demo/autoplay.html)

[![Demo](demo/demo-preview.gif)](http://bfred-it.github.io/iphone-inline-video/demo/)

## Main features

- ~1KB, standalone (no frameworks required)
- No setup: include it, call `makeVideoPlayableInline(video)`, [done](#usage)
- No custom API for playback, you can just call `video.play()` on `click`
- Supports **audio**
- Supports [autoplay](#usage-with-autoplaying-videos) on silent videos
- Doesn't need canvas
- Doesn't create new elements/wrappers
- It works with existing players like jPlayer

Limitations:

- Needs user interaction to play videos with sound (standard iOS limitation)
- Currently limited to iPhone, untested on Android, [unneeded on iPad](https://github.com/bfred-it/iphone-inline-video/pull/22#issuecomment-211822532)
- [Known issues](https://github.com/bfred-it/iphone-inline-video/labels/known%20issue)

## Install

```sh
npm install --save iphone-inline-video
```
```js
import makeVideoPlayableInline from 'iphone-inline-video';
```

If you don't use node/babel, include this:

```html
<script src="dist/iphone-inline-video.browser.js"></script>
```

## Usage

You will need:

- a `<video>` element  

	```html
	<video src="file.mp4"></video>
	```
	
- the CSS to hide the play button overlay  

	```css
	video::-webkit-media-controls-start-playback-button {
	  display:none;
	}
	```
	
- the activation call  

	```js
	// one video
	var video = document.querySelector('video');
	makeVideoPlayableInline(video);
	```
	
	```js
	// or if you're already using jQuery:
	var video = $('video').get(0);
	makeVideoPlayableInline(video);
	```
	
	```js
	// or if you have multiple videos:
	$('video').get().forEach(makeVideoPlayableInline)
	```

Done! It will only be enabled on iPhones and iPod Touch devices.

Now you can keep using it just like you would on a desktop. Run `video.play()`, `video.pause()`, listen to events with `video.addEventListener()` or `$(video).on()`, etc...

**BUT** you still need user interaction to play the audio, so do something like this:

```js
makeVideoPlayableInline(video);
video.addEventListener('touchstart', function () {
	video.play();
});
```

If at some point you want to open the video in fullscreen, use the standard (but still prefixed) `webkitEnterFullScreen()` API, but it has some [caveats.](https://github.com/bfred-it/iphone-inline-video/issues/33)

## Usage with audio-less videos

If your video doesn't have an audio track, then you need this:

```js
makeVideoPlayableInline(video, /* hasAudio */ false);
```

This uses a different behavior to play the video, so it might [not be particularly reliable](https://github.com/bfred-it/iphone-inline-video/issues/31) on slow connections. It's not thoroughly tested.

## Usage with autoplaying videos

You can also have **silent** videos autoplay. This module can load and play the video without user interaction, but not play the audio, so you **have to** set the `hasAudio` to `false`

```js
makeVideoPlayableInline(video, /* hasAudio */ false);
```

Once that's run, if `video` has the `autoplay` attribute, it will automatically start playing:

```html
<video autoplay muted src="video.mp4"></video>
```

## License

MIT © [Federico Brigante](http://twitter.com/bfred_it)
