# puppeteer-datalayer

puppeteer-datalayer is a node.js module to facilitate interaction with Google Tag Manager's dataLayer in combination with [Puppeteer](https://pttr.dev).

## What's it for?

puppeteer-datalayer was built with the following two main use cases in mind.

Especially when used with other testing tools like [Jest](https://jestjs.io) and [joi](https://github.com/hapijs/joi) it can be a huge timesaver, because you won't have to spend all your time manually clicking through your site to check if your tracking implementation is doing what it's supposed to.

### Frontend Testing

Puppeteer is very useful when doing frontend testing, i. e. checking whether your dataLayer events are trigger properly upon certain user actions on the website, e. g. adding a product to the shopping cart. Checking if the expected event is pushed to dataLayer after mocking the user action is a lot easier with puppeteer-datalayer.

### GTM Testing

Make sure your tags in GTM work as expected by pushing predefined events to dataLayer. Afterwards you can listen to network requests in Puppeteer to see if the expected requests (Google Analytics, 3rd party conversion tags, …) were triggered and contain the necessary parameters.

## Installation

You can install this package from [npm](http://npmjs.com/) by executing the following command while in your project directory:

    npm install puppeteer-datalayer --save

## Glossary

To clarify the terminology used in this package:

### Message

A _message_ describes any object inside dataLayer regardless of its attributes. These are examples of messages pushed to dataLayer:

```js
dataLayer.push({ event: "addToBasket", product: { id: 12345 } })
dataLayer.push({ order: null })
dataLayer.push({})
```

### Event

An _event_ describes any object inside dataLayer that has an `event` attribute. An event is always a _message_ but not every message is an event.
The name applies no matter whether an event was pushed by a developer-implemented `dataLayer.push()` or the automatically collected events starting with `gtm.`.

```js
// This is an event
dataLayer.push({ event: "gtm.click" })

// This one, too
dataLayer.push({ event: "addToBasket", product: { id: 12345 } })

// This is not an event, just a message
dataLayer.push({ order: null })
```

## Credits

Special thanks go out to the following people whose content represents the basis this package was built on. Check out their awesome resources:

-   [Simo Ahava](https://www.simoahava.com/)
-   [Jethro Nederhof](https://github.com/jethron)

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

-   [constructor](#constructor)
-   [get](#get)
-   [getContainerIDs](#getcontainerids)
-   [getDataModel](#getdatamodel)
-   [getEvents](#getevents)
-   [getLatestEvent](#getlatestevent)
-   [getLatestMessage](#getlatestmessage)
-   [history](#history)
-   [push](#push)
-   [waitForEvent](#waitforevent)

### constructor

Instantiates the dataLayer class

#### Parameters

-   `page` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The page object as provided by puppeteer
-   `containerId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The containerId of the GTM container you want to interact with

### get

Gets the current value of the specified dataLayer variable

#### Parameters

-   `variable` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The variable in the regular [GTM
    dot-notation](https://www.simoahava.com/analytics/variable-guide-google-tag-manager/#5-data-layer-variable)

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)** Promise that resolves to the current value of
the variable

### getContainerIDs

Returns all active container IDs for the page

Returns **[array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)** An array of strings containing all container IDs

### getDataModel

Returns the the full data model, i. e. the current names and values of
all variables in dataLayer

#### Parameters

-   `containerId` (optional, default `this.containerId`)
-   `The` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** GTM Container ID to fetch the data model from.
    Defaults to the `containerId` defined on the instance

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)** A Promise that resolves to the data model of dataLayer
as an object

### getEvents

Returns all dataLayer events matching the given event name

#### Parameters

-   `event` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The event name

Returns **[array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)** An array of event objects

### getLatestEvent

Returns the most recent event matching the specified event name from
dataLayer. Simply returns the most recent event if no eventName is
specified

#### Parameters

-   `eventName` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The event name to match

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)** A Promise that resolves to the latest da
event

### getLatestMessage

Returns the most recent message from dataLayer

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)** A Promise that resolves to the latest dataLayer message

### history

Retrieves all messages that are present in dataLayer

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)** A Promise that resolves to the full dataLayer array

### push

Pushes a message (which can be an event) to the dataLayer

#### Parameters

-   `message` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** The message to push

### waitForEvent

Waits for the specified dataLayer event

#### Parameters

-   `event` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** The event to wait for
-   `options` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Options to supply to the underlying
    waitForFunction, see the [puppeteer documentation for valid
    options](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#pagewaitforfunctionpagefunction-options-args) (optional, default `{}`)

Returns **[promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)** A promise that resolves as soon as the event
happens