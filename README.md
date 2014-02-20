*This repository is a mirror of the [component](http://component.io) module [sebpiq/audioparam](http://github.com/sebpiq/audioparam). It has been modified to work with NPM+Browserify. You can install it using the command `npm install npmcomponent/sebpiq-audioparam`. Please do not open issues or send pull requests against this repo. If you have issues with this repo, report it to [npmcomponent](https://github.com/airportyh/npmcomponent).*
AudioParam
============

With the current [Web Audio API](https://dvcs.w3.org/hg/audio/raw-file/tip/webaudio/specification.html#AudioParam) version, you cannot instantiate `AudioParam`.

This library implements a simple hack that allows you to instantiate a real `AudioParam`
which you can then use to build custom audio nodes with `ScriptProcessorNode`.

Usage
=======

**note :** a full example is available [here](http://sebpiq.github.io/AudioParam/test/example.html).

Download the latest stable release from [dist/](https://github.com/sebpiq/AudioParam/tree/master/dist), and put the file in your webpage. Then create an `AudioParam` like so :

```javascript

var myAudioParam = new AudioParam(audioContext, defaultValue)
```

The created `AudioParam` provides a method `AudioParam.connect(node, input)`, which allows you to connect it to a node. For example :

```
var param = new AudioParam(audioContext, 0.5)
  , customProcess = audioContext.createScriptProcessor(256, 3, 1)
  , channelMerger = audioContext.createChannelMerger(2)
  , paramData, audioSourceData

someAudioSource.connect(channelMerger, 0, 0)
param.connect(channelMerger, 1)

customProcess.onaudioprocess = function(event) {
  audioSourceData = event.inputBuffer.getChannelData(0)
  paramData = event.inputBuffer.getChannelData(1)
  // YOUR CUSTOM PROCESS HERE
}

customProcess.param = param
```
