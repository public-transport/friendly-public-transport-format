# friendly public transport format

## references

In order to be able to use this format in both static data *and* API responses, you may chose wether to reference an item by its `id` or inline the item as an object. Take this stop as an example:

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

## item types

### `station`

A station is a larger building or area that can be identified by a name. It is usually represented by a single node on a public transport map. Whereas a `stop` usually specifies a location, a `station` often is a broader area that may span across multiple levels or buildings.

```js
{
	type: 'station', // required
	id: '123456', // unique, url-safe, required
	name: 'Berlin Hauptbahnhof', // official non-abbreviated name, required
	// todo: other names
	coordinates: { // optional
		longitude: 52.5250839, // required
		latitude: 13.3672133 // required
	},
	address: 'Europaplatz 1, 10557 Berlin', // optional
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
	id: '12345678', // unique, url-safe, required
	station: '123456', // station id or station object, required
	name: 'Berlin Hauptbahnhof (tief)', // official non-abbreviated name, required
	// todo: other names
	coordinates: { // optional
		longitude: 52.5250839, // required
		latitude: 13.3672133 // required
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
	id: '1234', // unique, url-safe, required
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
	id: '123', // unique, url-safe, required
	name: 'ICE 599', // official non-abbreviated name, required
	mode: 'train', // see section on modes, required
	// todo: color, ...
	routes: [], // array of route ids or route objects
	operator: '123456', // operator id or station object
}
```

### `route`

A route represents a single set of stations, of a single `line`.

For a very consistent subway service, there may be one route for each direction. Planned detours, trains stopping early and additional directions would have their own route.

```js
{
	type: 'route', // required
	id: '1234', // unique, url-safe, required
	line: '123', // line id or line object, required,
	mode: 'bus', // see section on modes, overrides `line` mode, e.g. for replacements services
	stops: [ // array of stop/station ids or objects, required
		'12345678',
		'87654321'
	]
}
```

### `schedule`

*Note:* There are many ways to format schedules of public transport routes. This one tries to balance the amount of data and consumability. It is specifically geared towards urban public transport, with frequent trains and homogenous travels.

```js
{
	type: 'schedule', // required
	id: '12345', // unique, url-safe, required
	route: '1234', // route id or object, required
	mode: 'bus', // see section on modes, overrides `route`/`line` mode, e.g. for replacements services
	sequence: [ // relative to departure at first station/stop, in 1-to-1 relation to `route` stops
		// Note that the departure for the first stop must be 0.
		{
			departure: 0, // required
			arrival: 0
		},
		{
			departure: 130,
			arrival: 120
		}
	],
	starts: [ // array of Unix timestamps, required
		1488379661, // start time of the trip
		1488379761,
		1488379861,
		1488379961
	]
}
```

### `operator`

```js
{
	type: 'operator', // required
	id: 'sncf', // unique, url-safe, required
	name: 'Société nationale des chemins de fer français' // official non-abbreviated name, required
}
```

### `journey`

A `journey` is a computed set of directions to get from A to B at a specific time. It would typically be the result of a route planning algorithm.

```js
{
	type: 'journey', // required
	id: '12345', // unique, url-safe, required
	legs: [ // array of objects, required
		{
			origin: '12345678', // station/stop/location id or object, required
			destination: '87654321', // station/stop/location id or object, required
			departure: '2017-03-16T20:00:00+01:00', // ISO 8601 string (with origin timezone), required
			departureDelay: 120, // seconds relative to departure, optional
			departurePlatform: '4-1', // string
			arrival: '2017-03-17T15:00:00+02:00', // ISO 8601 string (with destination timezone), required
			arrivalDelay: -45, // seconds relative to arrival, optional
			departurePlatform: '9', // string
			schedule: '1234', // schedule id or object
			mode: 'walking', // see section on modes, overrides `schedule` mode
			public: true, // publicly accessible?
			operator: 'sncf' // operator id or station object, overrides `schedule` mode
		}
		// …
	],
	price: { // optional
		amount: 19.95, // number, required
		currency: 'EUR' // ISO 4217 code, required
	}
}
```

The `departureDelay` and `arrivalDelay` fields should only contain a value if realtime data or a prognosis based on realtime data is actually available.

## modes

Work in progess, see [the tracking issue for adding new `mode`s](https://github.com/public-transport/friendly-public-transport-format/issues/4).

- `walking`
- `train` – high-speed, regional, commuter/urban & underground trains
- `bus`
- `ferry` – urban & long-distance water transport
