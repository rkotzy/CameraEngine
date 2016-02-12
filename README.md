![cameraenginelogo](https://cloud.githubusercontent.com/assets/3276768/12975345/f3041ce8-d10e-11e5-99c4-2db99b5c9cd0.png)

<h3 align="center">🌟 The most advanced Camera framework in <strong>Swift</strong> 🌟</h1>

[![License MIT](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://raw.githubusercontent.com/ibireme/YYAsyncLayer/master/LICENSE)&nbsp;
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)&nbsp;
[![CocoaPods](http://img.shields.io/cocoapods/v/CameraEngine.svg?style=flat)](http://cocoapods.org/?q=YYAsyncLayer)&nbsp;
[![CocoaPods](http://img.shields.io/cocoapods/p/CameraEngine.svg?style=flat)](http://cocoapods.org/?q=YYAsyncLayer)&nbsp;
[![Support](https://img.shields.io/badge/support-iOS%206%2B%20-blue.svg?style=flat)](https://www.apple.com/nl/ios/)&nbsp;

**CameraEngine** *is an iOS camera engine library that allows easy integration of special capture features and camera customization in your iOS app.*

######Because users love to take pictures on the iPhone, billions of applications provide features related to the iPhone camera. For us iOS developers was available two methods to implement these features. The first is to use UIImagePickerController very limited interface side, and no manipulation hardware side. AVFoundation and offers plenty of opportunity for developers, this "low level" framework is hard to use.

*That's why Camera Engine was born, it proposes to have much flexibility, and offers users advanced features.*

## :fire: Features


              |  CameraEngine
--------------------------|------------------------------------------------------------
:camera: | Photo capture
:movie_camera: | Video capture
:chart_with_upwards_trend: | quality settings presset video / photo capture
:raising_hand: | sitch device (front, back) #selfiepower 
:bulb: | flash mode management
:flashlight: | torch mode management
:mag_right: | focus mode management
:bowtie: | detection face, barecode, and qrcode
:rocket: | GIF encoder

## 🔨 Installation

#### CocoaPods

- Add `pod "CameraEngine"` to your Podfile.
- Run `pod install` or `pod update`.
- import CameraEngine


#### Carthage

- Add `github "remirobert/CameraEngine"` to your Cartfile.
- Run `carthage update --platform ios` and add the framework to your project.
- import CameraEngine


#### Manually

- Download all the files in the CameraEngine subdirectory.
- Add the source files to your Xcode project.
- import CameraEngine


## :rocket: Quick start

> First let's init the and start the camera session. You can call that in the viewDidLoad, or in the appDelegate.

```Swift
override func viewDidLoad() {
  super.viewDidLoad()
  self.cameraEngine.startSession()
}
```
> Next time to display the preview layer

```Swift
override func viewDidLayoutSubviews() {
  let layer = self.cameraEngine.previewLayer
        
  layer.frame = self.view.bounds
  self.view.layer.insertSublayer(layer, atIndex: 0)
  self.view.layer.masksToBounds = true
}
```

> Capture a photo

```Swift
self.cameraEngine.capturePhoto { (image: UIImage?, error: NSError?) -> (Void) in
  //get the picture tooked in the 👉 image
}
```

> Capture a video

```Swift
private func startRecording() {
  guard let url = CameraEngineFileManager.documentPath("video.mp4") else {
    return
  }
            
  self.cameraEngine.startRecordingVideo(url, blockCompletion: { (url, error) -> (Void) in
  })
}

private func stopRecording() {
  self.cameraEngine.stopRecordingVideo()
}
```

> Generate animated image GIF

```swift
guard let url = CameraEngineFileManager.documentPath("animated.gif") else {
  return
}
self.cameraEngine.createGif(url, frames: self.frames, delayTime: 0.1, completionGif: { (success, url) -> (Void) in
  //Do some crazy stuff here
})
```

## :wrench: configurations

Camera Engine, allows you to set some parameters, such as management of flash, torch and focus. But also on the quality of the media, which also has an impact on the size of the output file. 

> Flash

```swift
self.cameraEngine.flashMode = .On
self.cameraEngine.flashMode = .Off
self.cameraEngine.flashMode = .Auto
```

> Torch

```swift
self.cameraEngine.torchMode = .On
self.cameraEngine.torchMode = .Off
self.cameraEngine.torchMode = .Auto
```

> Focus

              |  CameraEngine focus
--------------------------|------------------------------------------------------------
.Locked | means the lens is at a fixed position
.AutoFocus | means setting this will cause the camera to focus once automatically, and then return back to Locked
.ContinuousAutoFocus | means the camera will automatically refocus on the center of the frame when the scene changes

```swift
self.cameraEngine.cameraFocus = .Locked
self.cameraEngine.cameraFocus = .AutoFocus
self.cameraEngine.cameraFocus = .ContinuousAutoFocus
```

> Camera presset Photo

```swift
self.cameraEngine.sessionPresset = .Low
self.cameraEngine.sessionPresset = .Medium
self.cameraEngine.sessionPresset = .High
...
```

> Camera presset Video

```swift
self.cameraEngine.videoEncoderPresset = .Preset640x480
self.cameraEngine.videoEncoderPresset = .Preset960x540
self.cameraEngine.videoEncoderPresset = .Preset1280x720
self.cameraEngine.videoEncoderPresset = .Preset1920x1080
self.cameraEngine.videoEncoderPresset = .Preset3840x2160
```

## :eyes: Object detection

CameraEngine can detect faces, qrcodes, or barcode. It will return all metadata on each frame, when it detects something. To exploit you whenever you want later.

> Set the detection mode

```swift
self.cameraEngine.metadataDetection = .Face
self.cameraEngine.metadataDetection = .QRCode
self.cameraEngine.metadataDetection = .BareCode
self.cameraEngine.metadataDetection = .None //disable the detection
```
> exploiting face detection

```swift
self.cameraEngine.blockCompletionFaceDetection = { faceObject in
  let frameFace = (faceObject as AVMetadataObject).bounds
  self.displayLayerDetection(frame: frameFace)
}
```

> exploiting code detection (barecode and QRCode)

```swift
self.cameraEngine.blockCompletionCodeDetection = { codeObject in
  let valueCode = codeObject.stringValue
  let frameCode = (codeObject as AVMetadataObject).bounds
  self.displayLayerDetection(frame: frameCode)
}
```

## :car::dash: Example

You will find a sample project, which implements all the features of CameraEngine, with an interface that allows you to test and play with the settings.
To run the example projet, run `cocoapod install`, because it uses the current prod version of CameraEngine.

## License
This project is licensed under the terms of the MIT license. See the LICENSE file.

> This project is in no way affiliated with Apple Inc. This project is open source under the MIT license, which means you have full access to the source code and can modify it to fit your own needs.
