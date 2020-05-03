# Homebridge HTTP Window Blinds

Repository forked from [homebridge-http-switch](https://github.com/Supereg/homebridge-http-switch) to enable the use of regex expressions with the response from the window blind status endpoint.

This is based on an Archived module by @jeffreylanters
A simple [Homebridge](https://github.com/nfarina/homebridge) plugin for controlling window blinds over HTTP. This required a DIY device to move the windows blinds cord.

This plugin handles opening and closing blinds from 0% to 100%. It also updates the current position of the blinds if they moved outside HomeKit with other means e.g manual url request.

## IMPORTANT

- Requires Node.js >= 7.6.0
- If you're seeing errors then please check your node version before creating a new issue: node -v.

## Installation

- Install homebridge: `sudo npm install -g homebridge`
- Install this plugin: `sudo npm install -g homebridge-http-window-blinds-pattern`
- Update your configuration file. See config sample below.

## Config sample

```json
{
    // ...
    "accessories": [
        {
            // Required
            "accessory": "HttpWindowBlindsPattern",
            "name": "Office Blinds",
            "urlSetTargetPosition": "http://192.168.1.100/api/blinds?open=%VALUE%",
            "urlGetCurrentPosition": "http://192.168.1.100/api/status",

            // Optional
            "statusPattern": "([0-9]+)", // This is the default pattern
            "matchingGroup": 0,
            "outputValueMultiplier": 0.8,  // Default value 1
            "debug": true
        }
    ],
    // ...
}
```

## Under the hood

When setting the target position the url from the config will be used. %VALUE% will be replaced will the value passed by your Homekit app. This will be a value between 0 and 100 depending on the status of your window covering. Once the HTTP request was done, the current position will be updated so the Homekit app will complete and can take another request. There is no time-out for this request.

The response from urlGetCurrentPosition should match the RegExp given in statusPattern config value. This value should be an integer value within 0 and 100.
