# friendly public transport format

## station

```js
{
	type: 'station', // required
	id: '123456', // unique, url-safe, required
	name: 'Berlin Hauptbahnhof', // official non-abbreviated name, required
	// todo: other names
	coordinates: {
		longitude: 52.5250839,
		latitude: 13.3672133
	},
	address: 'Europaplatz 1, 10557 Berlin', // can also be an object
	stops: [] // array of stop ids or stop objects
}
```

## stop

```js
{
	type: 'stop', // required
	id: '12345678', // unique, url-safe, required
	station: '123456', // station id or station object, required
	name: 'Berlin Hauptbahnhof (tief)', // official non-abbreviated name, required
	// todo: other names
	coordinates: {
		longitude: 52.5250839,
		latitude: 13.3672133
	}
}
```

## line

```js
{
	type: 'line', // required
	id: '123', // unique, url-safe, required
	name: 'ICE 599', // official non-abbreviated name, required
	// todo: color, ...
	routes: [] // array of route ids or route objects
}
```

## route

```js
{
	type: 'route', // required
	id: '1234', // unique, url-safe, required
	line: '123', // line id or line object, required
	course: [], // array of station/stop ids or station/stop objects
}
```

## trip

```
{
	type: 'trip', // required
	id: '12345', // unique, url-safe, required
	name: 'string', // title like "train to München Hauptbahnhof"
	route: '1234' // route id or route object
	// todo: time at stops, schedule
}
```