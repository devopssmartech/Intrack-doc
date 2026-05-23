---
title: Getting Started
version: 3.0.0+
source_path: web/web-getting-started.md
docs_url: https://docs.intrack.ir/docs/getting-start/web/web-getting-started
---
# Getting Started

The **inTrack** Web SDK enables seamless web push notifications and comprehensive data gathering from web applications.

To enhance performance and security, the SDK leverages WebAssembly technology. Consequently, data collection is only supported on browsers that have webAssembly capabilities. [List of Browser](https://caniuse.com/wasm).

## Integration

To integrate the **inTrack** Web SDK, simply add the following code snippet to the end of the `<head>` section on each page you want to track:

### Asynchronous

```html
{
  `<!-- $inTrack Initialization-->
  <script type="text/javascript">
    var $inTrack_config = {
      app_key: 'App_Key',
      auth_key: 'Auth_Key',
      public_key: 'Public_Key',
      environment: 'production',
    };
    (function (i, n, t, r, a, c, k) {
      o = i['$InTrack'] = i['$InTrack'] || {}; i[a] = i[a] || function () { (o.q = o.q || []).push(arguments); };
      s = n.createElement(t); s.async = true; s.src = r; s.onload = function () { o.init(c) };
      e = n.getElementsByTagName(t)[0]; e.parentNode.insertBefore(s, k);
    })(window, document, 'script', "//static1.intrack.ir/api/web/download/sdk/v1/inTrack.min.js?v=00", "$Intk", inTrack_config);
  </script>
  <!-- End inTrack Initialization -->
`
}
```

### Synchronous

```html
{
  `<!-- $inTrack Initialization -->
  <script
    type="text/javascript"
    src="https://static1.intrack.ir/api/web/download/sdk/v1/inTrack.min.js?v=00"/>
  <script type="text/javascript">
    $InTrack.init({
      app_key: 'Provided_App_Key',
      auth_key: 'Provided_Auth_Key',
      public_key: 'Provided_Public_Key',
      environment: 'production',
    });
  </script>
  <!-- End $inTrack Initialization -->
`
}
```

:::info

Check your **APP_KEY** , **AUTH_KEY** and **Public_Key**, in [inTrack Panel](https://dash.intrack.ir)->setting->SDK->Initialize, choose SDK type in **Integration Guide for** dropdown, Now your keys are in Initialization step.

:::

:::warning
Also, you should use one of the values `production` or `stage` for the parameter `environment` according to your environment. by default sdk will consider it as `production` if you not pass it.
You should also give your website/webapp's production domain to the product or customer success team before you launch your website/webapp. Otherwise, sdk will not work properly in production mode.
:::

:::note

we strongly recommend to using our ASynchronous method, However if for any reason you choose to use Synchronous method, you shouldn't load sdk script with async or deffer attributes and place the script in Head tag. (otherwise you might loose some events or getting exceptions).

:::

## Advance features

### Logging

If logging is enabled, then our SDK will print out debug messages about its internal state and encountered problems. Those messages will be screened in browser's console.

you can set the debug flag of the SDK configuration object to enable logging:

### Asynchronous

```html
{
  `<!-- $inTrack Initialization-->
  <script type="text/javascript">
    var $inTrack_config = {
      app_key: 'App_Key',
      auth_key: 'Auth_Key',
      public_key: 'Public_Key',
      environment: 'production',
      debug: true, //<--
    };
    (function (i, n, t, r, a, c, k) {
      o = i['$InTrack'] = i['$InTrack'] || {}; i[a] = i[a] || function () { (o.q = o.q || []).push(arguments); };
      s = n.createElement(t); s.async = true; s.src = r; s.onload = function () { o.init(c) };
      e = n.getElementsByTagName(t)[0]; e.parentNode.insertBefore(s, k);
    })(window, document, 'script', "//static1.intrack.ir/api/web/download/sdk/v1/inTrack.min.js?v=00", "$Intk", $inTrack_config);
  </script>
  <!-- End $inTrack Initialization -->
`
}
```

### Synchronous

```html
{
  `<!-- $inTrack Initialization -->
    <script
      type="text/javascript"
      src="https://static1.intrack.ir/api/web/download/sdk/v1/inTrack.min.js?v=00"/>
    <script type="text/javascript">
      $InTrack.init({
        app_key: 'Provided_App_Key',
        auth_key: 'Provided_Auth_Key',
        public_key: 'Provided_Public_Key',
        environment: 'production',
        debug: true, //<--
      });
    </script>
    <!-- End $inTrack Initialization -->
`
}
```

### Sharing audience with Ad Networks

you can sync inTrack users id with Ad Networks like MediaAD and YektaNet, for this reason you should add following properties to SDK configuration object:

```javascript
{
  `var $intrack_config = {
    app_key: 'Provided_App_Key',
    auth_key: 'Provided_Auth_Key',
    public_key: 'Provided_Public_Key',
    environment: 'production',
    yektanet_syncing: true, //<-- sync $intrack user id with yektaNet
    mediaad_syncing: true, //<-- sync $intrack user id with mediaAD
  }
`
}
```
