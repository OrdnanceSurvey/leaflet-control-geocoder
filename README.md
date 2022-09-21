# Leaflet Control Geocoder for OS Names API and OS Places API

This [Leaflet JS](https://leafletjs.com/) plugin is an extension of Per Liedman's [Leaflet Control Geocoder](https://github.com/perliedman/leaflet-control-geocoder/tree/1.13.0#api).

It provides both forward and reverse geocoding (performed on map click location) capabilities using the [OS Names API](https://osdatahub.os.uk/docs/names/overview) and [OS Places API](https://osdatahub.os.uk/docs/places/overview).

## OS Names API

The OS Names API is a RESTful API, which returns queries to the service in either XML or JSON. The service offers two resources: `find` and `nearest`.

| Resource | Description |
| --- | --- |
| `find` | Free string text search, intended to be an ambiguous/fuzzy search. |
| `nearest` | Returns the closest address to a given point by using a pair of OSGB36/British National Grid (BNG) coordinates as the input. |

### Usage

The control is added to the map instance using the `L.Control.Geocoder.osNamesAPI(<String>key)` constructor.

Geocoding uses the `find` resource and will return a maximum of 5 results.

Reverse geocoding uses the `nearest` resource and will return the closest feature within 100m of the given point.

In both instances the coordinates (including bbox) will be returned in OSGB36 (EPSG:27700) eastings and northings. This means they would need to be transformed into WGS84 (EPSG:4326) latitude and longitude using a library like [Proj4JS](http://proj4js.org/).

For reverse geocoding, the map click coordinates will also need to be transformed from latitude and longitude into an OSGB36 easting and northing before being sent to the plugin.

### Customisation

If you wish to provide additional parameters in your requests, the constructor can be modified as follows: `L.Control.Geocoder.osNamesAPI(<String>apiKey, <Object>geocodingQueryParams, <Object>reverseQueryParams)`.

For example, to query the closest Named Road with 500m of the clicked map point when reverse geocoding - the control would be initiated using `L.Control.Geocoder.osNamesAPI(apiKey, null, { radius: 500, fq: 'LOCAL_TYPE:Named_Road' })`.

### Demo

https://labs.os.uk/public/os-api-resources/leaflet-control-geocoder/os-names-api.html

## OS Places API

The OS Places API is a RESTful API, which returns queries to the service in either XML or JSON. The service offers numerous resources - including `find`, `postcode`, `uprn` and `nearest`.

| Resource | Description |
| --- | --- |
| `find` | Free string text search. |
| `postcode` | Search based on a postal code (minimum requirement is the *outward code*, e.g. SO16). |
| `uprn` | Search based on a valid Unique Property Reference Number (URPN). |
| `nearest` | Takes a pair of coordinates (X and Y) as an input to determine the closest address. |

NOTE: Only those resources which are relevant to the control have been listed. The following resources also form part of the service: `bbox`, `radius` and `polygon`.

### Usage

The control is added to the map instance using the `L.Control.Geocoder.osPlacesAPI(<String>key)` constructor.

Geocoding uses the `find` resource [default setting] (but can be configured to use `postcode` or `uprn`) and will return up to 100 results.

Reverse geocoding uses the `nearest` resource and will return the closest feature within 100m of the clicked map point.

Coordinates (input and output) will be WGS84 (EPSG:4326) latitude and longitude.

### Customisation

If you wish to provide additional parameters in your requests, the constructor can be modified as follows: `L.Control.Geocoder.osPlacesAPI(<String>apiKey, <String>resource, <Object>geocodingQueryParams, <Object>reverseQueryParams)`.

For example, to query the `postcode` resource for both DPA and LPI addresses when geocoding - the control would be initiated using `L.Control.Geocoder.osPlacesAPI(apiKey, 'postcode', { dataset: 'DPA,LPI' })`.

### Demo

https://labs.os.uk/public/os-api-resources/leaflet-control-geocoder/os-places-api.html
