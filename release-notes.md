
# Release Notes

## Release Date: 2016-03-16
- **Transit type returned** -  The transit type (e.g. tram, metro, rail, bus, ferry, cable car, gondola, funicular) is now returned with each transit maneuver.
- **Guidance language** -  If the language option is not supplied or is unsupported then the language will be set to the default (en-US). Also, the service will return the language in the trip results.
- **Update multimodal path algorithm** - Applied some fixes to multimodal path algorithm. In particular fixed a bug where the wrong sortcost was added to the adjacency list. Also separated "in-station" transfer costs from transfers between stops.
- **Data producer updates** - Do not combine shortcut edges at gates or toll booths. Fixes avoid toll issues on routes that included shortcut edges.

## Release Date: 2016-03-07

- **Updated all APIs to honor the DNT (Do not track) if set in the header** -  This will avoid logging locations.
- **Reduce 'Merge maneuver' verbal alert instructions** -  Only create a verbal alert instruction for a 'Merge maneuver' if the previous maneuver is > 1.5 km.
- **Updated transit defaults.  Tweaked transit costing logic to obtain better routes.** -  use_rail = 0.6, use_transfers = 0.3, transfer_cost = 15.0 and transfer_penalty = 300.0.  Updated the TransferCostFactor to use the transfer_factor correctly.  TransitionCost for pedestrian costing bumped up from 20.0f to 30.0f when predecessor edge is a transit connection.
- **Initial Guidance Globalization** -  Partial framework for Guidance Globalization. Started reading some guidance phrases from en-US.json file.

## Release Date: 2016-02-22

- **Use bidirectional A* for automobile routes** - Switch to bidirectional A* for all but bus routes and short routes (where origin and destination are less than 10km apart). This improves performance and has less failure cases for longer routes. Some data import adjustments were made (02-19) to fix some issues encountered with arterial and highway hierarchies. Also only use a maximum of 2 passes for bidirecdtional A* to reduce "long time to fail" cases.
- **Added verbal multi-cue guidance** - This combines verbal instructions when 2 successive maneuvers occur in a short amount of time (e.g., Turn right onto MainStreet. Then Turn left onto 1st Avenue).

## Release Date: 2016-02-19

- **Data producer updates** - Reduce stop impact when all edges are links (ramps or turn channels). Update opposing edge logic to reject edges that do no have proper access (forward access == reverse access on opposing edge and vice-versa). Update ReclassifyLinks for cases where a single edge (often a service road) intersects a ramp improperly causing the ramp to reclassified when it should not be. Updated maximum OSM node Id (now exceeds 4000000000). Move lua from conf repository into mjolnir.

## Release Date: 2016-02-01

- **Data producer updates** - Reduce speed on unpaved/rough roads. Add statistics for hgv (truck) restrictions.

## Release Date: 2016-01-26

- **Added capability to disable narrative production** - Added the `narrative` boolean option to allow users to disable narrative production. Locations, shape, length, and time are still returned. The narrative production is enabled by default. The possible values for the `narrative` option are: false and true
- **Added capability to mark a request with an id** - The `id` is returned with the response so a user could match to the corresponding request.
- **Added some logging enhancements, specifically [ANALYTICS] logging** - We want to focus more on what our data is telling us by logging specific stats in Logstash.

## Release Date: 2016-01-18

- **Data producer updates** - Data importer configuration (lua) updates to fix a bug where buses were not allowed on restricted lanes.  Fixed surface issue (change the default surface to be "compacted" for footways).

## Release Date: 2016-01-04

- **Fixed Wrong Costing Options Applied** - Fixed a bug in which a previous requests costing options would be used as defaults for all subsequent requests.

## Release Date: 2015-12-18

- **Fix for bus access** - Data importer configuration (lua) updates to fix a bug where bus lanes were turning off access for other modes.

- **Fix for extra emergency data** - Data importer configuration (lua) updates to fix a bug where we were saving hospitals in the data.

- **Bicycle costing update** - Updated kTCSlight and kTCFavorable so that cycleways are favored by default vs roads.

## Release Date: 2015-12-17

- **Graph Tile Data Structure update** - Updated structures within graph tiles to support transit efforts and truck routing. Removed TransitTrip, changed TransitRoute and TransitStop to indexes (rather than binary search). Added access restrictions (like height and weight restrictions) and the mode which they impact to reduce need to look-up.
- **Data producer updates** - Updated graph tile structures and import processes.

## Release Date: 2015-11-23

- **Fixed Open App for OSRM functionality** - Added OSRM functionality back to Loki to support Open App.

## Release Date: 2015-11-13

- **Improved narrative for unnamed walkway, cycleway, and mountain bike trail** - A generic description will be used for the street name when a walkway, cycleway, or mountain bike trail maneuver is unnamed. For example, a turn right onto a unnamed walkway maneuver will now be: "Turn right onto walkway."
- **Fix costing bug** - Fix a bug introduced in EdgeLabel refactor (impacted time distance matrix only).

## Release Date: 2015-11-3

- **Enhance bi-directional A* logic** - Updates to bidirectional A* algorithm to fix the route completion logic to handle cases where a long "connection" edge could lead to a sub-optimal path. Add hierarchy and shortcut logic so we can test and use bidirectional A* for driving routes. Fix the destination logic to properly handle oneways as the destination edge. Also fix U-turn detection for reverse search when hierarchy transitions occur.
- **Change "Go" to "Head" for some instructions** - Start, exit ferry.
- **Update to roundabout instructions** - Call out roundabouts for edges marked as links (ramps, turn channels).
- **Update bicycle costing** - Fix the road factor (for applying weights based on road classification) and lower turn cost values.

## Data Producer Release Date: 2015-11-2

- **Updated logic to not create shortcut edges on roundabouts** - This fixes some roundabout exit counts.

## Release Date: 2015-10-20

- **Bug Fix for Pedestrian and Bicycle Routes** - Fixed a bug with setting the destination in the bi-directional Astar algorithm. Locations that snapped to a dead-end node would have failed the route and caused a timeout while searching for a valid path. Also fixed the elapsed time computation on the reverse path of bi-directional algorithm.

## Release Date: 2015-10-16

- **Through Location Types** - Improved support for locations with type = "through". Routes now combine paths that meet at each through location to create a single "leg" between locations with type = "break". Paths that continue at a through location will not create a U-turn unless the path enters a "dead-end" region (neighborhood with no outbound access).
- **Update shortcut edge logic** - Now skips long shortcut edges when close to the destination. This can lead to missing the proper connection if the shortcut is too long. Fixes #245 (thor).
- **Per mode service limits** - Update configuration to allow setting different maximum number of locations and distance per mode.
- **Fix shape index for trivial path** - Fix a bug where when building the the trip path for a "trivial" route (includes just one edge) where the shape index exceeded that size of the shape.

## Release Date: 2015-09-28

- ** Elevation Influenced Bicycle Routing** - Enabled elevation influenced bicycle routing. A "use-hills" option was added to the bicycle costing profile that can tune routes to avoid hills based on grade and amount of elevation change.
- **"Loop Edge" Fix** - Fixed a bug with edges that form a loop. Split them into 2 edges during data import.
- **Additional information returned from 'locate' method** - Added information that can be useful when debugging routes and data. Adds information about nodes and edges at a location.
- **Guidance/Narrative Updates** - Added side of street to destination narrative. Updated verbal instructions.
