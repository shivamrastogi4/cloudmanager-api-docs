---
title: Tutorial Step 3 - Cloud Manager API
description: This is step 3 of the Cloud Manager API Tutorial
keywords:
  - Experience Cloud
  - Adobe Experience Manager
  - API Documentation
  - Cloud Manager
  - Tutorial
  - JavaScript
---

import Glitch from "../../components/glitch"

# Tutorial Step 3 - Handling Specific Events

As seen in [Step 1](1-a-basic-webhook.md), webhooks can be registered for one or more events. Depending on the type of integration you are actually building with Cloud Manager, you will end up with a single webhook script which can handle multiple events in different ways. The event can be identified by using the combination of the `@type` and `xdmEventEnvelope:objectType` values. A full list can be found on the [Receiving Events](/guides/api-usage/receiving-events/) page. For this step in the tutorial, you're going to add a simple log statement when event being received is a Pipeline Execution Started event.

## Updating the Webhook

Replace the `app.post` block with this code:

```javascript
app.post("/webhook", (req, res) => {
  res.writeHead(200, { "Content-Type": "application/text" });
  res.end("pong");

  const STARTED = "https://ns.adobe.com/experience/cloudmanager/event/started";
  const EXECUTION =
    "https://ns.adobe.com/experience/cloudmanager/pipeline-execution";

  const event = req.body.event;

  if (
    STARTED === event["@type"] &&
    EXECUTION === event["xdmEventEnvelope:objectType"]
  ) {
    console.log("received execution start event");
  }
});
```

Now when a Pipeline Execution Started event is received, the message `received execution start event` will be logged.

## Running the Updated Webhook

If you are running the script locally, you'll need to stop and restart the node process. You don't need to restart ngrok. In fact, if you do restart ngrok, the URL will likely change and you'll need to go back into the <a href="https://developer.adobe.com/console/projects" target="_new">Adobe Developer Console</a> and update the Webhook URL.

If you are running the script through Glitch, Glitch will restart automatically. If you don't want to update your existing Glitch project (or lost it), you can click the button below to start over.

<Glitch projectName="adobe-cloudmanager-api-tutorial-step3" />

## Next Step

With all that done, you're ready to proceed to the next step. Continue to [Step 4](4-getting-an-access-token.md).
