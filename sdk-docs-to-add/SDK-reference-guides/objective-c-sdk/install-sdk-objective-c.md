---
title: "Install SDK"
slug: "install-sdk-objective-c"
hidden: false
createdAt: "2019-09-11T13:56:07.698Z"
updatedAt: "2019-09-13T11:58:10.613Z"
---
Our Objective-C-powered iOS SDKs are available for distribution through CocoaPods and Carthage, or they can be manually installed. You can use these SDKs with apps written in Objective-C and Swift.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "This iOS SDK is powered by Objective-C. You can also use our [iOS SDK powered by Swift](doc:install-the-swift-sdk) if you prefer."
}
[/block]
The Objective-C SDK changelog is also available in the [SDK's GitHub repo](https://github.com/optimizely/objective-c-sdk/blob/master/CHANGELOG.md).
[block:api-header]
{
  "title": "Requirements"
}
[/block]
iOS 8.0+ or tvOS 9.0+
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "In the instructions below, `<platform>` is used to represent the platform on which you are building your app. Currently, we support iOS and tvOS platforms. You should be pinning your dependency to a major or minor of the SDK to prevent breaking changes from major version upgrades from affecting your build."
}
[/block]

[block:api-header]
{
  "title": "CocoaPods"
}
[/block]
1. Add this line to the Podfile:
[block:code]
{
  "codes": [
    {
      "code": "pod 'OptimizelySDKiOS','3.0.0'\n",
      "language": "text",
      "name": "iOS Podfile"
    }
  ]
}
[/block]
   Alternatively, if you are developing on tvOS, use:
[block:code]
{
  "codes": [
    {
      "code": "pod 'OptimizelySDKTVOS','3.0.0'\n",
      "language": "text",
      "name": "tvOS Podfile"
    }
  ]
}
[/block]

Make sure `use_frameworks!` is also included in the Podfile:
[block:code]
{
  "codes": [
    {
      "code": "target 'Your App' do\n use_frameworks!\n pod 'OptimizelySDKiOS','3.0.0'\nend\n\n",
      "language": "text",
      "name": "Podfile"
    }
  ]
}
[/block]
2. Run the command:
[block:code]
{
  "codes": [
    {
      "code": "pod install",
      "language": "text",
      "name": "Shell"
    }
  ]
}
[/block]
 For more installation information, see the [CocoaPods Getting Started Guide](https://guides.cocoapods.org/using/getting-started.html).
[block:api-header]
{
  "title": "Carthage"
}
[/block]
1. Add this line to the _Cartfile_:
[block:code]
{
  "codes": [
    {
      "code": "github \"optimizely/objective-c-sdk\" \"3.0.0\"\n",
      "language": "text",
      "name": "Cartfile"
    }
  ]
}
[/block]
2. Run the command:
[block:code]
{
  "codes": [
    {
      "code": "carthage update\n",
      "language": "text",
      "name": "Shell"
    }
  ]
}
[/block]
3. Link the frameworks to your project. 
Go to your project target's **Link Binary With Libraries** and drag over these frameworks from the _Carthage/Build/<platform>_ folder:
[block:code]
{
  "codes": [
    {
      "code": "OptimizelySDKCore.framework\nOptimizelySDKDatafileManager.framework\nOptimizelySDKEventDispatcher.framework\nOptimizelySDKShared.framework\nOptimizelySDKUserProfileService.framework\nOptimizelySDK<platform>.framework\n\n",
      "language": "text",
      "name": "Carthage folder"
    }
  ]
}
[/block]
4. To ensure that proper bitcode-related files and dSYMs are copied when archiving your app, you must install a Carthage build script:
      a. Add a new **Run Script** phase in your target's **Build Phase**.
      b. Include this line in the script area: `/usr/local/bin/carthage copy-frameworks`
      c. Add the frameworks to the **Input Files** list:
[block:code]
{
  "codes": [
    {
      "code": "$(SRCROOT)/Carthage/Build/<platform>/OptimizelySDKCore.framework\n$(SRCROOT)/Carthage/Build/<platform>/OptimizelySDKDatafileManager.framework\n$(SRCROOT)/Carthage/Build/<platform>/OptimizelySDKEventDispatcher.framework\n$(SRCROOT)/Carthage/Build/<platform>/OptimizelySDKShared.framework\n$(SRCROOT)/Carthage/Build/<platform>/OptimizelySDKUserProfileService.framework\n$(SRCROOT)/Carthage/Build/<platform>/OptimizelySDK<platform>.framework\n\n",
      "language": "text",
      "name": "Input Files list"
    }
  ]
}
[/block]
For more installation information, see the [Carthage GitHub repository](https://github.com/Carthage/Carthage).
[block:api-header]
{
  "title": "Manual installation"
}
[/block]
The universal framework can be used in an application without the need for a third-party dependency manager. The universal framework packages up all Optimizely X Mobile modules, which include:
  * OptimizelySDKCore
  * OptimizelySDKShared
  * OptimizelySDKDatafileManager
  * OptimizelySDKEventDispatcher
  * OptimizelySDKUserProfileService

The universal framework for iOS includes builds for these architectures:
  * i386
  * x86_64
  * ARMv7
  * ARMv7s
  * ARM64

The universal framework for tvOS includes build for these architectures:
  * x86_64
  * ARM64

Bitcode is enabled for both the iOS and tvOS universal frameworks.

To install the universal framework:

1. Download the [iOS](https://github.com/optimizely/objective-c-sdk/blob/master/OptimizelySDKUniversal/generated-frameworks/Release-iOS-universal-SDK/OptimizelySDKiOS.framework.zip) or [tvOS](https://github.com/optimizely/objective-c-sdk/blob/master/OptimizelySDKUniversal/generated-frameworks/Release-tvOS-universal-SDK/OptimizelySDKTVOS.framework.zip) framework.

2. Unzip the framework, then drag the framework to your project in Xcode. Xcode should prompt you to select a target. Go to **Build Phases** and make sure that the framework is under the **Link Binary with Libraries** section.

3. Go to the **General** tab and add the framework to the **Embedded Binaries** section. If the **Embedded Binaries** section is not visible, add the framework in the **Copy Files** section (you can add this section in **Build Settings**).

4. The Apple store will reject your app if you have the universal framework installed as it includes simulator binaries. You must run a script to strip the extra binaries before you upload the app. To do this:
a. Go to **Build Phases** and add a **Run Script** section by clicking the ```+``` symbol. 
b. Copy and paste the script below, replacing the ```FRAMEWORK_NAME``` with the proper framework name:
[block:code]
{
  "codes": [
    {
      "code": "FRAMEWORK=\"FRAMEWORK_NAME\"\nFRAMEWORK_EXECUTABLE_PATH=\"${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/$FRAMEWORK.framework/$FRAMEWORK\"\nEXTRACTED_ARCHS=()\nfor ARCH in $ARCHS\ndo\n    lipo -extract \"$ARCH\" \"$FRAMEWORK_EXECUTABLE_PATH\" -o \"$FRAMEWORK_EXECUTABLE_PATH-$ARCH\"\n    EXTRACTED_ARCHS+=(\"$FRAMEWORK_EXECUTABLE_PATH-$ARCH\")\ndone\nlipo -o \"$FRAMEWORK_EXECUTABLE_PATH-merged\" -create \"${EXTRACTED_ARCHS[@]}\"\nrm \"${EXTRACTED_ARCHS[@]}\"\nrm \"$FRAMEWORK_EXECUTABLE_PATH\"\nmv \"$FRAMEWORK_EXECUTABLE_PATH-merged\" \"$FRAMEWORK_EXECUTABLE_PATH\"\n\n",
      "language": "text",
      "name": "Script"
    }
  ]
}
[/block]
To build the universal framework:
1. Build the _OptimizelySDKiOS-Universal_ or _OptimizelySDKTVOS-Universal_ schemes. 
2. Update our third-party dependencies, which are pulled in as Git submodules, by running this command:
[block:code]
{
  "codes": [
    {
      "code": "git submodule init\n",
      "language": "text",
      "name": "Shell"
    }
  ]
}
[/block]
followed by:
[block:code]
{
  "codes": [
    {
      "code": "git submodule update\n",
      "language": "text",
      "name": "Shell"
    }
  ]
}
[/block]
After building these schemes, the frameworks are output in the _OptimizelySDKUniversal/generated-frameworks_ folder.

The full source code is at https://github.com/optimizely/objective-c-sdk.