# streaming-media-encoder

A dead simple way to deliver media correctly formatted to streaming devices. Supports any media that supports ReadStreams.

`npm install streaming-media-encoder`

```
var encoder = require('streaming-media-encoder');
var engine = encoder.profile(encoder.profiles.CHROMECAST, file.size);
```

## This system relies heavily on you providing Streams on demand.
This can be a local file, or any byte range supported ReadStream.

```
engine.on("streamNeeded", function (startByte, endByte, cb) {
    cb(fs.createReadStream(file, {start: startByte, end: endByte}));
});
```

## Detecting if Transcoding is even necessary
If isSupported is true, then you can just send the file to the streaming device.
If isSupported is false, you will have to encode the media.
```
engine.canPlay(function (isSupported));
```

## Optionally you can supply startTime to the encode function, to seek.
In this example `res` is an Express response object.
```
encoder.encode(engine, {
        startTime: "00:00"
    }, function (stream) {
      stream.pipe(res, {end: true});
     }
);
```