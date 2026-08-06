# ConfigCat GTM Template

Official Google Tag Manager template for [ConfigCat](https://configcat.com/).

ConfigCat is a feature flag and configuration management service that lets you separate releases from deployments. You can turn your features ON/OFF using ConfigCat Dashboard even after they are deployed. ConfigCat lets you target specific groups of users based on region, email or any other custom user attribute.

## Installation

### From GTM Community Template Gallery

1. Open Google Tag Manager
2. Go to **Templates** -> **Search Gallery**
3. Search for "ConfigCat"
4. Click **Add to workspace**

### Manual Installation

1. Download `template.tpl` from this GitHub repository
2. Open Google Tag Manager
3. Go to **Templates** -> **New**
4. Click the three-dot menu (⋮) -> **Import**
5. Select the downloaded template file

## Configuration

### Required Fields

| Field | Description |
|-------|-------------|
| **SDK Key** | Your ConfigCat SDK Key. Get it from [ConfigCat Dashboard](https://app.configcat.com/). |

### Optional Fields

| Field | Description |
|-------|-------------|
| **Polling mode** | The polling mode to use to fetch the config data from the ConfigCat CDN. [More about polling modes](https://configcat.com/docs/sdk-reference/js/browser/#polling-modes). Default is `Auto polling` |
| **Data governance** | Describes the location of your feature flag and setting data within the ConfigCat CDN. This parameter needs to be in sync with your Data Governance preferences. [More about Data Governance](https://configcat.com/docs/advanced/data-governance/). Default is `Global` |
| **Base URL** | Sets the CDN base url (forward proxy, dedicated subscription) from where the SDK will download the config JSON. |

## Usage

1. Create a new tag using the ConfigCat GTM template
2. Enter your **SDK Key**
3. Set the trigger to **All Pages** (or your preferred pages)
4. Save and publish

## Using Feature Flags

To use feature flags, first create a **Custom Event** called "ConfigCatLoaded". 

1. In Google Tag Manager, go to **Triggers** -> **New** -> **Trigger Configuration**.
2. Select **Custom Event**.
3. Enter "ConfigCatLoaded" as the event name. The ConfigCat tag will push this event to the data layer when it has loaded.
4. Name the **Custom Event** trigger "ConfigCatLoaded".

Now you can create a separate **Custom HTML** tag with your logic and tigger it with the "ConfigCatLoaded" **Custom Event** you just created.

```html
<script>
  (function () {
    var sdkKey = '#YOUR-SDK-KEY#';

    if (!window.configcat) {
      console.warn('[ConfigCat] SDK not loaded');
      return;
    }

    // Same SDK key returns the shared instance created by the init tag
    var client = window.configcat.getClient(sdkKey);

    // Waiting for the feature flag configuration is downloaded and ready to use
    client.waitForReady().then(function () {
      return client.getValueAsync('ShowBlackFirdayDiscount', false);
    }).then(function (value) {
      // Example: Add a class when feature is enabled
      var element = document.getElementById("pricing");
      if (value) {
        if (element) element.classList.add("black-friday-discount");
      }
    });
  })();
</script>
```

## Need help?
https://configcat.com/support

## About ConfigCat
- [Official ConfigCat SDKs for other platforms](https://github.com/configcat)
- [Documentation](https://configcat.com/docs)
- [Blog](https://configcat.com/blog)

## License

Apache 2.0
