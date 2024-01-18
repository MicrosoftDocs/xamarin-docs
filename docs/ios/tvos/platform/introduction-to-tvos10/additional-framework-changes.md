---
title: "Additional tvOS 10 Frameworks Changes"
description: "This document describes minor changes and enhancements made to existing frameworks in iOS 10. It discusses updates to AVFoundation, AVKit, Core Data, Core Graphics, Foundation, GameKit, GameplayKit, and more."
ms.service: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
no-loc: [Objective-C]
---

# Additional tvOS 10 Frameworks Changes

In addition to the major changes to tvOS, Apple has made modifications and improvements to several existing frameworks in tvOS 10.

<a name="AV-Foundation-Framework"></a>

## AVFoundation Framework Additions

The AVFoundation framework includes the following enhancements:

- In tvOS 10, the app no longer implements different [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) behaviors based on content type. Simply set the `Rate` property and AVFoundation will determine when enough content is available for playback without stalling.
- The new `AVPlayerLooper` class makes it easier to loop a given piece of media during playback.

<a name="AVKit-Framework-Enhancements"></a>

## AVKit Framework Enhancements

The AVKit framework includes the following enhancements:

- The app now has control over the skipping behavior of the [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) so a skipping gesture might move to the next item in the playlist or advance within the current item.

<a name="Core-Data-Enhancements"></a>

## Core Data Enhancements

tvOS 10 includes the following enhancements to the Core Data framework:

- Root [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objects supports concurrent faulting and fetching without serialization.
- The [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) class maintains a pool of SQLite data stores.
- The [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objects with SQLite data stores in the WAL Journal Mode support the new query generation feature where Managed Object Contexts (MOC) can be pinned to specific database versions for future fetching and faulting transactions.
- Using the high-level `NSPersistenceContainer` to reference the `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) and other Core Data configuration resources.
- Several new convenience methods have been added to `NSManagedObject` making it easier to perform fetches and create subclasses.

For more information, please see Apple's [Core Data Framework Reference](https://developer.apple.com/reference/coredata).

<a name="Core-Graphics-Enhancements"></a>

## Core Graphics Enhancements

tvOS 10 includes the following enhancements to the Core graphics framework:

- The new CGColorConverterRef class can be used to perform a series of color conversions.

<a name="Core-Image-Enhancements"></a>

## Core Image Enhancements

tvOS 10 makes the following enhancements to the Core Image framework:

- The `ImageWithExtent` method of the [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) class can be used to insert custom processing into the filter operation. Core Image will invoke the given callback between filters when processing an image for output or display.
- The app can now process images in a color space outside of the Core Image context's working color space by converting in and out of the color space before and after processing.
- Several rendering performance enhancements have been made to `UIImage` rendering (when backed by Core Image image stores) in `UIImageView` objects.
- `UIImage` objects tagged wide-gamut will render as wide-gamut color in `UIImageView` objects on iOS devices that support wide color.
- Core Image kernel code can now request specific pixel output formats.

Additionally, the following new Core Image filters have been added:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

<a name="Foundation-Enhancements"></a>

## Foundation Enhancements

The following enhancements have been made to the Foundation framework for tvOS 10:

- Use the new [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) class to make date and time interval calculations such as durations, for comparing intervals and testing for interval intersections.
- Several new properties have been added to the [NSLocal](https://developer.apple.com/reference/foundation/nslocale) class to acquire local information and the available display formats.
- Use the new [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) class to convert between different Units of Measure (UOM) or perform calculations on values in different UOMs.
- Use the new [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) class to format localized measurements for displaying to the end user.
- Use the new [NSUnit](https://developer.apple.com/reference/foundation/nsunit) and [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) classes for representing specific UOMs.

<a name="GameKit-Enhancements"></a>

## GameKit Enhancements

The following enhancements have been made to the GameKit framework in tvOS 10:

- A new iCloud-only account type has been implemented by the [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) class.
- The new [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) class provides a generalized solution for managing persistent data storage on Game Center. `GKGameSession` maintains a list of players and the app is responsible form implementing how and when participant date is stored, retrieved or exchanged between players. In many instances Game Sessions can replace existing turn-based matches, real-time matches or persistent game save methods.

<a name="GameplayKit-Enhancements"></a>

## GameplayKit Enhancements

The following enhancements have been made to the GameplayKit framework in tvOS 10:

- Procedural noise generation has been added and can be used to enhance the realism in natural-looking textures, add realism to camera movements and help generate rich game worlds.
- Use Spatial Partitioning to partition the game world data for efficient searching.
- A new Monte Carlo strategist ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) has been added for exhaustive possible move computation.
- A new Decision Tree API has been added ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) and [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) to enhance the game-building AI.
- 3D support has been added to existing agent and path-finding behaviors using the new [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) and [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) classes.
- Use the new [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) class to provide high-performance, natural-looking paths.
- The new [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) and [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) classes make combining GameplayKit and SpriteKit easier than ever.

<a name="Metal-Enhancements"></a>

## Metal Enhancements

The following enhancements have been made to the Metal framework in tvOS 10:

- 3D apps and games can now use _Tessellation_ to efficiently render complex scenes and geometry via the GPU.
- Use Function Specialization to create a highly-optimized collection of material and light combination functions for a scene.
- Provide fine-grained control of resource allocation to optimize performance of Metal based apps using Resource Heaps and Memoryless Render Targets.

To learn more, please see Apple's [Metal Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="Metal-Performance-Shaders-Enhancements"></a>

## Metal Performance Shaders Enhancements

The following enhancements have been made to the Metal Performance Shaders framework in tvOS 10:

- Many new kernels have been added to the Metal Performance Shaders framework to allow the app to take advantage of high-optimized, data-parallel computations such as color space conversions and neural network operations.

<a name="ModelIO-Enhancements"></a>

## ModelIO Enhancements

The following enhancements have been made to the ModelIO framework in tvOS 10:

- The USD file format is now supported.
- Use the new `MDLMaterialPropertyGraph` class to easily support runtime changes to models.
- Signed Distance Field support has been added to the [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) class.
- Use the new `MDLLightProbeIrradianceDataSource` class to assist in Light Probe placement.

<a name="SceneKit-Enhancements"></a>

## SceneKit Enhancements

The following enhancements have been made to the SceneKit framework in tvOS 10:

- SceneKit now includes a new Physically Based Rendering (PBR) system for more realistic results with simpler asset authoring.
- Use the new [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) shading model to product a wide range of realistic shading effects while requiring only three fundamental properties (`Diffuse`, `Metalness` and `Roughness`).
- Since PBR shading works best with environment-based lighting, use the `LightingEnvironment` property to assign image-based lighting to tan entire scene.
- Use the `IESProfileURL` property to import real-world light fixtures that define lighting base on real-world values such as intensity (in lumens) and color temperature (in degrees Kelvin).
- The [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) class can provide greater realism by using HDR features and effects. Use adaptive exposure to create automatic effects or use vignetting, color fringing and color grading to add filmatic effects to the game.
- Both PBR and HDR camera features provide better results than traditional rendering techniques and, as a result, SceneKit now performs all color calculations in a linear color space (using P3 color gamut on wide-color device displays).
- SceneKit now color matches all colors by reading the color profile information.
- SceneKit interprets color component values in a linear RGB color space for all shader types.
- Since SceneKit reads and adjust for color profile information in texture images, use Asset Catalogs for all images to ensure this information is provided.
- Both linear color space rendering and wide-color can be disabled by specifying the `SCNDisableLinearSpaceRendering` and `SCNDisableWideGamut` keys in the app's `Info.plist`.
- Build arbitrary polygon primates (either loaded from files or generated programmatically) to specify geometry with the new [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/documentation/scenekit/scngeometryprimitivetype/scngeometryprimitivetypepolygon) class.

<a name="SpriteKit-Enhancements"></a>

## SpriteKit Enhancements

The following enhancements have been made to the SpriteKit framework in tvOS 10:

- Tilemaps now support square, hexagonal and isometric tile shapes for 2D, 2.5D and side-scrolling games using the `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` and `SKTileSet` classes.
- Use the new `SKWarpGeometry` class to stretch or distort [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) or [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) rendering. The new [SKAction](https://developer.apple.com/reference/spritekit/skaction) class can be used to animate transitions between warp effects.
- Custom shaders can provide attributes (`SKAttribute`) that can be configured separately by each node that uses the shader by supplying an Attribute Value (`SKAttributeValue`).
- The [SKView](https://developer.apple.com/reference/spritekit/skview) class provides several new methods to give fine-grained control over when and how a scene is rendered.

<a name="UIKit-Enhancements"></a>

## UIKit Enhancements

The following enhancements have been made to the UIKit framework in tvOS 10:

- The focus API has been enhanced to support focus of non-view item in addition to `UIViews`. Items that support focus _must_ implement the `IUIFocusItem` interface.
- The new `UIGraphicsRender` class provides an object-oriented method of creating bitmaps or PDFs from UIKit rendering or Core Graphics and replaces the deprecated `UIGraphicsBeginImageContext` method.
- The `UIUserInterfaceStyle` class was added to determine which user interface theme (dark or light) is currently active.
- New fully interactive, object-based, interruptible animation support has been added and van be linked to gestures. Pleas see Apple's [UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator Class Reference](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters Class Reference](https://developer.apple.com/reference/uikit/uicubictimingparameters) and [UISpringTimingParameter Class Reference](https://developer.apple.com/reference/uikit/uispringtimingparameters) for more information.
- The new `UIPreviewInteraction` and `UIPreviewInteractionDelegate` allow the app to provide a custom interface for peek and pop operations.
- The new `UIAccessibilityCustomRotor` class allows the app to provide custom, context-specific functionality to assistive technologies such as Voice Over.
- Use the `UIAccessibilityIsAssistiveTouchRunning` and `UIAccessibilityAssistiveTouchStatusDidChangeNotification` symbols to determine if AssistiveTouch is enabled.
- Use the `UIAccessibilityHearingDevicePairedEar` and `UIAccessibilityHearingDevicePairedEarDidChangeNotification` symbols to get the status of any paired MFi hearing aids.
- The new [UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) API provides new options (such as lifetime limitations) and will automatically declare compatible content types for common class types.
- To support Dynamic Type in labels, text fields and text boxes use the new `PreferredFontForTextStyle` method of the `UIFont` class.
- To decide if an element should update it font when the devices `UIContentSizeCategory` changes, use the `AdjustsFontForContentSizeCategory` property of the `UIContentSizeCategoryAdjusting` delegate.
- The app can now control the appearance of the badge for tab bar items such as text and background color.
- The Refresh Control in now supported in all scroll view and scroll view subclasses (such as `UICollectionView`).
- The `OpenURL` method of the `UIApplication` class is called asynchronously now supports a Completion Handler that is called after the open has completed.
- Initiate CloudKit sharing and modify its properties using the new `UICloudSharingController` and `UICloudSharingControllerDelegate` classes.
- Take advantage of prefetched cells to improve the scrolling experience of `UICollectionViews` with the new `UICollectionViewDataSourcePrefetching` delegate.

## Related Links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [What's new in tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)