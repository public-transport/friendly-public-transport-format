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

```
{
	type: 'stop',
	id: '1234',
	name: 'Bus Terminal A',
	station: { // inline `station` object
		type: 'station',
		id: '123',
		name: 'Copenhagen Central Station',
		station: '123' // this references a `station` object
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
	address: 'Europaplatz 1, 10557 Berlin'
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

### `line`

```js
{
	type: 'line', // required
	id: '123', // unique, url-safe, required
	name: 'ICE 599', // official non-abbreviated name, required
	// todo: color, ...
	routes: [] // array of route ids or route objects
}
```

### `route`

```js
{
	type: 'route', // required
	id: '1234', // unique, url-safe, required
	line: '123', // line id or line object, required
	course: [], // array of station/stop ids or station/stop objects
}
```

### `trip`

This markup is optimized for urban public transport, with consistent intervals between two stations.

```js
{
	type: 'trip', // required
	id: '12345', // unique, url-safe, required
	name: 'string', // title like "train to München Hauptbahnhof"
	route: '1234' // route id or route object
	schedule: '12345', // schedule id or schedule object
	sequence: [ // array of objects, required
		{
			time: 0, // time in seconds, required
			stop: '12345678' // station/stop id or station/stop object, required
		},
		{time: 70, stop: '87654321'},
		{time: 110, stop: '87651234'}
	]
}
```

### `schedule`

```js
{
	type: 'trip', // required
	id: '12345', // unique, url-safe, required
	validFrom: '2016-03-01', // ISO 8601 date string, required
	validUntil: '2017-03-01', // ISO 8601 date string, required
	starts: [ // array of Unix timestamps, required
		1488379661, // start time of the trip
		1488379761,
		1488379861,
		1488379961
	]
}
```