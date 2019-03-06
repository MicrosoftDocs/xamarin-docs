---
title: "File system compression in Xamarin.iOS"
description: "This document describes how to work with the libcompression API in Xamarin.iOS. It discusses deflating, inflating and the different supported algorithms.
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: mandel
ms.author: mandel
ms.date: 04/03/2019
---
# File system compression in Xamarin.iOS

As of iOS 9.0+ and Mac OS X 10.11+ applications can use the Compression Framework to compress (encode) and decompress (decode) data. Within
Xamarin this framework has been binded following the Stream API. This allows developers to interact with the compress and decompressed data
as if they were normal streams without the need to use callbacks or delegates.

The Compression Framework provides support for the following algorithms:

* LZ4
* LZ4 Raw
* Lzfse
* Lzma
* Zlib

Using the Compression Framework allows developers to perform the compression operations without the need of using any third party libraries or nugets minimizing
the dependencies as well as ensuring that the compresison operations will be supported on all platforms as long as they meet the minimun OS requirements.

## General file decompression

As previously mentioned, the Compression Framework has been binded following the stream API. This means that in order to compress data, the developer can use
the normal patterns used in other IO APIs within .Net. The following sample shows how to decompress data with the Compression Framework which is very similar
to the API found in the System.IO.Compression.DeflateStream API.

```csharp
// sample zlib data 
static byte [] compressed_data = { 0xf3, 0x48, 0xcd, 0xc9, 0xc9, 0xe7, 0x02, 0x00 };

using (var backing = new MemoryStream (compressed_data)) // compress data to read
using (var decompressing = new CompressionStream (backing, CompressionMode.Decompress, CompressionAlgorithm.Zlib)) // create decompressin stream with the correct algorithm
using (var reader = new StreamReader (decompressing))
{
    // perform the required stream operations
    Console.WriteLine (reader.ReadLine ());
}
```

As with other System.IO.Streams, the CompressionStream implementes the IDisposable interface, and therefore developers should ensure that resources are 
freed once they are not longed required.

## General file compression

The Compression API also allows to compress data following the same API. Data can be compressed using one of the provided algorithms stated in the CompressionAlgorithm enumerator.

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

The CompressionStream supports all the async operations that are supported by the System.IO.DeflateStream, this means that developers can use
the async keyword to perform the compress/decompress operations without blocking the UI thread.
