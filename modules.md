# FPTF JavaScript modules

This is a list of JavaScript modules adhering to the [*Friendly Public Transport Format*](https://github.com/public-transport/friendly-public-transport-format).

- [`validate-fptf`](https://github.com/public-transport/validate-fptf) – Validate *FPTF* data. Follows the spec.

## exposing or containing FPTF data

- [`db-hafas`](https://github.com/derhuerst/db-hafas) – JavaScript client for the DB HAFAS API.
- [`db-prices`](https://github.com/juliuste/db-prices) – Find the cheapest routes using the DB Sparpreise API.
- [`db-stations`](https://github.com/derhuerst/db-stations) – A list of DB stations.
- [`vbb-monitor`](https://github.com/derhuerst/vbb-monitor) – Fetch all departures of all lines at all stations of VBB.
- [`db-monitor`](https://github.com/derhuerst/db-monitor) – Fetch departures at DB stations.
- [`db-monitor-cli`](https://github.com/derhuerst/db-monitor-cli) – Command line tool to fetch departures at DB stations.
- [`db-zugradar-client`](https://github.com/derhuerst/db-zugradar-client) – Get live departures of DB trains.
- [`oebb-hafas`](https://github.com/juliuste/oebb-hafas) – JavaScript client for the ÖBB HAFAS API.
- [`deinbus`](https://github.com/juliuste/deinbus) – JavaScript client for the *deinbus* coach travel API.
- [`ecolines`](https://github.com/derhuerst/ecolines) – JavaScript client for the Ecolines API.
- [`eurolines-de`](https://github.com/juliuste/eurolines-de) – JavaScript client for the eurolines.de API.
- [`generate-db-graph`](https://github.com/derhuerst/generate-db-graph) – Generate a JSON graph of Deutsch Bahn public transport.
- [`hafas-client`](https://github.com/public-transport/hafas-client) – JavaScript client for HAFAS mobile APIs.
- [`interrail`](https://github.com/juliuste/interrail) – Find european train stations and routes. Client for the European Interrail / EuRail API.
- [`locomore`](https://github.com/derhuerst/locomore) – A JavaScript client for the Locomore API.
- [`meinfernbus`](https://github.com/juliuste/meinfernbus) – JavaScript client for the Meinfernbus/FlixBus API.
- [`nettbuss-stations`](https://github.com/derhuerst/nettbuss-stations) – A list of Nettbuss.se stations.
- [`ouibus`](https://github.com/juliuste/ouibus) – JavaScript client for the OUIBUS API.
- [`search-meinfernbus-locations`](https://github.com/derhuerst/search-meinfernbus-locations) – Search for Flixbus MeinFernbus cities & stations.
- [`sncf`](https://github.com/juliuste/sncf) – SNCF API client
- [`tallink`](https://github.com/juliuste/tallink) – JavaScript client for the tallink API.
- [`vbb-client`](https://github.com/derhuerst/vbb-client) – An API client for Berlin & Brandenburg public transport.
- [`vbb-fare-zones`](https://github.com/derhuerst/vbb-fare-zones) – All VBB stations and their fare zones.
- [`vbb-find-stations`](https://github.com/derhuerst/vbb-find-stations) – Search for stations of VBB.
- [`vbb-hafas`](https://github.com/derhuerst/vbb-hafas) – A JavaScript client for Berlin & Brandenburg public transport HAFAS API.
- [`vbb-lines`](https://github.com/derhuerst/vbb-lines) – VBB lines and their stations.
- [`vbb-lines-at`](https://github.com/derhuerst/vbb-lines-at) – Which lines run at a VBB station?
- [`vbb-positions-stream`](https://github.com/derhuerst/vbb-positions-stream) – A realtime stream for positions of buses and trains.
- [`hafas-rest-api`](https://github.com/derhuerst/hafas-rest-api) – Expose a HAFAS client via an HTTP REST API.
- [`vbb-rest`](https://github.com/derhuerst/vbb-rest) – An HTTP REST server for Berlin & Brandenburg public transport.
- [`db-rest`](https://github.com/derhuerst/db-rest) – A clean REST API wrapping around the Deutsche Bahn API.
- [`vbb-stations`](https://github.com/derhuerst/vbb-stations) – A list of VBB stations.
- [`vbb-stations-autocomplete`](https://github.com/derhuerst/vbb-stations-autocomplete) – Search for stations of VBB.
- [`db-stations-autocomplete`](https://github.com/derhuerst/db-stations-autocomplete) – Search for stations of DB.
- [`vbb-trips`](https://github.com/derhuerst/vbb-trips) – When do trains run where in VBB?
- [`wifi-on-ice-portal-client`](https://github.com/derhuerst/wifi-on-ice-portal-client) – Query information from the WifiOnICE portal in German ICE trains.
- [`insa-hafas`](https://github.com/derhuerst/insa-hafas) – JavaScript client for the NASA/INSA HAFAS API.
- [`nahsh-hafas`](https://github.com/juliuste/nahsh-hafas) – JavaScript client for the NAH.SH HAFAS API.
- [`hafas-monitor-departures`](https://github.com/derhuerst/hafas-monitor-departures) – Fetch all departures of all lines at all stations of VBB.
- [`hafas-monitor-departures-ws-server`](https://github.com/derhuerst/hafas-monitor-departures-ws-server) – A WebSocket server wrapping `hafas-monitor-departures`.
- [`hafas-monitor-journeys`](https://github.com/derhuerst/hafas-monitor-journeys) – Use any HAFAS API to monitor journeys from A to B.
- [`pyhafas`](https://github.com/FahrplanDatenGarten/pyhafas/) - python client for the HAFAS API
- [`motis-fptf-client`](https://github.com/motis-project/motis-fptf-client) - JavaScript client to expose the MOTIS API as a fptf API

## consuming FPTF data

- [`generate-db-shop-urls`](https://github.com/derhuerst/generate-db-shop-urls) – Magically generate Deutsche Bahn ticket URLs.
- [`generate-vbb-gtfs`](https://github.com/derhuerst/generate-vbb-gtfs) – Generate clean GTFS from VBB data.
- [`vbb-journey-ui`](https://github.com/derhuerst/vbb-journey-ui) – UI component for displaying a journey like in Google Maps.
- [`hafas-discover-stations`](https://github.com/derhuerst/hafas-discover-stations) – Pass in a HAFAS client, discover stations by querying departures.
- [`discover-vbb-stations`](https://github.com/derhuerst/discover-vbb-stations) – Build a graph of VBB stations by querying departures.
- [`discover-db-stations`](https://github.com/derhuerst/discover-db-stations) – Build a graph of DB stations by querying departures.
- [`hafas-collect-departures-at`](https://github.com/derhuerst/hafas-collect-departures-at) – Utility to collect departures, using any HAFAS client.
- [`hafas-estimate-station-weight`](https://github.com/derhuerst/hafas-estimate-station-weight) – Pass in a HAFAS client, estimate the importance of a station.
- [`are-vbb-hafas-stations-the-same`](https://github.com/derhuerst/are-vbb-hafas-stations-the-same) – Check if two stations from the VBB API should be one.
- [`merge-vbb-stations`](https://github.com/derhuerst/merge-vbb-stations) – Heuristic to find VBB stations & stops that should be one.
