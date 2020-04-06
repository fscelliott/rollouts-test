---
title: "Automatic datafile management"
slug: "automatic-datafile-management-php"
hidden: true
createdAt: "2019-09-12T16:10:52.020Z"
updatedAt: "2019-09-13T12:40:06.989Z"
---
[block:callout]
{
  "type": "danger",
  "title": "Incorporate this content in \"Initialize SDK\" topic",
  "body": "When ADM is GA for PHP, this content should be incorporated into the \"Initialize SDK\" topic (and deleted here in a separate topic) to be consistent with the other SDKs."
}
[/block]
The PHP SDK provides default implementations of  [ProjectConfigManagerInterface](https://github.com/optimizely/php-sdk/blob/master/src/Optimizely/ProjectConfigManager/ProjectConfigManagerInterface.php). 
You can instantiate the Optimizely SDK with the default configuration of [HttpProjectConfigManager](https://staging-optimizely-parent.readme.io/staging-optimizely-full-stack/docs/php-datafile-manager#section-httpprojectconfigmanager).
[block:api-header]
{
  "title": "Basic example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "$configManager = new HTTPProjectConfigManager($sdkKey);\n$optimizely = new Optimizely(\n  $datafile, \n  null, \n  null, \n  null, \n  false, \n  null, \n  $configManager, \n  $notificationCenter\n);\n",
      "language": "php"
    }
  ]
}
[/block]
[ProjectConfigManagerInterface](https://github.com/optimizely/php-sdk/blob/master/src/Optimizely/ProjectConfigManager/ProjectConfigManagerInterface.php) exposes `getConfig` method for retrieving ProjectConfig instance.
[block:api-header]
{
  "title": "HTTPProjectConfigManager"
}
[/block]
[HTTPProjectConfigManager](https://github.com/optimizely/php-sdk/blob/master/src/Optimizely/ProjectConfigManager/HTTPProjectConfigManager.php) is an implementation of [ProjectConfigManagerInterface](https://github.com/optimizely/php-sdk/blob/master/src/Optimizely/ProjectConfigManager/ProjectConfigManagerInterface.php) interface.
The `fetch` method makes a blocking HTTP GET request to the configured URL to download the project datafile and initialize an instance of the `ProjectConfig`.
[block:api-header]
{
  "title": "Advanced example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "$configManager = new HTTPProjectConfigManager(\n  $sdkKey,\n  $url,\n  $urlTemplate,\n  $fetchOnInit,\n  $datafile\n);\n\n$optimizely = new Optimizely(\n  $datafile,\n  null, \n  null, \n  null, \n  false, \n  null, \n  $configManager, \n  $notificationCenter\n);\n",
      "language": "php"
    }
  ]
}
[/block]
The `fetch` method allows you to refresh the `ProjectConfig`. In order to update the `ProjectConfig`, you can do something like:
[block:code]
{
  "codes": [
    {
      "code": "// Call fetch to retrieve an up-to-date-datafile.\n$configManager->fetch();\n// Now the Optimizely instance has the latest version of the datafile.",
      "language": "php"
    }
  ]
}
[/block]
### SDK key

The SDK key is used to compose the outbound HTTP request to the default datafile location on the Optimizely CDN.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in PHP.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Default",
    "0-0": "**sdkKey**",
    "0-1": "null",
    "1-0": "**url**",
    "1-1": "null",
    "2-0": "**urlTemplate**",
    "2-1": "null",
    "h-2": "Description",
    "0-2": "Optimizely project SDK key; required unless source URL is overridden.",
    "1-2": "URL override location used to specify custom HTTP source for the Optimizely datafile.",
    "2-2": "Parameterized datafile URL by SDK key.",
    "3-0": "**fetchOnInit**",
    "4-0": "**datafile**",
    "3-1": "false",
    "4-1": "null",
    "3-2": "Boolean flag to specify if datafile fetching should start right away, as soon as the HTTPConfigManager is initialized.",
    "4-2": "Initial datafile, typically sourced from a local cached source."
  },
  "cols": 3,
  "rows": 5
}
[/block]
### Update config notifications

A notification will be triggered whenever a new datafile is fetched and `ProjectConfig` is updated. To subscribe to these notifications, use the `$notificationCenter->addNotificationListener();`.
[block:code]
{
  "codes": [
    {
      "code": "$notificationCenter->addNotificationListener(\n   NotificationType::OPTIMIZELY_CONFIG_UPDATE,\n   $updateCallback\n);\n\n",
      "language": "php"
    }
  ]
}
[/block]