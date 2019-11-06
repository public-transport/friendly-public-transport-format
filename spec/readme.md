# friendly public transport format

## references

In order to be able to use this format in both static data *and* API responses, you may choose whether to reference an item by its `id` or inline the item as an object. Take this stop as an example:

```js
{
	type: 'stop',
	id: '1234',
	name: 'Bus Terminal A',
	station: '123' // this references a `station` object
}
```

You may also inline the `station` if that's more convenient:

```js
{
	type: 'stop',
	id: '1234',
	name: 'Bus Terminal A',
	station: { // inline `station` object
		type: 'station',
		id: '123',
		name: 'Copenhagen Central Station'
	}
}
```

## `location` objects

A `location` object is used by other items to indicate their locations.

```js
{
	type: 'location', // required
	name: 'Reichstagsgebäude', // optional
	address: 'Platz der Republik 1, 11011 Berlin', // optional

	longitude: 13.4, // optional
	latitude: 52.5, // optional
	altitude: 130.5 // optional, in meters
}
```

If the `latitude` field is specified in a `location` object, the `longitude` field must also be specified, and vice-versa.

## item types

### `station`

A station is a larger building or area that can be identified by a name. It is usually represented by a single node on a public transport map. Whereas a `stop` usually specifies a location, a `station` often is a broader area that may span across multiple levels or buildings.

```js
{
	type: 'station', // required
	id: '123456', // unique, required
	name: 'Berlin Hauptbahnhof', // official non-abbreviated name, required
	// todo: other names
	location: { // location object, optional
		// … see above
	},
	regions: [ // region ids or region objects, see the section on 'region's, optional
		'1234', '2345'
	]
}
```

### `stop`

A stop is a single small point or structure at which vehicles stop. A stop always belongs to a station. It may for example be a sign, a basic shelter or a railway platform.

If the underlying data source does not allow such a fine-grained distinction, use `station`s instead.

```js
{
	type: 'stop', // required
	id: '12345678', // unique, required
	station: '123456', // station id or station object, required
	name: 'Berlin Hauptbahnhof (tief)', // official non-abbreviated name, required
	// todo: other names
	location: { // location object, optional
		// … see above
	}
}
```

### `region`

A `region` is a group of `station`s, for example a metropolitan area or a geographical or cultural region.

In many urban areas, there are several long-distance train & bus stations, all distinct but well-connected through local public transport. It makes sense to keep them as `station`s, because they may still have individual `stop`s, but clustering them enables more advanced routing information.

A `station` can be part of multiple `region`s.

```js
{
	type: 'region', // required
	id: '1234', // unique, required
	name: 'Bretagne', // official non-abbreviated name, required
	// todo: other names
	stations: [ // station ids or station objects, required
		'123456', '234567'
	]
}
```

### `line`

```js
{
	type: 'line', // required
	id: '123', // unique, required
	name: 'ICE 599', // official non-abbreviated name, required
	mode: 'train', // see section on modes, required
	subMode: …, // reserved for future use
	// todo: color, ...
	routes: [], // array of route ids or route objects
	operator: '123456', // operator id or operator object
}
```

### `route`

A route represents a single set of stations, of a single `line`.

For a very consistent subway service, there may be one route for each direction. Planned detours, trains stopping early and additional directions would have their own route.

```js
{
	type: 'route', // required
	id: '1234', // unique, required
	line: '123', // line id or line object, required,
	mode: 'bus', // see section on modes, overrides `line` mode, e.g. for replacements services
	subMode: …, // reserved for future use
	stops: [ // array of stop/station ids or objects, required
		'12345678',
		'87654321'
	]
}
```

### `trip`

A `trip` represents a single run of a vehicle at a specific time. Passengers taking a trip usually don't enter the vehicle before the trip's first arrival or after its last departure.

```js
{
	type: 'trip', // required
	id: '1234', // unique, required
	line: '1234', // line id or object, optional
	route: '1234', // route id or object, optional
	mode: 'bus', // required if route is an id or if the trip mode differs from the route mode
	subMode: '…', // reserved for future use
	stopovers: [] // list of stopover objects, required, must contain at least two entries
}
```

### `schedule`

*Note:* While FPTF already offers `trip` to describe public transport timetables, a large list of `trip`s tends to contain a lot of duplicate information, especially in urban public transport, with frequent trains and homogenous travels. In those cases, schedules come in handy, describing a set of trips. However, unlike `trip`, they don't allow storing real time data.

```js
{
	type: 'schedule', // required
	id: '12345', // unique, required
	route: '1234', // route id or object, required
	line: '1234', // line id or object, optional
	mode: 'bus', // see section on modes, overrides `route`/`line` mode, e.g. for replacements services
	subMode: …, // reserved for future use
	sequence: [
		// seconds relative to departure at first station/stop
		// in 1-to-1 relation to `route` stops
		{
			arrival: -30 // optional, when the vehicle enters the route
			// The departure at the first stop must be 0.
			departure: 0 // required
		},
		{
			arrival: 50, // optional
			departure: 70 // required
		}
		{
			arrival: 120, // The arrival at the last stop is required.
			departure: 150 // optional, when the vehicle leaves the route
		}
	],
	starts: { // object trip.id -> ISO 8601 string (with timezone), required
		'trip1234': '2019-03-29T20:00:00+01:00', // start time of the trip
		'trip2345': '2019-03-30T20:00:00+01:00',
		'trip3456': '2019-03-31T20:00:00+02:00',
		'trip4567': '2019-04-01T20:00:00+02:00',
	}
}
```

### `operator`

```js
{
	type: 'operator', // required
	id: 'sncf', // unique, required
	name: 'Société nationale des chemins de fer français' // official non-abbreviated name, required
}
```

### `stopover`

A `stopover` represents a vehicle stopping at a stop/station at a specific time.

```js
{
	type: 'stopover', // required

	// - stop/station id or object
	// - required
	stop: '12345-678',

	// - ISO 8601 string (with stop/station timezone)
	// - required if `departure` is `null`
	// - realtime/prognosis data if available, otherwise plan data, otherwise `null`
	arrival: '2017-03-17T15:00:00+02:00',

	// - ISO 8601 string (with stop/station timezone)
	// - plan data if available, otherwise `null`
	plannedArrival: '2017-03-17T15:45:00+02:00',

	// - if realtime/prognosis & plan data is available
	//   - required
	//   - value must be `arrival - plannedArrival` in seconds
	// - otherwise optional
	arrivalDelay: -45,

	arrivalPlatform: '4-1', // string, optional

	// - ISO 8601 string (with stop/station timezone)
	// - required if `arrival` is `null`
	// - realtime/prognosis data if available, otherwise plan data, otherwise `null`
	departure: '2017-03-17T15:02:00+02:00',

	// - ISO 8601 string (with stop/station timezone)
	// - plan data if available, otherwise `null`
	plannedDeparture: '2017-03-17T15:02:00+02:00',

	// - if realtime/prognosis & plan data is available
	//   - required
	//   - value must be `arrival - plannedArrival` in seconds
	// - otherwise optional
	departureDelay: null,

	departurePlatform: null, // string, optional
}
```

### `journey`

A `journey` is a computed set of directions to get from A to B at a specific time. It would typically be the result of a route planning algorithm.

```js
{
	type: 'journey', // required
	id: '12345', // unique, optional
	legs: [], // array of leg objects, required, must contain at least one entry
	price: { // optional
		amount: 19.95, // number, required
		currency: 'EUR' // ISO 4217 code, required
	}
}
```

### `leg`

A `leg` is a section of a `journey`. Legs that don't involve on-demand transport (e.g. bike-sharing or walking) usually correspond to a specific part of a `trip` taken by the user.

```js
{
	type: 'leg', // required
	id: '12345', // unique, optional

	// - station/stop/location id or object
	// - required
	origin: '12345678',

	// station/stop/location id or object
	// - required
	destination: '87654321',

	// - ISO 8601 string (with stop/station timezone)
	// - required if `arrival` is `null`
	// - realtime/prognosis data if available, otherwise plan data, otherwise `null`
	departure: '2017-03-17T15:02:00+02:00',

	// - ISO 8601 string (with stop/station timezone)
	// - plan data if available, otherwise `null`
	plannedDeparture: '2017-03-17T15:02:00+02:00',

	// - if realtime/prognosis & plan data is available
	//   - required
	//   - value must be `arrival - plannedArrival` in seconds
	// - otherwise optional
	departureDelay: null,

	departurePlatform: null, // string, optional

	// - ISO 8601 string (with stop/station timezone)
	// - required if `departure` is `null`
	// - realtime/prognosis data if available, otherwise plan data, otherwise `null`
	arrival: '2017-03-17T15:00:00+02:00',

	// - ISO 8601 string (with stop/station timezone)
	// - plan data if available, otherwise `null`
	plannedArrival: '2017-03-17T15:45:00+02:00',

	// - if realtime/prognosis & plan data is available
	//   - required
	//   - value must be `arrival - plannedArrival` in seconds
	// - otherwise optional
	arrivalDelay: -45,

	arrivalPlatform: '4-1', // string, optional

	// - array of stopover objects
	// - optional
	stopovers: […],

	// - schedule id or object
	// - optional
	schedule: '1234',

	// - see section on modes
	// - overrides `schedule`'s `mode`
	mode: 'train',

	subMode: …, // reserved for future use

	public: true, // is it publicly accessible?

	// - operator id or object
	// - overrides `schedule`'s `operator`
	operator: 'sncf'

	// use this if pricing information is available for specific legs
	price: { // optional
		amount: 12.50, // number, required
		currency: 'EUR' // ISO 4217 code, required
	}
}
```

## modes

As discussed in [#4](https://github.com/public-transport/friendly-public-transport-format/issues/4), we decided to have two fields `mode` and `subMode`.

The following list shows all possible values for a `mode` property. For consumers to be able to use `mode` meaningfully, we will keep this list very short.

- `train`
- `bus`
- `watercraft`
- `taxi`
- `gondola`
- `aircraft`
- `car`
- `bicycle`
- `walking`

In order to convey more details, we will add the `subMode` field in the future. It will differentiate means of transport in a more fine-grained way, in order to enable consumers to provide more context and a better service.
