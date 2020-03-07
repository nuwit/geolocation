# Geolocation

I promise this is easier than music visualization. 

Whenever you use the internet, the javascript in your browser technically has access to a whole lot of data about you (if you let it). Though that sounds kinda scary, it can actually be used for a lot cool purposes!

One way your browser can access information is through the Navigator API. An **API** is An application program interface (API) is a set of routines, protocols, and tools for building software applications. In this case, you can use the API to retrieve some information about a user visiting your site. 

Let's take a look at the docs: [https://developer.mozilla.org/en-US/docs/Web/API/Navigator](https://developer.mozilla.org/en-US/docs/Web/API/Navigator)

Your browser can see a lot of stuff—your battery levels, network connections, etc. 

We're going to be focused on location. Geolocation, specifically. [Let's look at the geolocation docs!](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/geolocation)

The navigator API gives us access an object of type `Geolocation`. This is similar to a class instance in Java. 

Let's get some more information on the `Geolocation` object type: [https://developer.mozilla.org/en-US/docs/Web/API/Geolocation](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation)

Within Geolocation, we see three methods: 

`Geolocation.getCurrentPosition()`: 
* Determines the device's current location and gives back a GeolocationPosition object with the data. 
* *What does this mean*: This function sends out a single request for location. Every time you need an updated location, you'll need to run this function again. So you'd maybe use this when pressing a button to get the location. 

`Geolocation.watchPosition()`: 
* Returns a long value representing the newly established callback function to be invoked whenever the device location changes.
* *What does this mean*: This function will WATCH to see if the location changes. If the location changes, it will refetch data about the new location automatically. 

`Geolocation.clearWatch()`: 
* Removes the particular handler previously installed using `watchPosition()`
* *What does this mean*: You may not want to watch a user's position forever. In fact, location tracking can drain battery pretty quickly. This is how you can stop watching a user's location. 

The first two functions accept three parameters. They're a success function, an error function, and a set of options. [Documentation!](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/getCurrentPosition)

In Javascript, yes, functions can be parameters. All things are objects in Javascript. Including functions. 

These parameters will look like something like this:

 ```
 navigator.geolocation.getCurrentPosition(
  function() { // i got it! },
  function() { // something went wrong! },
  { timeout: 100000, maximumAge: null, enableHighAccuracy: false });
 
 ```
As it sounds, the success function is what happens when we successfully receive the user's location. The error function is triggered only when something goes wrong. Timeout is how long the function will wait before it gives up on receiving data. Maximum age is the age of stalest data that the function will accept. 

You don't necessarily need the error function or the options. If that's the case, and the request fails, nothing will happen. 

The successful request will return a [GeolocationPosition](https://developer.mozilla.org/en-US/docs/Web/API/GeolocationPosition).

`Geolocation.coords` will give us latitude and longitude. Technically, we could use latitude and longitude to infer all information about a user's location. Lat and Long (and money) is basically all you need to integrate with the [Google Maps API](https://cloud.google.com/maps-platform/) to create complex geolocation web apps. 

But we're not gonna mess with that API today. Besides, even without it, IF they give us permission, we can access a user's position as easily as:

```
<script>
if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition(function(position) {
    console.log(position);
  }
}       
</script>
```
within a `index.html` file. 

You can view those coordinates by opening up the Javascript Console in Chrome (Ctrl + Shift + J. Mac: Cmd + Opt +J).

Why do we check that `navigator.geolocation` exists? Some devices may not support geolocation. Some users may have blocked all geolocation requests. This will make `navigator.geolocation` equal `undefined`. `undefined` means a value was never set for that property. This is opposed to `null`, which is a calculated choice in value to convey a nonexistent value. 

We need to ensure that `navigator.geolocation` is not undefined before we execute methods on it. Otherwise, we'll get a `TypeError`. 
