---
title: "Install SDK"
slug: "install-sdk-swift"
hidden: false
createdAt: "2019-08-21T21:58:15.117Z"
updatedAt: "2019-08-21T21:58:23.728Z"
---
The Optimizely X Swift SDK is written completely in Swift and uses all of its native types and patterns.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "If this doesn't look familiar, you may be looking for our [Objective-C SDK](https://docs.developers.optimizely.com/full-stack/docs/install-the-objective-c-sdk)."
}
[/block]
Our Swift SDK is available for distribution through CocoaPods, Carthage, or Swift Package Manager (SPM). You can use this SDK with apps written in Swift and Objective C.

### Requirements

* Swift client applications must use Swift 5 or higher. 
* Minimum OS version supported is iOS/9.0 and tvOS/9.0.

### CocoaPods

1. Add this line to the Podfile:
[block:code]
{
  "codes": [
    {
      "code": "pod 'OptimizelySwiftSDK','~> 3.1.0'\n\n",
      "language": "text",
      "name": "iOS Podfile"
    }
  ]
}
[/block]
2. Run the command:
[block:code]
{
  "codes": [
    {
      "code": "pod install\n\n",
      "language": "text",
      "name": "Shell"
    }
  ]
}
[/block]
For more installation information, see the [CocoaPods Getting Started Guide](https://guides.cocoapods.org/using/getting-started.html).

### Carthage

1. Add this line to the _Cartfile_:
[block:code]
{
  "codes": [
    {
      "code": "github \"optimizely/swift-sdk\" ~> \"3.1.0\"\n\n",
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
      "code": "carthage update\n\n",
      "language": "text",
      "name": "Shell"
    }
  ]
}
[/block]
3. Link the frameworks to your project. 
Go to your project target's **Link Binary With Libraries** and drag over these frameworks from the Carthage/Build/&lt;platform&gt; folder:
[block:code]
{
  "codes": [
    {
      "code": "Optimizely.framework\n\n",
      "language": "text",
      "name": "Carthage folder"
    }
  ]
}
[/block]
4. To ensure that proper bitcode-related files and dSYMs are copied when archiving your app, you must install a Carthage build script:
  a. Add a new **Run Script** phase in your target's **Build Phase**.
  b. Include this line in the script area: 
`/usr/local/bin/carthage copy-frameworks`
  c. Add the frameworks to the **Input Files** list: `$(SRCROOT)/Carthage/Build/<platform>/Optimizely.framework`
  d. Add the paths to the copied frameworks to the Output Files list:
`$(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/Optimizely.framework`

For more installation information, see the [Carthage GitHub repository](https://github.com/Carthage/Carthage).

### Swift Package Manager (SPM)

Add the following line to the dependencies value of your Package.swift:
[block:code]
{
  "codes": [
    {
      "code": "dependencies: [\n    .package(url: \"https://github.com/optimizely/swift-sdk.git\", \"3.1.0\"..<\"3.2.0\")\n]\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "- All public types start with the `Optimizely` prefix for clarity.\n- This release includes a wrapper for all public APIs to support Objective-C client applications. See the [demo Swift app](https://github.com/optimizely/swift-sdk/tree/master/DemoSwiftApp) on GitHub.\n\n**If you're using the Objective-C SDK and want to transition to the Swift SDK, see our guide to [migrate to the Swift SDK from the legacy Objective-C SDK](https://docs.developers.optimizely.com/full-stack/docs/migrate-to-swift-sdk-from-objective-c-sdk).**",
  "title": "Note"
}
[/block]