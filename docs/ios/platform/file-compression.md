---
title: "File compression in Xamarin.iOS"
description: "This document describes how to work with the libcompression API in Xamarin.iOS. It discusses deflating, inflating, and the different supported algorithms."
ms.service: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.subservice: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
no-loc: [Objective-C]
---
# File compression in Xamarin.iOS

Xamarin apps targeting iOS 9.0 or macOS 10.11 (and newer) can use the _Compression Framework_ to compress (encode) and decompress (decode) data. Xamarin.iOS provides this framework following the Stream API. The compression framework allows developers to interact with the compress and decompressed data as if they are normal streams without the need to use callbacks or delegates.

The compression framework provides support for the following algorithms:

* LZ4
* LZ4 Raw
* Lzfse
* Lzma
* Zlib

Using the compression framework allows developers to perform compression operations without any third-party libraries or NuGets. This reduces external dependencies and ensures that the compression operations will be supported on all platforms (as long as they meet the minimum OS requirements).

## General file decompression

The compression framework uses a stream API in Xamarin.iOS and Xamarin.Mac. This API means that to compress data, the developer can use
the normal patterns used in other IO APIs within .NET. The following sample shows how to decompress data with the compression framework, which is similar
to the API found in the `System.IO.Compression.DeflateStream` API:

```csharp
// sample zlib data
static byte [] compressed_data = { 0xf3, 0x48, 0xcd, 0xc9, 0xc9, 0xe7, 0x02, 0x00 };

using (var backing = new MemoryStream (compressed_data)) // compress data to read
using (var decompressing = new CompressionStream (backing, CompressionMode.Decompress, CompressionAlgorithm.Zlib)) // create decompression stream with the correct algorithm
using (var reader = new StreamReader (decompressing))
{
    // perform the required stream operations
    Console.WriteLine (reader.ReadLine ());
}
```

The `CompressionStream` implements the `IDisposable` interface, like other `System.IO.Streams`, so developers should ensure that resources are freed once they are no longer required.

## General file compression

The Compression API also allows developers to compress data following the same API. Data can be compressed using one of the provided algorithms stated in the `CompressionAlgorithm` enumerator.

```csharp
// sample method that copies the data from the source stream to the destination stream
static void CopyStream (Stream src, Stream dest)
{
    byte[] array = new byte[1024];
    int bytes_read;
    bytes_read = src.Read (array, 0, 1024);
    while (bytes_read != 0) {
        dest.Write (array, 0, bytes_read);
        bytes_read = src.Read (array, 0, 1024);
    }
}

static void CompressExample ()
{
    // create some sample data to compress
    byte [] data = new byte[100000];
    for (int i = 0; i < 100000; i++) {
        data[i] = (byte) i;
    }

    using (var dataStream = new MemoryStream (data)) // stream that contains the data to compress
    using (var backing = new MemoryStream ()) // stream in which the compress data will be written
    using (var compressing = new CompressionStream (backing, CompressionMode.Compress, CompressionAlgorithm.Zlib, true))
    {
        // copy the data to the compressing stream
        CopyStream (dataStream, compressing);
        // at this point, the CompressionStream contains all the data from the dataStream but compressed
        // using the zlib algorithm
    }
```

## Async support

The `CompressionStream` supports all the async operations that are supported by the `System.IO.DeflateStream`, which means that developers can use the async keyword to perform the compress/decompress operations without blocking the UI thread.
