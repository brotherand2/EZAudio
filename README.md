![alt text](https://s3-us-west-1.amazonaws.com/ezaudio-media/EZAudioJumbo-Alt.png "EZAudioLogo")

#EZAudio
A simple, intuitive audio framework for iOS and OSX.

##Features


**Awesome Components**

I've designed six core components to allow you to immediately get your hands dirty recording, playing, and visualizing audio data. These components simply plug into each other and build on top of the high-performance, low-latency AudioUnits API and give you an easy to use API written in Objective-C instead of pure C.

[EZMicrophone](#EZMicrophone)

A microphone class that provides its delegate audio data from the default device microphone with one line of code.

[EZRecorder](#EZRecorder)

A recorder class that provides a quick and easy way to write audio files from any datasource.

[EZAudioFile](#EZAudioFile)

An audio file class that reads/seeks through audio files and provides useful delegate callbacks. 

[EZOutput](#EZOutput)

An output class that will playback any audio it is provided by its datasource. 

[EZAudioPlot](#EZAudioPlot)

A CoreGraphics-based audio waveform plot capable of visualizing any float array as a buffer or rolling plot.

[EZAudioPlotGL](#EZAudioPlotGL)

An OpenGL-based, GPU-accelerated audio waveform plot capable of visualizing any float array as a buffer or rolling plot.

**Cross Platform**

`EZAudio` was designed to work transparently across all iOS and OSX devices. This means one universal API whether you're building for Mac or iOS. For instance, under the hood an `EZAudioPlot` knows that it will subclass a UIView for iOS or an NSView for OSX and the `EZMicrophone` knows to build on top of the RemoteIO AudioUnit for iOS, but defaults to the system defaults for input and output for OSX.

##<a name="Examples">Examples & Docs

Within this repo you'll find the examples for iOS and OSX to get you up to speed using each component and plugging them into each other. With just a few lines of code you'll be recording from the microphone, generating audio waveforms, and playing audio files like a boss. See the full Getting Started guide for an interactive look into each of components.

### Example Projects

**_EZAudioCoreGraphicsWaveformExample_** 

![CoreGraphicsWaveformExampleGif](https://cloud.githubusercontent.com/assets/1275640/8516226/1eb885ec-2366-11e5-8d76-3a4b4d982eb0.gif)

Shows how to use the `EZMicrophone` and `EZAudioPlot` to visualize the audio data from the microphone in real-time. The waveform can be displayed as a buffer or a rolling waveform plot (traditional waveform look). 

**_EZAudioOpenGLWaveformExample_**

![OpenGLWaveformExampleGif](https://cloud.githubusercontent.com/assets/1275640/8516234/499f6fd2-2366-11e5-9771-7d0afae59391.gif)

Shows how to use the `EZMicrophone` and `EZAudioPlotGL` to visualize the audio data from the microphone in real-time. The drawing is using OpenGL so the performance much better for plots needing a lot of points.

**_EZAudioPlayFileExample_**

![PlayFileExample](https://cloud.githubusercontent.com/assets/1275640/8516245/711ca232-2366-11e5-8d20-2538164f3307.gif)

Shows how to use the `EZAudioPlayer` and `EZAudioPlotGL` to playback, pause, and seek through an audio file while displaying its waveform as a buffer or a rolling waveform plot.

**_EZAudioRecordWaveformExample_**

![RecordWaveformExample](https://cloud.githubusercontent.com/assets/1275640/8516310/86da80f2-2367-11e5-84aa-aea25a439a76.gif)

Shows how to use the `EZMicrophone`, `EZRecorder`, and `EZAudioPlotGL` to record the audio from the microphone input to a file while displaying the audio waveform of the incoming data. You can then playback the newly recorded audio file using AVFoundation and keep adding more audio data to the tail of the file.

**_EZAudioWaveformFromFileExample_**

![WaveformExample](https://cloud.githubusercontent.com/assets/1275640/8516597/f27240ea-236a-11e5-8ecd-68cf05b7ce40.gif)

Shows how to use the `EZAudioFile` and `EZAudioPlot` to animate in an audio waveform for an entire audio file. 

**_EZAudioPassThroughExample_**

![PassthroughExample](https://cloud.githubusercontent.com/assets/1275640/8516692/7abfbe36-236c-11e5-9d69-4f82956177b3.gif)

Shows how to use the `EZMicrophone`, `EZOutput`, and the `EZAudioPlotGL` to pass the microphone input to the output for playback while displaying the audio waveform (as a buffer or rolling plot) in real-time. 

**_EZAudioFFTExample_**

![FFTExample](https://cloud.githubusercontent.com/assets/1275640/8516750/4bf07522-236d-11e5-9112-685d80424e5f.gif)

Shows how to calculate the real-time FFT of the audio data coming from the `EZMicrophone` and the Accelerate framework. The audio data is plotted using two `EZAudioPlots` for the time and frequency displays.

### Documentation
The official documentation for EZAudio can be found here: http://cocoadocs.org/docsets/EZAudio/0.9.1/
<br>You can also generate the docset yourself using appledocs by running the appledocs on the EZAudio source folder.

##<a name="GettingStarted">Getting Started
To begin using `EZAudio` you must first make sure you have the proper build requirements and frameworks. Below you'll find explanations of each component and code snippets to show how to use each to perform common tasks like getting microphone data, updating audio waveform plots, reading/seeking through audio files, and performing playback.

###Build Requirements
**iOS**
- 6.0+

**OSX**
- 10.8+

###Frameworks
**iOS**
- AudioToolbox
- AVFoundation
- GLKit

**OSX**
- AudioToolbox
- AudioUnit
- CoreAudio
- QuartzCore
- OpenGL
- GLKit

###<a name="AddingToProject">Adding To Project
You can add EZAudio to your project in a few ways: <br><br>1.) The easiest way to use EZAudio is via <a href="http://cocoapods.org/", target="_blank">Cocoapods</a>. Simply add EZAudio to your <a href="http://guides.cocoapods.org/using/the-podfile.html", target="_blank">Podfile</a> like so:

`
pod 'EZAudio', '~> 0.9.1'
`

####<a name="AmazingAudioEngineCocoapod">Using EZAudio & The Amazing Audio Engine
If you're also using the Amazing Audio Engine then use the `EZAudio/Core` subspec like so:

`
pod 'EZAudio/Core', '~> 0.9.1'
`

2.) Alternatively, you could clone or fork this repo and just drag and drop the source into your project. 

##<a name="CoreComponents"></a>Core Components

`EZAudio` currently offers six audio components that encompass a wide range of functionality. In addition to the functional aspects of these components such as pulling audio data, reading/writing from files, and performing playback they also take special care to hook into the interface components to allow developers to display visual feedback (see the [Interface Components](#InterfaceComponents) below).

###<a name="EZAudioFile"></a>EZAudioFile
Provides simple read/seek operations, pulls waveform amplitude data, and provides the `EZAudioFileDelegate` to notify of any read/seek action occuring on the `EZAudioFile`. This can be thought of as the NSImage/UIImage equivalent of the audio world.

**_Relevant Example Projects_**
- EZAudioWaveformFromFileExample (iOS)
- EZAudioWaveformFromFileExample (OSX)

####Opening An Audio File
To open an audio file create a new instance of the `EZAudioFile` class.
```objectivec
// Declare the EZAudioFile as a strong property
@property (nonatomic, strong) EZAudioFile *audioFile;

...

// Initialize the EZAudioFile instance and assign it a delegate to receive the read/seek callbacks
self.audioFile = [EZAudioFile audioFileWithURL:[NSURL fileURLWithPath:@"/path/to/your/file"] 
									   delegate:self];
```

####Getting Waveform Data

The EZAudioFile allows you to quickly fetch waveform data from an audio file with as much or little detail as you'd like.
```objectivec
__weak typeof (self) weakSelf = self;
[self.audioFile getWaveformDataWithNumberOfPoints:1024
                                  completionBlock:^(float **waveformData,
                                                    int length)
{
     [weakSelf.audioPlot updateBuffer:waveformData[0]
                       withBufferSize:length];
}];
```

####Reading From An Audio File

Reading audio data from a file requires you to create an AudioBufferList to hold the data. The `EZAudio` utility function, `audioBufferList`, provides a convenient way to get an allocated AudioBufferList to use. There is also a utility function, `freeBufferList:`, to use to free (or release) the AudioBufferList when you are done using that audio data.

**Note: You have to free the AudioBufferList, even in ARC.**
```objectivec
// Allocate an AudioBufferList to hold the audio data (the client format is the non-compressed in-app format that is used for reading, it's different than the file format which is usually something compressed like an mp3 or m4a)
AudioStreamBasicDescription clientFormat = [self.audioFile clientFormat];
UInt32 numberOfFramesToRead = 512;
UInt32 channels = clientFormat.mChannelsPerFrame;
BOOL isInterleaved = [EZAudioUtilities isInterleaved:clientFormat];
AudioBufferList *bufferList = [EZAudioUtilities audioBufferListWithNumberOfFrames:numberOfFramesToRead
                                                                 numberOfChannels:channels
                                                                      interleaved:isInterleaved];

// Read the frames from the EZAudioFile into the AudioBufferList
UInt32 framesRead;
UInt32 isEndOfFile;
[self.audioFile readFrames:numberOfFramesToRead
           audioBufferList:bufferList
                bufferSize:&framesRead
                       eof:&isEndOfFile]
```

When a read occurs the `EZAudioFileDelegate` receives two events.

An event notifying the delegate of the read audio data as float arrays:
```objectivec
-(void)     audioFile:(EZAudioFile *)audioFile
            readAudio:(float **)buffer
       withBufferSize:(UInt32)bufferSize
 withNumberOfChannels:(UInt32)numberOfChannels
{
    __weak typeof (self) weakSelf = self;
    dispatch_async(dispatch_get_main_queue(), ^{
        [weakSelf.audioPlot updateBuffer:buffer[0]
                          withBufferSize:bufferSize];
    });
}
```
and an event notifying the delegate of the new frame position within the `EZAudioFile`:
```objectivec
-(void)audioFile:(EZAudioFile *)audioFile updatedPosition:(SInt64)framePosition
{
    __weak typeof (self) weakSelf = self;
    dispatch_async(dispatch_get_main_queue(), ^{
		// Update UI
    });
}
```

####Seeking Through An Audio File

You can seek very easily through an audio file using the `EZAudioFile`'s seekToFrame: method. The `EZAudioFile` provides a `totalFrames` method to provide you the total amount of frames in an audio file so you can calculate a proper offset.
```objectivec
// Get the total number of frames for the audio file
SInt64 totalFrames = [self.audioFile totalFrames];

// Seeks halfway through the audio file
[self.audioFile seekToFrame:(totalFrames/2)];
```
When a seek occurs the `EZAudioFileDelegate` receives the seek event:
```objectivec
-(void)audioFile:(EZAudioFile *)audioFile updatedPosition:(SInt64)framePosition
{
    __weak typeof (self) weakSelf = self;
    dispatch_async(dispatch_get_main_queue(), ^{
		// Update UI
    });
}
```
###<a name="EZMicrophone"></a>EZMicrophone
Provides access to the default device microphone in one line of code and provides delegate callbacks to receive the audio data as an AudioBufferList and float arrays.

**_Relevant Example Projects_**
- EZAudioCoreGraphicsWaveformExample (iOS)
- EZAudioCoreGraphicsWaveformExample (OSX)
- EZAudioOpenGLWaveformExample (iOS)
- EZAudioOpenGLWaveformExample (OSX)
- EZAudioRecordExample (iOS)
- EZAudioRecordExample (OSX)
- EZAudioPassThroughExample (iOS)
- EZAudioPassThroughExample (OSX)
- EZAudioFFTExample (iOS)
- EZAudioFFTExample (OSX)

####Creating A Microphone

Create an `EZMicrophone` instance by declaring a property and initializing it like so:

```objectivec
// Declare the EZMicrophone as a strong property
@property (nonatomic, strong) EZMicrophone *microphone;

...

// Initialize the microphone instance and assign it a delegate to receive the audio data callbacks
self.microphone = [EZMicrophone microphoneWithDelegate:self];
```
Alternatively, you could also use the shared `EZMicrophone` instance and just assign its `EZMicrophoneDelegate`.
```objectivec
// Assign a delegate to the shared instance of the microphone to receive the audio data callbacks
[EZMicrophone sharedMicrophone].delegate = self;
```

####Getting Microphone Data

To tell the microphone to start fetching audio use the `startFetchingAudio` function.

```objectivec
// Starts fetching audio from the default device microphone and sends data to EZMicrophoneDelegate
[self.microphone startFetchingAudio];
```
Once the `EZMicrophone` has started it will send the `EZMicrophoneDelegate` the audio back in a few ways.
An array of float arrays:
```objectivec
/**
 The microphone data represented as non-interleaved float arrays useful for:
    - Creating real-time waveforms using EZAudioPlot or EZAudioPlotGL
    - Creating any number of custom visualizations that utilize audio!
 */
-(void)   microphone:(EZMicrophone *)microphone
    hasAudioReceived:(float **)buffer
      withBufferSize:(UInt32)bufferSize
withNumberOfChannels:(UInt32)numberOfChannels 
{
    __weak typeof (self) weakSelf = self;
	// Getting audio data as an array of float buffer arrays that can be fed into the EZAudioPlot, EZAudioPlotGL, or whatever visualization you would like to do with the microphone data.
	dispatch_async(dispatch_get_main_queue(),^{
		// Visualize this data brah, buffer[0] = left channel, buffer[1] = right channel
		[weakSelf.audioPlot updateBuffer:buffer[0] withBufferSize:bufferSize];
    });
}
```
or the AudioBufferList representation:
```objectivec
/**
 The microphone data represented as CoreAudio's AudioBufferList useful for:
    - Appending data to an audio file via the EZRecorder
    - Playback via the EZOutput
    
 */
-(void)    microphone:(EZMicrophone *)microphone
        hasBufferList:(AudioBufferList *)bufferList
       withBufferSize:(UInt32)bufferSize
 withNumberOfChannels:(UInt32)numberOfChannels 
{
	// Getting audio data as an AudioBufferList that can be directly fed into the EZRecorder or EZOutput. Say whattt...
}
```
####Pausing/Resuming The Microphone

Pause or resume fetching audio at any time like so:
```objectivec
// Stop fetching audio
[self.microphone stopFetchingAudio];

// Resume fetching audio
[self.microphone startFetchingAudio];
```

Alternatively, you could also toggle the `microphoneOn` property (safe to use with Cocoa Bindings)
```objectivec
// Stop fetching audio
self.microphone.microphoneOn = NO;

// Start fetching audio
self.microphone.microphoneOn = YES;
```

###<a name="EZOutput"></a>EZOutput
Provides flexible playback to the default output device by asking the `EZOutputDataSource` for audio data to play. Doesn't care where the buffers come from (microphone, audio file, streaming audio, etc). As of 1.0.0 the `EZOutputDataSource` has been simplified to have only one method to provide audio data to your `EZOutput` instance.
```objectivec
// The EZOutputDataSource should fill out the audioBufferList with the given frame count. The timestamp is provided for sample accurate calculation, but for basic use cases can be ignored.
- (OSStatus)        output:(EZOutput *)output
 shouldFillAudioBufferList:(AudioBufferList *)audioBufferList
        withNumberOfFrames:(UInt32)frames
                 timestamp:(const AudioTimeStamp *)timestamp;
```

**_Relevant Example Projects_**
- EZAudioPlayFileExample (iOS)
- EZAudioPlayFileExample (OSX)
- EZAudioPassThroughExample (iOS)
- EZAudioPassThroughExample (OSX)

####Creating An Output

Create an `EZOutput` by declaring a property and initializing it like so:

```objectivec
// Declare the EZOutput as a strong property
@property (nonatomic, strong) EZOutput *output;
...

// Initialize the EZOutput instance and assign it a delegate to provide the output audio data
self.output = [EZOutput outputWithDataSource:self];
```
Alternatively, you could also use the shared output instance and just assign it an `EZOutputDataSource` if you will only have one EZOutput instance for your application.
```objectivec
// Assign a delegate to the shared instance of the output to provide the output audio data
[EZOutput sharedOutput].delegate = self;
```
####Playback Using An AudioBufferList

#####Setting The Input Format

When providing audio data the EZOutputDataSource will expect you to fill out the AudioBufferList provided with whatever `inputFormat` that is set on the `EZOutput`. By default the input format is a stereo, non-interleaved, float format (see [defaultInputFormat](http://cocoadocs.org/docsets/EZAudio/0.9.1/Classes/EZOutput.html#//api/name/defaultInputFormat) for more information). If you're dealing with a different input format (which is typically the case), just set the `inputFormat` property. For instance:
```objectivec
// Set a mono, float format with a sample rate of 44.1 kHz
AudioStreamBasicDescription monoFloatFormat = [EZAudioUtilities monoFloatFormatWithSampleRate:44100.0f];
[self.output setInputFormat:monoFloatFormat];
```
#####Implementing the EZOutputDataSource

An example of implementing the EZOutputDataSource is done internally in the `EZAudioPlayer` using an `EZAudioFile` to read audio from an audio file on disk like so:
```objectivec
- (OSStatus)        output:(EZOutput *)output
 shouldFillAudioBufferList:(AudioBufferList *)audioBufferList
        withNumberOfFrames:(UInt32)frames
                 timestamp:(const AudioTimeStamp *)timestamp
{
    if (self.audioFile)
    {
        UInt32 bufferSize; // amount of frames actually read
        BOOL eof; // end of file
        [self.audioFile readFrames:frames
                   audioBufferList:audioBufferList
                        bufferSize:&bufferSize
                               eof:&eof];
        if (eof && [self.delegate respondsToSelector:@selector(audioPlayer:reachedEndOfAudioFile:)]) 
        {
            [self.delegate audioPlayer:self reachedEndOfAudioFile:self.audioFile];
        }
        if (eof && self.shouldLoop)
        {
            [self seekToFrame:0];
        }
        else if (eof)
        {
            [self pause];
            [self seekToFrame:0];
            [[NSNotificationCenter defaultCenter] postNotificationName:EZAudioPlayerDidReachEndOfFileNotification
                                                                object:self];
        }
    }
    return noErr;
}
```

I created a sample project that uses the EZOutput to act as a signal generator to play sine, square, triangle, sawtooth, and noise waveforms. Here's a snippet of that code:
```objectivec
...
double const SAMPLE_RATE = 44100.0;

- (void)awakeFromNib
{
    //
    // Create EZOutput to play audio data with mono format (EZOutput will convert this mono, float "inputFormat" to a clientFormat, i.e. the stereo output format).
    //
    AudioStreamBasicDescription inputFormat = [EZAudioUtilities monoFloatFormatWithSampleRate:SAMPLE_RATE];
    self.output = [EZOutput outputWithDataSource:self inputFormat:inputFormat];
    [self.output setDelegate:self];
    self.frequency = 200.0;
    self.sampleRate = SAMPLE_RATE;
    self.amplitude = 0.80;
}

- (OSStatus)        output:(EZOutput *)output
 shouldFillAudioBufferList:(AudioBufferList *)audioBufferList
        withNumberOfFrames:(UInt32)frames
                 timestamp:(const AudioTimeStamp *)timestamp
{
    Float32 *buffer = (Float32 *)audioBufferList->mBuffers[0].mData;
    size_t bufferByteSize = (size_t)audioBufferList->mBuffers[0].mDataByteSize;
    double theta = self.theta;
    double frequency = self.frequency;
    double thetaIncrement = 2.0 * M_PI * frequency / SAMPLE_RATE;
    if (self.type == GeneratorTypeSine)
    {
        for (UInt32 frame = 0; frame < frames; frame++)
        {
            buffer[frame] = self.amplitude * sin(theta);
            theta += thetaIncrement;
            if (theta > 2.0 * M_PI)
            {
                theta -= 2.0 * M_PI;
            }
        }
        self.theta = theta;
    }
    else if (... other shapes in full source)
}
```

For the full implementation of the square, triangle, sawtooth, and noise functions here: (https://github.com/syedhali/SineExample/blob/master/SineExample/GeneratorViewController.m#L220-L305)

Once the `EZOutput` has started it will send the `EZOutputDelegate` the audio back as float arrays for visualizing. These are converted inside the EZOutput component from whatever input format you may have provided. For instance, if you provide an interleaved, signed integer AudioStreamBasicDescription for the `inputFormat` property then that will be automatically converted to a stereo, non-interleaved, float format when sent back in the delegate `playedAudio:...` method below:
An array of float arrays:
```objectivec
/**
 The output data represented as non-interleaved float arrays useful for:
    - Creating real-time waveforms using EZAudioPlot or EZAudioPlotGL
    - Creating any number of custom visualizations that utilize audio!
 */
- (void)       output:(EZOutput *)output
          playedAudio:(float **)buffer
       withBufferSize:(UInt32)bufferSize
 withNumberOfChannels:(UInt32)numberOfChannels
{
    __weak typeof (self) weakSelf = self;
    dispatch_async(dispatch_get_main_queue(), ^{
	// Update plot, buffer[0] = left channel, buffer[1] = right channel
    });
}
```

####Pausing/Resuming The Output

Pause or resume the output component at any time like so:
```objectivec
// Stop fetching audio
[self.output stopPlayback];

// Resume fetching audio
[self.output startPlayback];
```

###<a name="EZRecorder"></a>EZRecorder
Provides a way to record any audio source to an audio file. This hooks into the other components quite nicely to do something like plot the audio waveform while recording to give visual feedback as to what is happening.

*Relevant Example Projects*
- EZAudioRecordExample (iOS)
- EZAudioRecordExample (OSX)

####Creating A Recorder

To create an `EZRecorder` you must start with an AudioStreamBasicDescription, which is just a CoreAudio structure representing the audio format of a file. The `EZMicrophone` and `EZAudioFile` both provide the AudioStreamBasicDescription as properties (for the `EZAudioFile` use the clientFormat property) that you can use when initializing the `EZRecorder`.

```objectivec
// Declare the EZRecorder as a strong property
@property (nonatomic,strong) EZRecorder *recorder;

...

// Here's how we would initialize the recorder for an EZMicrophone instance
self.recorder = [EZRecorder recorderWithDestinationURL:[NSURL fileURLWithPath:@"path/to/file.caf"]
                                       andSourceFormat:microphone.audioStreamBasicDescription];
                                       
// Here's how we would initialize the recorder for an EZAudioFile instance
self.recorder = [EZRecorder recorderWithDestinationURL:[NSURL fileURLWithPath:@"path/to/file.caf"]
                                       andSourceFormat:audioFile.clientFormat];
```

##Recording Some Audio

Once you've initialized your `EZRecorder` you can append data by passing in an AudioBufferList and its buffer size like so:
```objectivec
// Append the microphone data coming as a AudioBufferList with the specified buffer size to the recorder
-(void)    microphone:(EZMicrophone *)microphone
        hasBufferList:(AudioBufferList *)bufferList
       withBufferSize:(UInt32)bufferSize
 withNumberOfChannels:(UInt32)numberOfChannels {
  // Getting audio data as a buffer list that can be directly fed into the EZRecorder. This is happening on the audio thread - any UI updating needs a GCD main queue block.
  if( self.isRecording ){
    [self.recorder appendDataFromBufferList:bufferList
                             withBufferSize:bufferSize];
  } 
}
```

##<a name="InterfaceComponents"></a>Interface Components
`EZAudio` currently offers two drop in audio waveform components that help simplify the process of visualizing audio.

###<a name="EZAudioPlot"></a>EZAudioPlot
Provides an audio waveform plot that uses CoreGraphics to perform the drawing. On iOS this is a subclass of UIView while on OSX this is a subclass of NSView. As of the 1.0.0 release, the waveforms are drawn using CALayers where compositing is done on the GPU. As a result, there have been some huge performance gains and CPU usage per real-time (i.e. 60 frames per second redrawing) plot is now about 2-3% CPU as opposed to the 20-30% we were experiencing before.

*Relevant Example Projects*
- EZAudioCoreGraphicsWaveformExample (iOS)
- EZAudioCoreGraphicsWaveformExample (OSX)
- EZAudioRecordExample (iOS)
- EZAudioRecordExample (OSX)
- EZAudioWaveformFromFileExample (iOS)
- EZAudioWaveformFromFileExample (OSX)
- EZAudioFFTExample (iOS)
- EZAudioFFTExample (OSX)

####Creating An Audio Plot

You can create an audio plot in the interface builder by dragging in a UIView on iOS or an NSView on OSX onto your content area. Then change the custom class of the UIView/NSView to `EZAudioPlot`.

![EZAudioPlotInterfaceBuilder](https://cloud.githubusercontent.com/assets/1275640/8532901/47d6f9ce-23e6-11e5-9766-d9969e630338.gif)

Alternatively, you can could create the audio plot programmatically

```objectivec
// Programmatically create an audio plot
EZAudioPlot *audioPlot = [[EZAudioPlot alloc] initWithFrame:self.view.frame];
[self.view addSubview:audioPlot];
```

####Customizing The Audio Plot

All plots offer the ability to change the background color, waveform color, plot type (buffer or rolling), toggle between filled and stroked, and toggle between mirrored and unmirrored (about the x-axis). For iOS colors are of the type UIColor while on OSX colors are of the type NSColor.

```objectivec
// Background color (use UIColor for iOS)
audioPlot.backgroundColor = [NSColor colorWithCalibratedRed:0.816 
                                                      green:0.349 
                                                       blue:0.255 
                                                      alpha:1];
// Waveform color (use UIColor for iOS)
audioPlot.color = [NSColor colorWithCalibratedRed:1.000 
                                            green:1.000 
                                             blue:1.000
                                            alpha:1];
// Plot type
audioPlot.plotType = EZPlotTypeBuffer;
// Fill
audioPlot.shouldFill = YES;
// Mirror
audioPlot.shouldMirror = YES;
```

####IBInspectable Attributes

Also, as of iOS 8 you can adjust the background color, color, gain, shouldFill, and shouldMirror parameters directly in the Interface Builder via the IBInspectable attributes:

![EZAudioPlotInspectableAttributes](https://cloud.githubusercontent.com/assets/1275640/8530670/288840c8-23d7-11e5-954b-644ed4ed67b4.png)

####Updating The Audio Plot

All plots have only one update function, `updateBuffer:withBufferSize:`, which expects a float array and its length.
```objectivec
// The microphone component provides audio data to its delegate as an array of float buffer arrays.
- (void)   microphone:(EZMicrophone *)microphone
     hasAudioReceived:(float **)buffer
       withBufferSize:(UInt32)bufferSize
 withNumberOfChannels:(UInt32)numberOfChannels 
{
    /** 
     Update the audio plot using the float array provided by the microphone:
       buffer[0] = left channel
       buffer[1] = right channel
     Note: Audio updates happen asynchronously so we need to make sure
         sure to update the plot on the main thread
     */
    __weak typeof (self) weakSelf = self;
    dispatch_async(dispatch_get_main_queue(), ^{
        [weakSelf.audioPlot updateBuffer:buffer[0] withBufferSize:bufferSize];
    });
}
```

###<a name="EZAudioPlotGL"></a>EZAudioPlotGL
Provides an audio waveform plot that uses OpenGL to perform the drawing. The API this class are exactly the same as those for the EZAudioPlot above. On iOS this is a subclass of the GLKView while on OSX this is a subclass of the NSOpenGLView. In most cases this is the plot you want to use, it's GPU-accelerated, can handle lots of points  while displaying 60 frames per second (the EZAudioPlot starts to choke on anything greater than 1024), and performs amazingly on all devices. The only downside is that you can only have one OpenGL plot onscreen at a time. However, you can combine OpenGL plots with Core Graphics plots in the view hierachy (see the EZAudioRecordExample for an example of how to do this).

*Relevant Example Projects*
- EZAudioOpenGLWaveformExample (iOS)
- EZAudioOpenGLWaveformExample (OSX)
- EZAudioPlayFileExample (iOS)
- EZAudioPlayFileExample (OSX)
- EZAudioRecordExample (iOS)
- EZAudioRecordExample (OSX)
- EZAudioPassThroughExample (iOS)
- EZAudioPassThroughExample (OSX)

####Creating An OpenGL Audio Plot

You can create an audio plot in the interface builder by dragging in a UIView on iOS or an NSView on OSX onto your content area. Then change the custom class of the UIView/NSView to `EZAudioPlotGL`.

![EZAudioPlotGLInterfaceBuilder](https://cloud.githubusercontent.com/assets/1275640/8532900/47d62346-23e6-11e5-8128-07c6641f4af8.gif)

Alternatively, you can could create the `EZAudioPlotGL` programmatically
```objectivec
// Programmatically create an audio plot
EZAudioPlotGL *audioPlotGL = [[EZAudioPlotGL alloc] initWithFrame:self.view.frame];
[self.view addSubview:audioPlotGL];
```

####Customizing The OpenGL Audio Plot

All plots offer the ability to change the background color, waveform color, plot type (buffer or rolling), toggle between filled and stroked, and toggle between mirrored and unmirrored (about the x-axis). For iOS colors are of the type UIColor while on OSX colors are of the type NSColor.
```objectivec
// Background color (use UIColor for iOS)
audioPlotGL.backgroundColor = [NSColor colorWithCalibratedRed:0.816 
                                                        green:0.349 
                                                         blue:0.255 
                                                        alpha:1];
// Waveform color (use UIColor for iOS)
audioPlotGL.color = [NSColor colorWithCalibratedRed:1.000 
                                              green:1.000 
                                               blue:1.000
                                              alpha:1];
// Plot type
audioPlotGL.plotType = EZPlotTypeBuffer;
// Fill
audioPlotGL.shouldFill = YES;
// Mirror
audioPlotGL.shouldMirror = YES;
```

####IBInspectable Attributes

Also, as of iOS 8 you can adjust the background color, color, gain, shouldFill, and shouldMirror parameters directly in the Interface Builder via the IBInspectable attributes:

![EZAudioPlotGLInspectableAttributes](https://cloud.githubusercontent.com/assets/1275640/8530670/288840c8-23d7-11e5-954b-644ed4ed67b4.png)

####Updating The OpenGL Audio Plot

All plots have only one update function, `updateBuffer:withBufferSize:`, which expects a float array and its length.
```objectivec
// The microphone component provides audio data to its delegate as an array of float buffer arrays.
- (void)   microphone:(EZMicrophone *)microphone
     hasAudioReceived:(float **)buffer
       withBufferSize:(UInt32)bufferSize
 withNumberOfChannels:(UInt32)numberOfChannels 
{
    /** 
     Update the audio plot using the float array provided by the microphone:
       buffer[0] = left channel
       buffer[1] = right channel
     Note: Audio updates happen asynchronously so we need to make sure
         sure to update the plot on the main thread
     */
    __weak typeof (self) weakSelf = self;
    dispatch_async(dispatch_get_main_queue(), ^{
        [weakSelf.audioPlotGL updateBuffer:buffer[0] withBufferSize:bufferSize];
    });
}
```

##License
EZAudio is available under the MIT license. See the LICENSE file for more info.

##Contact & Contributers
Syed Haris Ali<br>
www.syedharisali.com<br>
syedhali07[at]gmail.com

##Acknowledgements
EZAudio could not have been created without the invaluable help of:
- <a href="http://atastypixel.com/blog/">Michael Tyson</a> for creating the <a href="http://atastypixel.com/blog/a-simple-fast-circular-buffer-implementation-for-audio-processing/">TPCircularBuffer</a> and the <a href="http://theamazingaudioengine.com/">Amazing Audio Engine</a>'s `AEFloatConverter`.
- Chris Adamson and Kevin Avila for writing the amazing book <a href="http://www.amazon.com/Learning-Core-Audio-Hands-On-Programming/dp/0321636848">Learning Core Audio</a>
