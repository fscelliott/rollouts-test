---
title: "Event Batching FAQ"
slug: "event-batching-faq"
hidden: true
createdAt: "2020-02-12T20:08:51.495Z"
updatedAt: "2020-02-12T20:38:26.272Z"
---
[block:parameters]
{
  "data": {
    "0-0": "- [I’m calling activate/track but I see no changes on the Results page.](#changes)\n- [I made lots of activate calls, but the number on the Results page doesn’t match the number I expected.](#match)",
    "0-1": "- [I sometimes see 4xx code from Optimizely.](#4xx-code)\n- [General advice: Look for event processor log messages.](#log-messages)"
  },
  "cols": 2,
  "rows": 1
}
[/block]
<a id="changes">**I’m calling activate/track but I see no changes on the Results page.**</a>
- Check if the batch size limit or flush interval was reached before you check results.
- Check the logs to determine if the event was actually sent to the Optimizely backend.

<a id="match">**I made lots of activate calls, but the number on the Results page doesn’t match the number I expected.**</a>
- Check if the batch size limit or flush interval was reached before you check results.
- Check the logs to determine if the event was actually sent to the Optimizely backend.
- Check if your program exited abruptly. Optimizely SDKs make a best effort to send all events before shutting down.

<a id="4xx-code">**I sometimes see 4xx code from Optimizely.**</a>
- Check the batch size: if you set it too high, the event payload could be larger than 10 MB. In that case reduce the batch size / flush interval.

<a id="log-messages">**General advice: Look for event processor log messages.**</a>
- Set logging to DEBUG for maximum visibility.