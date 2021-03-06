# Get a shipping estimate with Convelio


## Mandatory objects description and examples

In order to give a shipping price to your visitors we need you to fulfill the following requirements.

> This is the minimum mandatory information needed for the widget to work properly. It can be set up in a global window `CVOQWSettings` object **before** loading the widget script, or **later on** by using the `CVOQW.settings` method.  

#### Company name

We need the name of the company where the pickup occurs to create a quote. You'll be able to enter it in a parameter called `companyName` in the global window `CVOQWSettings` object.

#### <a name="item"></a> Item Object

We need an item object describing the goods to be shipped :

```
{
  length: 25.1, // number
  height: 20.2, // number
  width: 5, // number
  weight: 1, // number, optional
  quantity: 1, // number, optional
  value: {
    amount: 65000, // number
    currency_code: "EUR" // string
  },
  name: "The Great Wave off Kanagawa", // string
  description: "woodblock print", // string, optional
  measurement_system: "metric", // string, optional
  packing_type: "not_packed" // string, optional
}
```

> - the `amount` is a unitary value, use a dot to add decimal values. In the previous example the item value is 65000€.
> - `currency_code` can be 'EUR', 'USD' or 'GBP', default is 'EUR'.
> - `quantity` is optional and set to 1 by default, but can be explicitly indicated in the case of a batch of several identical objects (example: a pair of chairs).
> - `measurement_system` can be 'metric' or 'us', default is 'metric'.
> - `packing_type` can be 'not_packed' or 'crated', default is 'not_packed'.

#### <a name="address"></a> Address Object

We also need an address object to specify the location at which the goods will be picked up :

```
{
  street: "68 avenue Parmentier",
  city: "Paris",
  postcode: "75011",
  state: "Ile-de-France",
  country: "France",
  country_code: "FR"
}
```

> - Try to fill in your address as accurately as possible. For instance `street` should be composed of a street number and a street name.
> - `country_code` is your country [ISO2 code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)

#### <a name="item"></a> Your public API key

Don't forget to also input your public API key (the one we gave you) while setting up the widget (either by using the global `CVOQWSettings` object, or by using the `CVOQW.settings` method as described below).

## How to install

You can follow one of these methods, to enable visitors to your platform can get instant shipping prices. The one you follow will depend on your web app configuration to install the Convelio widget on your website.

## Basic Javascript...

If you have a web app with multiple pages where each one triggers a new page refresh then you will only need to follow one step. 

To get the Convelio widget to appear on your web app simply copy/paste the following code snippet before the HTML `</body>` tag on every page where you want the widget to appear for your visitors (most likely on your product pages) :

```
<script>
  var current_item = {
    length: number,
    height: number,
    width: number,
    weight: number, // optional
    quantity: number, // optional
    value: {
      amount: number,
      currency_code: string, // can be 'EUR', 'USD' or 'GBP'
    },
    name: string,
    description: string, // optional
    measurement_system: string, // optional, can be 'metric' or 'us'
    packing_type: string, // optional, can be 'not_packed' or 'crated'
  };
  var your_address = {
    street: string,
    city: string,
    postcode: string,
    state: string,
    country: string,
    country_code: string
  };
  window.CVOQWSettings = {
    publicApiKey: YOUR_PUBLIC_API_KEY,
    companyName: YOUR_COMPANY_NAME, // string
    item: current_item,
    pickupAddress: your_address,
    currency: string // optional, can be 'EUR', 'USD' or 'GBP', default is 'EUR'
  };
  
  (function(){var script=document.createElement("script");script.async=true;script.src="https://storage.googleapis.com/widget-convelio-com/cvoqw.js";var entry=document.getElementsByTagName("script")[0];entry.parentNode.insertBefore(script,entry)})();
</script>
```

> Note : see the [item object](#item) and [address object](#address) descriptions
> 
> The `currency` setting allows you to change the desired currency for the shipping prices displayed by the widget.

Make sure to correctly fulfill the `item`, `pickupAddress`, `companyName` and `publicApiKey` information in the global window `CVOQWSettings` object.


## ... or Single Page App

If you have a single page app then you can get the Convelio widget installed on your app in two simple steps.

### Step 1: Include the JS

First, include this Javascript snippet before the HTML `</body>` tag :

```
<script>
  (function(){var script=document.createElement("script");script.async=true;script.src="https://storage.googleapis.com/widget-convelio-com/cvoqw.js";var entry=document.getElementsByTagName("script")[0];entry.parentNode.insertBefore(script,entry)})();
</script>
```

It will asynchronously load the Convelio widget script file into your web application.

### Step 2: Set up the widget settings

Then, set up the Convelio widget settings on each of your product page where you want the widget to appear :

```
var current_item = {
  length: number,
  height: number,
  width: number,
  weight: number, // optional
  quantity: number, // optional
  value: {
    amount: number,
    currency_code: string, // can be 'EUR', 'USD' or 'GBP'
  },
  name: string,
  description: string, // optional
  measurement_system: string, // optional, can be 'metric' or 'us'
  packing_type: string // optional, can be 'not_packed' or 'crated'
};
var your_address = {
  street: string,
  city: string,
  postcode: string,
  state: string,
  country: string,
  country_code: string
};
window.CVOQW.settings({
  publicApiKey: YOUR_PUBLIC_API_KEY,
  companyName: YOUR_COMPANY_NAME, // string
  item: current_item,
  pickupAddress: your_address,
  currency: string // optional, can be 'EUR', 'USD' or 'GBP', default is 'EUR'
});
```

As the widget is asynchronously loaded, the `CVOQW.settings` method must be called only once the widget has been properly and fully loaded.

> The `currency` setting allows you to change the desired currency for the shipping prices displayed by the widget.

### <a name="global"></a>Global settings

The `publicApiKey`, `companyName`, `pickupAddress` and `currency` settings may be declared only once when your app initializes or for instance in the initial CVOQW loading script : **step 1** would then becomes

```
<script>
  var your_address = {
    street: string,
    city: string,
    postcode: string,
    state: string,
    country: string,
    country_code: string
  };
  window.CVOQWSettings = {
    publicApiKey: YOUR_PUBLIC_API_KEY,
    companyName: YOUR_COMPANY_NAME, // string
    pickupAddress: your_address,
    currency: string // optional, can be 'EUR', 'USD' or 'GBP', default is 'EUR'
  };
  (function(){var script=document.createElement("script");script.async=true;script.src="https://storage.googleapis.com/widget-convelio-com/cvoqw.js";var entry=document.getElementsByTagName("script")[0];entry.parentNode.insertBefore(script,entry)})();
</script>
```

### Updating the item information

If your pickup address is the same for all your goods and you decided to set it up as a CVOQW global setting (see previous point [Global Settings](#global)), you can then simply update the item information when navigating on a new product page by using the following method :

```
window.CVOQW.updateItem({
  length: number,
  height: number,
  width: number,
  weight: number, // optional
  quantity: number, // optional
  value: {
    amount: number,
    currency_code: string
  },
  name: string,
  description: item_description, // string, optional
  measurement_system: item_measurement_system, // string, optional
  packing_type: item_packing_condition // string, optional
});
```

Keep in mind that the dimensions (`length`, `height`, `width`), amount (`value.amount`) and `name` properties are mandatory !

If you don't update the `currency_code` property the price will be calculated using the default currency value (EUR).

The same behavior applies to the `measurement_system` and `packing_type` properties.


### Changing the currency

You can use the `changeCurrency` method if you want to dynamically update the currency of the shipping prices displayed by the widget.

It uses a string for argument which can be 'EUR', 'USD' or 'GBP';

```
window.CVOQW.changeCurrency('USD'); // will change the currency to $
```


# Open the widget

Once you're all set up, you'll want to give the availability to your visitors to open and use the Convelio widget.

Simply add a button in your HTML and bind the following method to its `onclick` event : `window.CVOQW.open()`

ex.:
`<button type="button" onclick="CVOQW.open()">Get a shipping estimate with Convelio</button>`

# Close the widget

In case you need to close the widget remotely you can simply use the following method : `window.CVOQW.close()`

This can happen if you're embedding the widget in a SPA and you want the widget to close on a new navigation event (if the user is leaving a product page for example).

# Customization

You can add a `styles` parameter to the `CVOQWSettings` global object in order to customize the widget.

For now the only available rule is `position`, which will display the widget on the left or right side of the screen.

It should look like this :

```
window.CVOQWSettings = {
  ...,
  styles: {
    position: string // 'left' or 'right'
  }
};
```

> We're planning to add more customizable parameters in the future to offer you a better embedding experience (like color customization for example).
