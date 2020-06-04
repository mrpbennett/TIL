# Catching errors when using API data

Never really thought about it before, what if the URL or user inputted something incorrect when it comes to calling a request. What would be the best error to present to the user?

#### For example:

A working MapBox URL

`https://api.mapbox.com/geocoding/v5/mapbox.places/London.json?access_token=XXXX&limit=1`

A Error causing MapBox URL

`https://api.mapbox.com/geocoding/v5/mapbox.places/12What.json?access_token=XXXX&limit=1`

If the 2nd URL was used, our API request would look something like this.

```
"type": "FeatureCollection",
"query": [
"12what"
],
"features": [

] ...
`

The features array is blank with no, as the URL in the request had `12What` as its location. Of course a working URL would present something like this.

```
"type": "FeatureCollection",
"query": [
"london"
],
"features": [
{
    "id": "place.8780954591631530",
    "type": "Feature",
    "place_type": [
    "place"
    ],
    "relevance": 1,
    "properties": {
    "wikidata": "Q84"
    },
```

For the above I would catch the error by checking if the length of `features` is 0. Then print a meaningful message to the console, like below.

```
if (res.body.features.length === 0){
    console.log(chalk.red.bold('unable to locate location data'))
} 
```

This is just one way to catch errors and make them user friendly. I am sure I am barely scratching the surface of error handling also.