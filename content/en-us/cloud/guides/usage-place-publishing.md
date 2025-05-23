---
title: Usage guide for place publishing
description: Explains how to use Open Cloud Place Publishing API to publish places programmatically with version control.
---

The [place publishing API](../../reference/cloud/universes-api/v1.json) offers similar functionality to [publishing to Roblox](../../production/publishing/publish-experiences-and-places.md) in Studio, with extra permission control and automation in continuous release workflow. You can use this API to update existing places of an experience to a new version, making it useful for automating your publishing workflow. For example, you can call this API from a GitHub action and have it automatically push a binary place file to Roblox after successful integration testing.

## Limitations

- This API only supports HTTPS requests.
- The place publishing API doesn't update certain instance types. If your experience contains `Class.EditableImage`, `Class.EditableMesh`, `Class.PartOperation`, `Class.SurfaceAppearance`, or `Class.BaseWrap` instances, publish from Studio after modifying them.

## Places on Roblox

Experiences on Roblox can have multiple places, with one **starting place** as the user entry point of your experience and optional other places that you can [teleport users between](../../projects/teleport.md). You can use the API to publish either the starting place or other places of your experience with version control.

Before you can update an existing place of an experience, you need the place's **Place ID** and the experience's **Universe ID**. The **Place ID** identifies the place and the **Universe ID** identifies the experience. Each is unique even if your experience has only one place.

## Publish a place

To publish an existing place of an experience with [Place Publishing API](../../reference/cloud/universes-api/v1.json):

1. [Create an API key](../auth/api-keys.md#create-api-keys) on the **Creator Dashboard**. Make sure you perform the following settings:

   1. Add **universe-places** to **Access Permissions**.
   1. Add **Write** operation to your selected experience.

1. Get the **Universe ID** of the experience in which you want to publish the place.

   1. Navigate to the [Creator Dashboard](https://create.roblox.com/dashboard/creations).
   1. Find the experience with the place that you want to update.
   1. Hover over the experience's thumbnail, click the **&ctdot;** button, and select **Copy Universe ID**.

      <img src="../../assets/creator-dashboard/Options-Button-Experience-Public.png" width="200" />

1. Get the **Place ID** of the place that you want to update.

   1. Stay on the [Creations](https://create.roblox.com/dashboard/creations) page on **Creator Dashboard** and click the thumbnail of the target experience.
   1. On the left navigation menu, click the **Places** tab.
   1. Find the place that you want to update and click its thumbnail.
   1. The **Place ID** is in the redirected URL. For example, in the URL `https://create.roblox.com/dashboard/creations/experiences/0000000/places/111111/configure`, the **Place ID** is `111111`.

1. Add the API Key in the **x-api-key** header of a `POST` request to the API. The following two example requests reference a Roblox XML place file (`.rbxlx`) and a binary place file (`.rbxl`).

   ```bash title="XML Place File Request"
   $ curl --verbose --location POST 'https://apis.roblox.com/universes/v1/{universeId}/places/{placeId}/versions?versionType=Published' \
   --header 'x-api-key: <your-api-key>' \
   --header 'Content-Type: application/xml' \
   --data-binary @/home/placefiles/place1.rbxlx
   ```

   ```bash title="Binary Place File Request"
   $ curl --verbose --location POST 'https://apis.roblox.com/universes/v1/{universeId}/places/{placeId}/versions?versionType=Published' \
   --header 'x-api-key: <your-api-key>' \
   --header 'Content-Type: application/octet-stream' \
   --data-binary @/home/placefiles/place1.rbxl
   ```

   To run the commands, replace `{universeId}` and `{placeId}` with the actual **Universe ID** and **Place ID** of the experience and place that you want to publish.

1. If you send your request correctly, you receive a success response body with the place version number in the following format:

   ```json title="Example Response Body"
   { "versionNumber": 7 }
   ```

1. (Optional) Verify the upload in Studio or on [Creator Dashboard](https://create.roblox.com/dashboard/creations).
