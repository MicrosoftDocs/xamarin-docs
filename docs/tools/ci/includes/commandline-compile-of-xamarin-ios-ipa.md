
The following command line to specify a Release build of the solution **SOLUTION_FILE.sln** for the iPhone. The location of the IPA can be set by specifying the `IpaPackageDir` property on the command line:

 - On the Mac, using **xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

The **xbuild** command is typically found in the directory **/Library/Frameworks/Mono.framework/Commands**.

 - On Windows, using **msbuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**msbuild** will not automatically expand `$( )` expressions passed in by the command line. For this reason it is recommended to use a full path when setting the `IpaPackageDir` at the command line.


See the [Release Notes for iOS 9.8](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location) for more details about the `IpaPackageDir` property.
