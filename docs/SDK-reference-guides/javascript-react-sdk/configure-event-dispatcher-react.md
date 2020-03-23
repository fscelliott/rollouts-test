---
title: "Configure event dispatcher"
slug: "configure-event-dispatcher-react"
hidden: false
createdAt: "2019-09-12T15:03:18.419Z"
updatedAt: "2019-09-26T17:12:36.832Z"
---
The Optimizely SDKs make HTTP requests for every impression or conversion that gets triggered. The React SDK has a built-in **event dispatcher** for handling these events.

You can customize the event dispatcher you use in production to have more control over the way the SDK sends events so that it scales to the volumes handled by your application. Customizing the event dispatcher allows you to take advantage of features like batching, which makes it easier to handle large event volumes efficiently or to implement retry logic when a request fails. You can build your dispatcher from scratch or start with the provided dispatcher.

The examples below shows how to initialize the Optimizely client (or manager) with an event dispatcher instance:
[block:code]
{
  "codes": [
    {
      "code": "import { createInstance, eventDispatcher } from '@optimizely/react-sdk';\n\n// Create an Optimizely client with the default event dispatcher\n// Note: If you don't pass eventDispatcher, this default event dispatcher\n// will be used automatically. This is just to demonstrate how to provide\n// an event dispatcher when creating an instance.\nconst optimizely = createInstance({\n  datafile: window.datafile, // assuming you have a datafile at window.datafile\n  eventDispatcher: eventDispatcher,\n});",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]
The event dispatcher should implement a `dispatchEvent` function, which takes in three arguments: `httpVerb`, `url`, and `params`, all of which are created by the internal `EventBuilder` class. In this function, you should send a `POST` request to the given `url` using the `params` as the body of the request (be sure to stringify it to JSON) and `{content-type: 'application/json'}` in the headers.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "If you are using a custom event dispatcher, do not modify the event payload returned from Optimizely. Modifying this payload will alter your results."
}
[/block]