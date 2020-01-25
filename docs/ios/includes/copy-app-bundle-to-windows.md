
When building iOS apps in Visual Studio and the Mac Build agent, the .app bundle is not copied back to the Windows machine. Xamarin Tools for Visual Studio 7.4 adds a new `CopyAppBundle` property that allows CI builds to copy .app bundles back to Windows.

To use this functionality, add the`CopyAppBundle` property to the .csproj under the property group you wish to apply this functionality to. For example, the following example shows how to copy the .app bundle back to the Windows computer for a **Debug** build targeting the **iPhoneSimulator**:

```xml
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
    <CopyAppBundle>true</CopyAppBundle>
</PropertyGroup>
```
