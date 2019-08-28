---
title: "Xamarin.Essentials Unit Converters"
description: "The UnitConverters class in Xamarin.Essentials provides several unit converters to help developers when using Xamarin.Essentials."
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
---

# Xamarin.Essentials: Unit Converters

The **UnitConverters** class provides several unit converters to help developers when using Xamarin.Essentials.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using Unit Converters

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

All unit converters are available by using the static `UnitConverters` class in Xamarin.Essentials. For instance you can easily convert Fahrenheit to Celsius.

```csharp
var celcius = UnitConverters.FahrenheitToCelsius(32.0);
```

Here is a list of available conversions:

- FahrenheitToCelsius
- CelsiusToFahrenheit
- CelsiusToKelvin
- KelvinToCelsius
- MilesToMeters
- MilesToKilometers
- KilometersToMiles
- DegreesToRadians
- RadiansToDegrees
- DegreesPerSecondToRadiansPerSecond
- RadiansPerSecondToDegreesPerSecond
- DegreesPerSecondToHertz
- RadiansPerSecondToHertz
- HertzToDegreesPerSecond
- HertzToRadiansPerSecond
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- KilopascalsToPascals
- HectopascalsToPascals
- AtmospheresToPascals
- PascalsToAtmospheres
- CoordinatesToMiles
- CoordinatesToKilometers

## API

- [Unit Converters source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [Unit Converters API documentation](xref:Xamarin.Essentials.UnitConverters)
