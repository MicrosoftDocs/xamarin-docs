---
title: "Core ML 2 in Xamarin.iOS"
description: "This document describes updates to Core ML available as part of iOS 12. In particular, it looks at performance improvements associated with the new batch prediction API."
ms.service: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/15/2018
no-loc: [Objective-C]
---
# Core ML 2 in Xamarin.iOS

Core ML is a machine learning technology available on iOS, macOS, tvOS,
and watchOS. It allows apps to make predictions based on machine learning
models.

In iOS 12, Core ML includes a batch processing API. This API makes Core
ML more efficient and provides performance improvements in scenarios where
a model is used to make a sequence of predictions.

## Sample app: MarsHabitatCoreMLTimer

To demonstrate batch predictions with Core ML, take a look at the
[MarsHabitatCoreMLTimer](/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer)
sample app. This sample uses a Core ML model trained to predict the cost
of building a habitat on Mars, based on various inputs: number of solar
panels, number of greenhouses, and number of acres.

The code snippets in this document come from this sample.

## Generate sample data

In `ViewController`, the sample app's `ViewDidLoad` method calls
`LoadMLModel`, which loads the included Core ML model:

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

Then, the sample app creates 100,000 `MarsHabitatPricerInput` objects to
use as input for sequential Core ML predictions. Each generated sample
has a random value set for the number of solar panels, the number of
greenhouses, and the number of acres:

```csharp
async void CreateInputs(int num)
{
    // ...
    Random r = new Random();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            double solarPanels = r.NextDouble() * MaxSolarPanels;
            double greenHouses = r.NextDouble() * MaxGreenHouses;
            double acres = r.NextDouble() * MaxAcres;
            inputs[i] = new MarsHabitatPricerInput(solarPanels, greenHouses, acres);
        }
    });
    // ...
}
```

Tapping any of the app's three buttons executes two sequences of
predictions: one using a `for` loop, and another using the new batch
`GetPredictions` method introduced in iOS 12:

```csharp
async void RunTest(int num)
{
    // ...
    await FetchNonBatchResults(num);
    // ...
    await FetchBatchResults(num);
    // ...
}
```

## for loop

The `for` loop version of the test naively iterates over the specified
number of inputs, calling [`GetPrediction`](xref:CoreML.MLModel.GetPrediction*)
for each and discarding the result. The method times how long it takes to
make the predictions:

```csharp
async Task FetchNonBatchResults(int num)
{
    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            IMLFeatureProvider output = model.GetPrediction(inputs[i], out NSError error);
        }
    });
    stopWatch.Stop();
    nonBatchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## GetPredictions (new batch API)

The batch version of the test creates an `MLArrayBatchProvider` object
from the input array (since this is a required input parameter for the
`GetPredictions` method), creates an
[`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
object that prevents prediction computations from being restricted to the
CPU, and uses the `GetPredictions` API to fetch the predictions, again
discarding the result:

```csharp
async Task FetchBatchResults(int num)
{
    var batch = new MLArrayBatchProvider(inputs.Take(num).ToArray());
    var options = new MLPredictionOptions()
    {
        UsesCpuOnly = false
    };

    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        model.GetPredictions(batch, options, out NSError error);
    });
    stopWatch.Stop();
    batchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## Results

On both simulator and device, `GetPredictions` finishes more quickly than
the loop-based Core ML predictions.

## Related links

- [Sample app – MarsHabitatCoreMLTimer](/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer)
- [What's new in Core ML, Part 1 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/708/)
- [What's new in Core ML, Part 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Introduction to Core ML in Xamarin.iOS](../introduction-to-ios11/coreml.md)
- [Core ML (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [Working with Core ML Models](https://developer.apple.com/machine-learning/build-run-models/)