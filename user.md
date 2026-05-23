---
title: User
version: 3.0.0+
source_path: user.md
docs_url: https://docs.intrack.ir/docs/getting-start/user
---
# User
## Types of Users
:::info

    At **inTrack**, anybody who has interacted with your business at least once is called a User.

:::
### Unknown Users
When a new visitor lands on your website or downloads your app, they are initially tracked as anonymous users. This means that an Unknown User Profile is created for them in [dashboard](https://dash.intrack.ir)

**inTrack** SDK automatically generate a random id for each device known as a **deviceId**, for these unknown users. As the user interacts with your platform, their profile begins to populate with various data points.

- **[System User Attributes](events#system-attributes):** Automatically collected attributes such as device type, browser, etc.
- **[Custom User Attributes](events#custom-attributes):** Specific user details that you choose to track, like preferences or interests.
- **[System Events](events#system-events):** Pre-defined events such as page views, clicks, etc.
- **[Custom Events](events#custom-events):** User-specific actions or interactions that you define and track.

### Known Users
As users interact further and provide identifiable information, you have the option to assign them a unique ID, known as a **UserID**. This transition marks them as Known Users within our system.

Once a user is identified and assigned a UserID, their profile undergoes an upgrade. All the data previously associated with their **DeviceID** (from the Unknown User stage) is merged with the new identifiable data. This ensures a seamless user profile that captures both anonymous and known interactions, providing a holistic view of the user's journey.

By distinguishing between Unknown and Known Users, **inTrack** allows businesses to track and engage users effectively, whether they are anonymous visitors or identified customers.
