# DeviceFarm Web Node.js Test

## Steps to create

The environment is Node.js. For client, I chose WebdriverIO. It is one of the two clients Appium website recommends for Node.js, so it seems a safe choice.

[The Appium Clients - Appium](http://appium.io/docs/en/about-appium/appium-clients/index.html)

I created this test package based on the WebdriverIO Getting Started tutorial’s sample test:

[Getting Started · WebdriverIO](https://webdriver.io/docs/gettingstarted.html)

I diverged from the tutorial, for service, I chose appium instead of chromedriver.

Then I made the following changes:

-   Moved wdio dependencies from `devDependencies` to `dependencies` in `package.json` so that `npm-bundle` correctly includes them in the bundle.
-   In `wdio.conf.js`:
    -   Removed `browserName: 'firefox'` capability.
    -   Set `connectionRetryCount: 10` (was 3).
    -   Removed `appium` from `services` so that wdio won’t try to start the Appium service, since it is already running on the DeviceFarm instances.

These changes are visible in this commit:

[minor modifications on wdio.config.js: · gmatt/devicefarm-web-nodejs-test@d135910 · GitHub](https://github.com/gmatt/devicefarm-web-nodejs-test/commit/d13591030268333c9fedc382d9131d7ab780631b)

To run the package on DeviceFarm, I followed this tutorial:

[Working with Appium Node.js for Web Applications and AWS Device Farm - AWS Device Farm](https://docs.aws.amazon.com/devicefarm/latest/developerguide/test-types-web-app-appium-node.html)

Ran these commands:

```bash
npm-bundle
```

```bash
zip -r /MyTests.zip/ *.tgz
```

Then on DeviceFarm web console:

[AWS Device Farm](https://console.aws.amazon.com/devicefarm)

-   [Select project] > Create a new run
-   Web Application icon > Next
-   Test: Appium Node.js
-   [Upload the zip file]

-   Edit YAML:

replace

```yaml
node YOUR_TEST_FILENAME.js
```

with

```yaml
./node_modules/.bin/wdio wdio.conf.js
```

I saved the yaml as `wdio-testspec.yml`.

-   It is available as an upload here:
    -   https://console.aws.amazon.com/devicefarm/home#/projects/b6cf3b2c-8863-40e0-9b53-7c2b22c6986a/settings/uploads
-   And here:

    -   [devicefarm-web-nodejs-findings/wdio-testspec.yml at master · gmatt/devicefarm-web-nodejs-findings · GitHub](https://github.com/gmatt/devicefarm-web-nodejs-findings/blob/master/wdio-testspec.yml)

-   Next step > [Set device pool] > Next step > Confirm and start run

## Resources

-   The zip package is available here:
    -   [devicefarm-web-nodejs-test/MyTests.zip at master · gmatt/devicefarm-web-nodejs-test · GitHub](https://github.com/gmatt/devicefarm-web-nodejs-test/blob/master/MyTests.zip)
-   Or as an upload here:
    -   https://console.aws.amazon.com/devicefarm/home#/projects/b6cf3b2c-8863-40e0-9b53-7c2b22c6986a/settings/uploads
