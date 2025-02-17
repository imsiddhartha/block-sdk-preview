# apama-analytics-builder-block-sdk

This is a Software Development Kit (SDK) for producing blocks for the Apama Analytics Builder Block SDK.

This is currently incomplete - please check back later.

## Disclaimer

These tools are provided as-is and without warranty or support. They do not constitute part of the Software AG product suite. Users are free to use, fork and modify them, subject to the license agreement. While Software AG welcomes contributions, we cannot guarantee to include every contribution in the master project.

## Licensing

This project is licensed under the Apache 2.0 license - see <https://www.apache.org/licenses/LICENSE-2.0>

This excludes the Analytics Framework 'CDP' file, which is provided in binary form only for the purpose of testing.

## Analytics Builder version

This version of the SDK supports Analytics Builder 10.5.0 (October 2019).  To make use of this, you will require an installation of Apama 10.5.0. If you do not already have access, then you can download the 'community' edition from <http://www.apamacommunity.com/downloads/>

## Documentation

The guide to writing blocks is available in the [doc directory](doc/000-contents.md) and there is [ApamaDoc reference](https://imsiddhartha.github.io/block-sdk-preview/doc/apamadoc/index.html) available.

## Getting started

From an Apama command prompt:

* Run sample tests (Windows):
```bat
set ANALYTICS_BUILDER_SDK=%cd%
cd samples/tests
pysys run
```

* Run sample tests (Unix):

```bash
export ANALYTICS_BUILDER_SDK=`pwd`
cd samples/tests
pysys run
```

* Package samples as an extension and upload:

```bash
analytics_builder build extension --input samples/blocks --cumulocity_url <URL> --username <tenantID>/<username> --password <password> --name sample-blocks
```
