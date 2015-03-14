# WebDrum
A WebAudio drum synthesizer.  

### Installation

WebDrum requires [WebNoise][web-noise-repo] and [WebAHDSR][web-ahdsr-repo], which are included in this repository.  
Include `src/web-noise.js`, `src/web-ahdsr.js` and `src/web-drum.js` in your HTML document:  

	<script type="text/javascript" src="<path-to-web-noise>/src/web-noise.js"></script>
	<script type="text/javascript" src="<path-to-web-noise>/src/web-ahdsr.js"></script>
	<script type="text/javascript" src="<path-to-web-noise>/src/web-drum.js"></script>

### Usage:

WebDrum requires an AudioContext:  

	// Create an AudioContext:
	var context = null;
	if (typeof window.AudioContext != 'undefined') {
		context = new AudioContext();
	} else if (typeof window.webkitAudioContext != 'undefined') {
		context = new webkitAudioContext();
	}

	if (!context) {
		console.log('Web Audio API is not supported.');
		return;
	}

	// Create a WebDrum instance and call connect() to connect it to context.destination:
	var drum = new WebDrum(context);
	drum.connect();

If you don't want to connect a WebDrum instance directly to its context.destination, you can pass another AudioNode object to `connect()`. For example, connect a lowpass filter between a WebDrum and `context.destination`:  

	// Create a lowpass filter:
	var filter = context.createBiquadFilter();
	filter.type = 'lowpass';
	filter.frequency.value = 1000.0;

	// Connect WebDrum to the filter, then connect the filter to context.destination:
	drum.connect(filter);
	filter.connect(context.destination);

Call `disconnect()` to disconnect a WebDrum instance from whatever node it is currently connected to:  

	drum.disconnect();

Call `trigger()` to play the drum:  

	drum.trigger();

### Sound Configuration:

A WebDrum instance consists of two signal chains:  

* A tone oscillator with pitch, filter frequency and amplitude modulation via AHDSR envelopes.
* A noise generator with filter frequency and amplitude modulation via AHDSR envelopes.

The `tone` chain consists of the following objects:  

* `drum.tone` - [OscillatorNode][oscillator-node-docs]
* `drum.toneAmp` - [GainNode][gain-node-docs]
* `drum.toneFilter` - [BiquadFilterNode][filter-node-docs]
* `drum.tonePitchEnv` - [WebAHDSR][web-ahdsr-repo]
* `drum.toneAmpEnv` - [WebAHDSR][web-ahdsr-repo]
* `drum.toneFilterEnv` - [WebAHDSR][web-ahdsr-repo]

The `noise` chain consists of the following objects:  

* `drum.noise` - [WebNoise][web-noise-repo]
* `drum.noiseAmp` - [GainNode][gain-node-docs]
* `drum.noiseFilter` - [BiquadFilterNode][filter-node-docs]
* `drum.noiseAmpEnv` - [WebAHDSR][web-ahdsr-repo]
* `drum.noiseFilterEnv` - [WebAHDSR][web-ahdsr-repo]

The `tone` and `noise` chains are joined by the `mix` object, which is a [GainNode][gain-node-docs]:  

	// Set the overall level:
	drum.mix.gain.value = 0.5;

View `/demo/index.html` for example configurations for common drum sounds.  

### License:
WebDrum is made available under the MIT License. See LICENSE.txt for details.  

### Credits:
WebDrum was created by [Tony Wallace](http://tonywallace.ca).  

[web-noise-repo]: https://github.com/irritant/WebNoise
[web-ahdsr-repo]: https://github.com/irritant/WebAHDSR
[oscillator-node-docs]: https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode
[gain-node-docs]: https://developer.mozilla.org/en-US/docs/Web/API/GainNode
[filter-node-docs]: https://developer.mozilla.org/en-US/docs/Web/API/BiquadFilterNode

