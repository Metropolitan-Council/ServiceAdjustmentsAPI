## DRAFT API specification for Service Adjustments

### TODO:

- [ ] Test on loops (same stop visited twice in the same pattern)
- [ ] Detour modifications: same id, or new id with a reference to previous version? If same id will need a modification date field.

### Known Issues with OpenAPI definition

- Some optional fields may not be set to nullable
- Handling of adjustment end date vs expected end date hasn't been finalized
- Some fields may be required for GTFS-realtime, but optional for the adjusted schedule, or could be in a simplified format (e.g., string instead of [TranslatedString](https://gtfs.org/reference/realtime/v2/#message-translatedstring))
- Possibly others!


### Adjusted Schedule
This endpoint bundles the information required to apply a service adjustment (detour) to Metro Transit's schedule database.

### GTFS-realtime Service Changes APIs
These API endpoints are based on the GTFS-realtime Service Changes v3.1 proposal, or aspects of the spec that have been merged into GTFS-realtime v2.0.

- TripUpdates
- NewShapes: https://github.com/google/transit/pull/272
- NewTrips
- NewRoutes
- NewStops
- ServiceUpdates
