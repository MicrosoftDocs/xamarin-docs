---
title: "Additional iOS 10 Frameworks Changes"
description: "This document describes minor changes and enhancements made to existing frameworks in iOS 10 and discusses how to make use of these updates in Xamarin.iOS."
ms.service: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
no-loc: [Objective-C]
---

# Additional iOS 10 Frameworks Changes

_This article covers additional, minor changes or enhancements to existing frameworks for iOS 10._

## AV Foundation Framework Additions

The AVFoundation framework includes the following enhancements:

- In iOS 10, the developer no longer has to implement different [AVPlayerItem](xref:AVFoundation.AVPlayerItem) behaviors based on content type. Simply set the `Rate` property and AVFoundation will determine when enough content is available for playback without stalling.
- The new [AVCapturePhotoOutput](xref:AVFoundation.AVCaptureFileOutput) class replaces the  deprecated `AVCaptureStillImageOutput` class and provides a unified method for handling all photography workflows by providing sophisticated control and monitoring of the capture process and support for new features such as Live Photos and the RAW capture format.
- The new `AVPlayerLooper` class makes it easier to loop a given piece of media during playback.
- The `AVAssetDownloadURLSession` class allows for the downloading and later playback of FairPlay encrypted HLS streams.
- By default, the [AVCaptureSession](xref:AVFoundation.AVCaptureSession) class automatically supports wide-color, wide-gamut capture when the device hardware supports it. See Apple's [iOS Device Compatibility Reference](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) for more details.

## AVKit Additions

The AVKit framework now includes the new `UpdatesNowPlayingInfoCenter` property to indicate when the Now Playing Info Center should be updated.

## Core Data Enhancements

iOS 10 includes the following enhancements to the Core Data framework:

- The [NSManagedObjectContext](xref:CoreData.NSManagedObjectContext) objects with SQLite data stores in the WAL Journal Mode support the new query generation feature where Managed Object Contexts (MOC) can be pinned to specific database versions for future fetching and faulting transactions.
- Root [NSManagedObjectContext](xref:CoreData.NSManagedObjectContext) objects supports concurrent faulting and fetching without serialization.
- The [NSPersistentStoreCoordinator](xref:CoreData.NSPersistentStoreCoordinator) class maintains a pool of SQLite data stores.
- Several new convenience methods have been added to `NSManagedObject` making it easier to perform fetches and create subclasses.
- Using the high-level `NSPersistenceContainer` to reference the `NSPersistentStoreCoordinator`, [NSManagedObjectModel](xref:CoreData.NSManagedObjectModel) and other Core Data configuration resources.

For more information, please see Apple's [Core Data Framework Reference](https://developer.apple.com/reference/coredata).

## Core Image Enhancements

iOS 10 makes the following enhancements to the Core Image framework:

- The developer can now process images in a color space outside of the Core Image context's working color space by converting in and out of the color space before and after processing.
- For iOS devices that use the A8 or A9 CPUs, the RAW image format is now supported. Core Image now provides support for decoding RAW images from either the built-in iSight camera or from a 3rd party camera. Use the `FilterWithImageData` or `FilterWithImageURL` methods of the [CIFilter](xref:CoreImage.CIFilter) class to process RAW images.
- Several rendering performance enhancements have been made to `UIImage` rendering (when backed by Core Image image stores) in `UIImageView` objects.
- `UIImage` objects tagged wide-gamut will render as wide-gamut color in `UIImageView` objects on iOS devices that support wide color.
- Core Image kernel code can now request specific pixel output formats.
- The `ImageWithExtent` method of the [CIFilter](xref:CoreImage.CIFilter) class can be used to insert custom processing into the filter operation. Core Image will invoke the given callback between filters when processing an image for output or display.

Additionally, the following new Core Image filters have been added:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## Core Motion Additions

New to iOS 10, the Core Motion framework includes pedometer events which enable an app to receive fast, real-time notifications of the user pausing and resuming tracking while running. Use the [CMPedometer](xref:CoreMotion.CMPedometer) to register for foreground or background pedometer events.

## Foundation Enhancements

The following enhancements have been made to the Foundation framework for iOS 10:

- Use the new [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) class to format localized measurements for displaying to the end user.
- Use the new [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) class to make date and time interval calculations such as durations, for comparing intervals and testing for interval intersections.
- Use the new [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) class to convert between different Units of Measure (UOM) or perform calculations on values in different UOMs.

- Use the new [NSUnit](https://developer.apple.com/reference/foundation/nsunit) and [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) classes for representing specific UOMs.
- Several new properties have been added to the [NSLocal](https://developer.apple.com/reference/foundation/nslocale) class to acquire local information and the available display formats.

## GameKit Enhancements

The following enhancements have been made to the GameKit framework in iOS 10:

- The **Game Center App** has been deprecated and removed from iOS. If the app uses GameKit, it _must_ present its own interface to display GameKit features such as leaderboards, etc.
- A new iCloud-only account type has been implemented by the [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) class.
- The new [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) class provides a generalized solution for managing persistent data storage on Game Center. `GKGameSession` maintains a list of players and the app is responsible for implementing how and when participant date is stored, retrieved or exchanged between players. In many instances Game Sessions can replace existing turn-based matches, real-time matches or persistent game save methods.

## GameplayKit Enhancements

The following enhancements have been made to the GameplayKit framework in iOS 10:

- Use the new [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) class to provide high-performance, natural-looking paths.
- Procedural noise generation has been added and can be used to enhance the realism in natural-looking textures, add realism to camera movements and help generate rich game worlds.
- Use Spatial Partitioning to partition the game world data for efficient searching.
- A new Monte Carlo strategist ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) has been added for exhaustive possible move computation.
- 3D support has been added to existing agent and path-finding behaviors using the new [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) and [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) classes.
- The new [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) and [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) classes make combining GameplayKit and SpriteKit easier than ever.
- A new Decision Tree API has been added ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) and [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) to enhance the game-building AI.

## HealthKit Enhancements

The following enhancements have been made to the HealthKit framework in iOS 10:

- New metadata keys have been added for weather types (such as `HKWeatherConditionClear` and `HKWeatherConditionCloudy`) and workout types (such as `HKWorkoutActivityTypeFlexibility` and `HKWorkoutActivityTypeWheelchairRunPace`) have been added.
- The new `HKCDADocument` class has been added to represent a Clinical Document Architecture (CDA) formatted document.
- Use the new [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) class to specify the `ActivityType` and `LocationType` of a workout.
- The new [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) and the `WheelchairUse` method of the [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) class have been added for working with wheelchair related health data.

## HomeKit Enhancements

The following enhancements have been made to the HomeKit framework in iOS 10:

- New services and characteristics have been added.
- An iPad can be configured to act as a HomeKit Hub to provide remote accessory access, run automation triggers and enable shared user permissions.
- Support has been added for camera and doorbell accessories.
- More context and configurations have been provided for accessories.

Please see our [Introduction to HomeKit](~/ios/platform/homekit.md) documentation for more information.

## Metal Enhancements

The following enhancements have been made to the Metal framework in iOS 10:

- 3D apps and games can now use _Tessellation_ to efficiently render complex scenes and geometry via the GPU.
- Provide fine-grained control of resource allocation to optimize performance of Metal based apps using Resource Heaps and Memoryless Render Targets.
- Use Function Specialization to create a highly-optimized collection of material and light combination functions for a scene.

To learn more, please see Apple's [Metal Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

## ModelIO Enhancements

The following enhancements have been made to the ModelIO framework in iOS 10:

- The USD file format is now supported.
- Signed Distance Field support has been added to the [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) class.
- Use the new `MDLLightProbeIrradianceDataSource` class to assist in Light Probe placement.
- Use the new `MDLMaterialPropertyGraph` class to easily support runtime changes to models.

## Photos Enhancements

The following enhancements have been made to the Photos framework in iOS 10:

- Use the [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) and [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) classes to take advantage of the new Core Image processor feature to perform edits.
- Live Photo editing is now available for apps that support the Photos framework and to photo editing extensions (for use inside of the Photos and Camera apps).
- Use the new [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) class to apply edits to both the video and still content of Live Photos.

## ReplayKit Enhancements

The following enhancements have been made to the ReplayKit framework in iOS 10:

- Use the [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) and [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) classes to support broadcasting of recorded media through 3rd party sites.
- The Broadcast UI and Broadcast Upload extensions are required to support ReplayKit 3rd party broadcast services in the app.

## SceneKit Enhancements

The following enhancements have been made to the SceneKit framework in iOS 10:

- The [SCNCamera](xref:SceneKit.SCNCamera) class can provide greater realism by using HDR features and effects. Use adaptive exposure to create automatic effects or use vignetting, color fringing and color grading to add fillmatic effects to the game.
- SceneKit now includes a new Physically Based Rendering (PBR) system for more realistic results with simpler asset authoring.
- Use the new [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) shading model to product a wide range of realistic shading effects while requiring only three fundamental properties (`Diffuse`, `Metalness` and `Roughness`).
- Since PBR shading works best with environment-based lighting, use the `LightingEnvironment` property to assign image-based lighting to an entire scene.
- Use the `IESProfileURL` property to import real-world light fixtures that define lighting based on real-world values such as intensity (in lumens) and color temperature (in degrees Kelvin).
- Both PBR and HDR camera features provide better results than traditional rendering techniques and, as a result, SceneKit now performs all color calculations in a linear color space (using P3 color gamut on wide-color device displays).
- SceneKit now color matches all colors by reading the color profile information.
- SceneKit interprets color component values in a linear RGB color space for all shader types.
- Both linear color space rendering and wide-color can be disabled by specifying the `SCNDisableLinearSpaceRendering` and `SCNDisableWideGamut` keys in the app's `Info.plist`.
- Build arbitrary polygon primates (either loaded from files or generated programmatically) to specify geometry with the new [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/documentation/scenekit/scngeometryprimitivetype/scngeometryprimitivetypepolygon) class.
- Since SceneKit reads and adjust for color profile information in texture images, use Asset Catalogs for all images to ensure this information is provided.

## SpriteKit Enhancements

The following enhancements have been made to the SpriteKit framework in iOS 10:

- Custom shaders can provide attributes (`SKAttribute`) that can be configured separately by each node that uses the shader by supplying an Attribute Value (`SKAttributeValue`).
- Tilemaps now support square, hexagonal and isometric tile shapes for 2D, 2.5D and side-scrolling games using the `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` and `SKTileSet` classes.
- Use the new `SKWarpGeometry` class to stretch or distort [SKSpriteNode](xref:SpriteKit.SKSpriteNode) or [SKEffectNode](xref:SpriteKit.SKEffectNode) rendering. The new [SKAction](xref:SpriteKit.SKAction) class can be used to animate transitions between warp effects.
- The [SKView](xref:SpriteKit.SKView) class provides several new methods to give fine-grained control over when and how a scene is rendered.

## ScrollView Enhancements

The following enhancements have been made to the ScrollView control in iOS 10.3:

- `UIScrollView` now include the `IndexDisplayMode` property to control how the index is shown while the user is scrolling as a `UIScrollViewIndexDisplayMode` of:
  - `Automatic` - The index display is controlled by the OS.
  - `AlwaysHidden` - The index display is always hidden.

See the [iOSTenThree Sample](/samples/xamarin/ios-samples/ios10-iostenthree) for usage.

## UIKit Enhancements

The following enhancements have been made to the UIKit framework in iOS 10:

- The new [UIPasteboard](xref:UIKit.UIPasteboard) API provides new options (such as lifetime limitations) and will automatically declare compatible content types for common class types.
- New fully interactive, object-based, interruptible animation support has been added and can be linked to gestures. Please see Apple's [UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator Class Reference](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters Class Reference](https://developer.apple.com/reference/uikit/uicubictimingparameters) and [UISpringTimingParameter Class Reference](https://developer.apple.com/reference/uikit/uispringtimingparameters) for more information.
- The new `UIPreviewInteraction` and `UIPreviewInteractionDelegate` allow the developer app to provide a custom interface for peek and pop operations.
- The new `UIAccessibilityCustomRotor` class allows the app to provide custom, context-specific functionality to assistive technologies such as Voice Over.
- Use the `UIAccessibilityIsAssistiveTouchRunning` and `UIAccessibilityAssistiveTouchStatusDidChangeNotification` symbols to determine if AssistiveTouch is enabled.
- Use the `UIAccessibilityHearingDevicePairedEar` and `UIAccessibilityHearingDevicePairedEarDidChangeNotification` symbols to get the status of any paired MFi hearing aids.
- To support Dynamic Type in labels, text fields and text boxes use the new `PreferredFontForTextStyle` method of the `UIFont` class.
- To decide if an element should update its font when the device's `UIContentSizeCategory` changes, use the `AdjustsFontForContentSizeCategory` property of the `UIContentSizeCategoryAdjusting` delegate.
- The `OpenURL` method of the `UIApplication` class is called asynchronously and now supports a Completion Handler that is called after the open action has completed.
- Initiate CloudKit sharing and modify its properties using the new `UICloudSharingController` and `UICloudSharingControllerDelegate` classes.
- Take advantage of prefetched cells to improve the scrolling experience of `UICollectionViews` with the new `UICollectionViewDataSourcePrefetching` delegate.
- The developer can now control the appearance of the badge for tab bar items (such as text and background color).
- The Refresh Control is now supported in all scroll view and scroll view subclasses (such as `UICollectionView`).

## WebKit Enhancements

The following enhancements have been made to the WebKit framework in iOS 10:

- Peek and pop support has been added to the `WKWebView` class. Use the `ShouldPreviewElement` method to determine if a given web view should display a preview.

## Related Links

- [iOS 10 Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2biOS10)
- [What's new in iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)