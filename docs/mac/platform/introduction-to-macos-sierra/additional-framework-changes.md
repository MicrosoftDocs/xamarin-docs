---
title: "Additional macOS Sierra Framework Changes"
description: "This document describes minor changes and enhancements to existing frameworks introduced in macOS Sierra. It examines changes to the Accelerate framework, AppKit, AVFoundation, Core Data, Core Image, Foundation, and more."
ms.service: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.subservice: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
no-loc: [Objective-C]
---

# Additional macOS Sierra Framework Changes

<a name="Accelerate-Framework-Enhancements"></a>

## Accelerate Framework Enhancements

The following enhancement have been made to the Accelerate Framework for macOS Sierra:

- Added Quadrature (integral calculus).
- Added Basic functions for constructing neural networks.
- Added Geometric predicate functions to test for things like the intersection of two geometric objects.

<a name="AppKit-Framework-Enhancements"></a>

## AppKit Framework Enhancements

The following enhancement have been made to the AppKit Framework for macOS Sierra:

- Several enhancements to `NSCollectionView` such as:
  - **Collapsible Sections** - Allows the user to collapse a Collection View section into a single horizontal row.
  - **Floating Headers** - Headers and Footers can now be floated (in a flow layout) using the same API as [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) in iOS.
  - **Scrollable Background Views** - A collection Views background can now be set to scroll along with the content.
- The deferred view layout pass has been optimized and extended.
- The drag-and-drop API now includes the new `NSFilePromiseProvider` and `NSFilePromiseReceiver` classes to support drag flocking.
- Several convenience constructors have been added to existing controls:
  - `NSButton` includes new constructors for creating push buttons, checkboxes and radio buttons.
  - `NSTextField` includes new constructors for creating wrapping and non-wrapping labels, attributed labels and editable text fields.
  - `NSSegmentedControl` includes new constructors for creating segmented controls from a group of labels or images.
  - `NSSlider` includes new constructors for creating horizontal linear sliders.
  - `NSImageView` includes new constructors for creating non-editable image views from a given `NSImage`.
- The new `NSGridView` has been added to auto layout a collection of sub views into a grid with variable sized rows and columns that can be dynamically hidden or shown.

<a name="AVFoundation-Framework-Enhancements"></a>

## AVFoundation Framework Enhancements

The following enhancement have been made to the AVFoundation Framework for macOS Sierra:

- In macOS, the app no longer has to implement different [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) behaviors based on content type. Simply set the `Rate` property and AVFoundation will determine when enough content is available for playback without stalling.
- The new `AVPlayerLooper` class makes it easier to loop a given piece of media during playback.
- The `AVAssetDownloadURLSession` class allows for the downloading and later playback of FairPlay encrypted HLS streams.

<a name="Core-Data-Framework-Enhancements"></a>

## Core Data Framework Enhancements

The following enhancement have been made to the Core Data Framework for macOS Sierra:

- Root [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objects supports concurrent faulting and fetching without serialization.
- The [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) class maintains a pool of SQLite data stores.
- The [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objects with SQLite data stores in the WAL Journal Mode support the new query generation feature where Managed Object Contexts (MOC) can be pinned to specific database versions for future fetching and faulting transactions.
- Using the high-level `NSPersistenceContainer` to reference the `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) and other Core Data configuration resources.
- Several new convenience methods have been added to `NSManagedObject` making it easier to perform fetches and create subclasses.

For more information, please see Apple's [Core Data Framework Reference](https://developer.apple.com/reference/coredata).

<a name="Core-Image-Framework-Enhancements"></a>

## Core Image Framework Enhancements

The following enhancement have been made to the Core Image Framework for macOS Sierra:

- The `ImageWithExtent` method of the [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) class can be used to insert custom processing into the filter operation. Core Image will invoke the given callback between filters when processing an image for output or display.
- The app can now process images in a color space outside of the Core Image context's working color space by converting in and out of the color space before and after processing.
- The Core Image kernel can now request a specific pixel output format.
- The following new image filters have been added: `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` and `CIClamp`.

<a name="Foundation-Framework-Enhancements"></a>

## Foundation Framework Enhancements

The following enhancement have been made to the Foundation Framework for macOS Sierra:

- Use the [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) API for representing, converting and displaying many of the most common physical units such as mass, length, speed, duration and temperature.
- Use the [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) class for parsing and generating ISO 8601 formatted dates.
- Use the new [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) class to make date and time interval calculations such as durations, for comparing intervals and testing for interval intersections.
- Use the [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) class to parse the elements of a person's name from a string.
- Use the new [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) class to obtain metrics for a URL networking session.

For more information, please see Apple's [Foundation Release Notes for OS X v10.12 and iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements"></a>

## GameKit Framework Enhancements

The following enhancement have been made to the GameKit Framework for macOS Sierra:

- The **Game Center App** has been deprecated and removed from macOS. If the app uses GameKit, it _must_ present its own interface to display GameKit features such as leaderboards, etc.
- A new iCloud-only account type has been implemented by the [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) class.
- The new [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) class provides a generalized solution for managing persistent data storage on Game Center. `GKGameSession` maintains a list of players and the app is responsible form implementing how and when participant date is stored, retrieved or exchanged between players. In many instances Game Sessions can replace existing turn-based matches, real-time matches or persistent game save methods.

<a name="GamePlayKit-Framework-Enhancements"></a>

## GamePlayKit Framework Enhancements

The following enhancement have been made to the GamePlayKit Framework for macOS Sierra:

- Procedural noise generation has been added and can be used to enhance the realism in natural-looking textures, add realism to camera movements and help generate rich game worlds.
- Use Spatial Partitioning to partition the game world data for efficient searching.
- A new Monte Carlo strategist ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) has been added for exhaustive possible move computation.
- A new Decision Tree API has been added ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) and [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) to enhance the game-building AI.
- 3D support has been added to existing agent and path-finding behaviors using the new [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) and [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) classes.
- Use the new [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) class to provide high-performance, natural-looking paths.
- The new [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) and [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) classes make combining GameplayKit and SpriteKit easier than ever.

<a name="Metal-Framework-Enhancements"></a>

## Metal Framework Enhancements

The following enhancement have been made to the Metal Framework for macOS Sierra:

- 3D apps and games can now use _Tessellation_ to efficiently render complex scenes and geometry via the GPU.
- Use Function Specialization to create a highly-optimized collection of material and light combination functions for a scene.
- Provide fine-grained control of resource allocation to optimize performance of Metal based apps using Resource Heaps and Memoryless Render Targets.

To learn more, please see Apple's [Metal Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="ModelIO-Framework-Enhancements"></a>

## Model I/O Framework Enhancements

The following enhancement have been made to the Model I/O Framework for macOS Sierra:

- The USD file format is now supported.
- Use the new `MDLMaterialPropertyGraph` class to easily support runtime changes to models.
- Signed Distance Field support has been added to the [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) class.
- Use the new `MDLLightProbeIrradianceDataSource` class to assist in Light Probe placement.

<a name="Photos-Framework-Enhancements"></a>

## Photos Framework Enhancements

The following enhancement have been made to the Photos Framework for macOS Sierra:

- Live Photo editing is now available for apps that support the Photos framework and to photo editing extensions (for use inside of the Photos and Camera apps).
- Use the new [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) class to apply edits to both the video and still content of Live Photos.
- Use the [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) and [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) classes to take advantage of the new Core Image processor feature to perform edits.
- To support Live Photos, the [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) and [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) classes have been ported from iOS to macOS.

<a name="SceneKit-Framework-Enhancements"></a>

## SceneKit Framework Enhancements

The following enhancement have been made to the SceneKit Framework for macOS Sierra:

- Now includes a new Physically Based Rendering (PBR) system for more realistic results with simpler asset authoring.
- Use the new [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) shading model to product a wide range of realistic shading effects while requiring only three fundamental properties (`Diffuse`, `Metalness` and `Roughness`).
- Since PBR shading works best with environment-based lighting, use the `LightingEnvironment` property to assign image-based lighting to tan entire scene.
- Use the `IESProfileURL` property to import real-world light fixtures that define lighting base on real-world values such as intensity (in lumens) and color temperature (in degrees Kelvin).
- The [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) class can provide greater realism by using HDR features and effects. Use adaptive exposure to create automatic effects or use vignetting, color fringing and color grading to add filmatic effects to the game.
- Both PBR and HDR camera features provide better results than traditional rendering techniques and, as a result, SceneKit now performs all color calculations in a linear color space (using P3 color gamut on wide-color device displays).
- SceneKit now color matches all colors by reading the color profile information.
- SceneKit interprets color component values in a linear RGB color space for all shader types.
- Since SceneKit reads and adjust for color profile information in texture images, use Asset Catalogs for all images to ensure this information is provided.
- Both linear color space rendering and wide-color can be disabled by specifying the `SCNDisableLinearSpaceRendering` and `SCNDisableWideGamut` keys in the app's `Info.plist`.
- Build arbitrary polygon primates (either loaded from files or generated programmatically) to specify geometry with the new [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/documentation/scenekit/scngeometryprimitivetype/scngeometryprimitivetypepolygon?language=objc) class.

<a name="Security-Framework-Enhancements"></a>

## Security Framework Enhancements

The following enhancement have been made to the Security Framework for macOS Sierra:

- The `SecKey` interface has been modernized and unified across all platforms (iOS, tvOS, watchOS and macOS).

<a name="SpriteKit-Framework-Enhancements"></a>

## SpriteKit Framework Enhancements

The following enhancement have been made to the SpriteKit Framework for macOS Sierra:

- Tilemaps now support square, hexagonal and isometric tile shapes for 2D, 2.5D and side-scrolling games using the `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` and `SKTileSet` classes.
- Use the new `SKWarpGeometry` class to stretch or distort [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) or [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) rendering. The new [SKAction](https://developer.apple.com/reference/spritekit/skaction) class can be used to animate transitions between warp effects.
- Custom shaders can provide attributes (`SKAttribute`) that can be configured separately by each node that uses the shader by supplying an Attribute Value (`SKAttributeValue`).
- The [SKView](https://developer.apple.com/reference/spritekit/skview) class provides several new methods to give fine-grained control over when and how a scene is rendered.

<a name="New-Frameworks"></a>

## New Frameworks

The following frameworks have been added to macOS Sierra:

- **Intents Framework** - This framework allow the app to examine interactions (such as location or user actions), and take action based on that information.
- **SafariServices Framework** - This framework allow the app to develop app extensions for Safari (such as content blockers) for both macOS and iOS.

## Related Links

- [Mac Samples](/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [What's new in OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)