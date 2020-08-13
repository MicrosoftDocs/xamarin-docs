---
title: "How do I collect the current call stacks of the Visual Studio process?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
---

# How do I collect the current call stacks of the Visual Studio process?

When the GUI locks up (hangs, freezes) in Visual Studio, an important piece of diagnostic information to collect is the set of call stacks from all the threads of the Visual Studio process. To save this information for a hung instance of Visual Studio, you can use a second instance of Visual Studio:

1. Start a second instance (a new window) of Visual Studio.

2. Close any open solutions in the new instance of Visual Studio.

3. Select **Debug > Attach to Process**.

   ![Select Debug > Attach to Process](vs-callstack-images/image1.png)

4. Select the original hung instance of `devenv.exe` from the list of **Available Processes**.

5. Select **Debug > Break All**.

   ![Select Debug > Break All](vs-callstack-images/image2.png)

6. Select **Debug > Save Dump As**.

   ![Select Debug > Save Dump As](vs-callstack-images/image3.png)

7. Change **Save as type** to **Minidump (\*.dmp)**. This will produce a much smaller file than **Minidump with Heap**, and the heap is usually not relevant for diagnosing freezes.

   ![This will produce a much smaller file than Minidump with Heap, and the heap is usually not relevant for diagnosing freezes](vs-callstack-images/image4.png)

8. Save the dump file. If submitting the file online, you can zip it to reduce the size.
