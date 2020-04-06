---
title: "Pass in audience attributes"
slug: "pass-in-audience-attributes"
hidden: false
createdAt: "2019-09-27T21:57:26.365Z"
updatedAt: "2019-09-27T22:01:32.726Z"
---
In React SDK, you can provide user id and user attributes together as part of the `user` prop, which is passed to the `OptimizelyProvider` component.
[block:code]
{
  "codes": [
    {
      "code": "import React from 'react';\nimport {\n  createInstance,\n  OptimizelyProvider,\n} from '@optimizely/react-sdk'\n\nconst optimizely = createInstance({\n  sdkKey: '<Your_SDK_Key>', // TODO: Update to your SDK Key\n})\n\n// You can pass strings, numbers, Booleans, and null as user attribute values.\n// The example below shows how to pass in attributes.\nconst user = {\n  id: ‘myuser123’,\n  attributes: {\n    device: 'iPhone',\n    lifetime: 24738388,\n    is_logged_in: true,\n  },\n};\n\nfunction App() {\n  return (\n    <OptimizelyProvider\n      optimizely={optimizely}\n      user={user}\n    >\n    {/* … your application components here … */}\n    </OptimizelyProvider>\n  </App>\n}\n\n",
      "language": "javascript"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "During audience evaluation, note that if you don't pass a valid attribute value for a given audience condition—for example, if you pass a string when the audience condition requires a Boolean, or if you simply forget to pass a value—then that condition will be skipped. The [SDK logs](doc:customize-logger-react) will include warnings when this occurs."
}
[/block]