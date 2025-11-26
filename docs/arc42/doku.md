# Dokumentation vom Projekt<br>"Abfahrten in der Nähe"
##### Raffael Wolpers, Ibrahim Agboola, Alexander Jakobs

## 1. Introduction & Goals
- Location is either entered by the user or automatically retrieved from GPS
- 3 stations closest to the starting position
- Stations sorted by distance in ascending order
- The next departure point in each direction is displayed for each station 
- Time is displayed in minutes 
- Distance from each station is displayed in meters (as the crow flies) 
- Application updates after pressing a refresh button 
- Internet connection required 
- Response should be received within 5 seconds;<br>otherwise, an error message "no internet connection" will be displayed 
- Loading indicator (of any type) indicating that the search is in progress,<br> status update for user 

### Quality goals:
| **Quality category**| **Quality** | **Description** |
|:---------|:--------|:---------|
| Functional stability 		| Basic functionality | System will always search and show the 3 closest stations<br> to a user’s starting point  |
| Performance				| Stability			| 15 Minutes during Labs -> must function at least for 15 minutes strait |
| Cultural and Regional		| Multilanguage		|The user interface texts must be translatable into English<br> or German using a translation file that supports the ASCII character set  |

### Transparencies
| **Transparency Type**| **Relevance** |
|:------|:---------|
| Acces | The user must be in the HVV region -> Show the user that the data is just for HVV-region    |
| Error | The user should not notice that there is an error. -> Display time of the last data update 

### Stakeholder

| Role           | Name                                                               | Expectations |
|:---------------|:-------------------------------------------------------------------|:-------|
| Developer      | Jakobs Alexander, Agboola Ibrahim<br>Ziegler Luca, Wolpers Raffael | • documentation matches code<br>• Java<br>• runnable from IDE<br>• English code |
| Project Owner  | Becke, Martin                                                      | • An API connection with an external service (Extern GraphQL, RESTful)<br>and ICC (Intern RPC)<br>• Loadsharing<br>• Support a service orchestration with RPC<br>• measurable goals |

## 2. Constraints
- Data is only relevant for Hamburg, Germany, and dependent <br>on the HVV (Hamburg public transport system)
- API connection to an external service (external GraphQL, RESTful)<br> and the ICC (internal RPC)
- Load sharing is supported
- Service orchestration via RPC is supported
- Geofox Thin interface, HTTP, JSON
- 1 request/second to HVV and nomination

## 3. Context & Scope
### Business view
![Kontextsicht](/docs/arc42/images/Kontextsicht.jpg)

| Element                  | Description                                                                                                                  |
|:-------------------------|:-----------------------------------------------------------------------------------------------------------------------------|
| User                     | Gives his position (Streetname and number)                                                                                   |
| hvv                      | Station data provider                                                                                                        |
| Locationconverter        | An external provider, which handles the positioning for us. <br>It should provide us with coordinates/position for the user. |
| "Abfahrten in der Nähe"  |  Our System                                                                                                                              | 

### Technical view (application)
![Bausteinsicht](/docs/arc42/images/Baustein_v2.0.png)

| Element           | Description                                                                                                                  |
|:------------------|:-----------------------------------------------------------------------------------------------------------------------------|
| hvv               | Gives us actuall data in JSON format                                                                                                     |
| Locationconverter | Receives location from our system and returns coordinated  |
| Geofox API        |  Passes coordinates to hvv System in JSON format                                                                                                                       | 

### Technical view (middleware)
![Middleware architecture](/docs/arc42/images/Middleware.png)
## 4. Solution strategy

| Name                     | Precondition                                | Postcondition                                | Parameter                          | Description                                                      |
|--------------------------|---------------------------------------------|---------------------------------------------|-----------------------------------|-----------------------------------------------------------------|
| `UserPassLocation`       | No location in system                      | Location object                             | String Street, String housenumber | User passes his starting position as street name and number    |
| `getCoordinates`          | Location object has one attribute text     | We get the coordinates X and Y back         | String address                    | Pass address to external system to get corresponding coordinates|
| `addCoordinatesToLocation` | Location object without attribute coordinates| Location object has coordinates as attributes| String X, String Y               | Location receives coordinated attributes                        |
| `getDepartures`          | Location is ready but no departures        | We get JSON with departures from our location| Location                          | Get departure data from data supplier                          |
| `showDepartures`         | Departures have not been shown             | Departures are being shown to a user        | Departure                         | Data of nearest departures is being shown                      |
| `statusUpdate`           | --                                         | Timestamp being shown to user               | --                                | System shows when the last connection to the data supplier was |

### Technical decisions
**Implementation in Java**, because nothing stands against it and the developer team prefer the language

**GitHub**, it is preferred by some team members (more stable than git.haw)

**Kanban**, for a good overview of the progress and for the documentation so it is all in one place. It is recommended by the German Innenministerium (Orgahandbuch - Kanban).

**Geolocation**, [Nominatim](https://nominatim.org/) -> openstreetmap -> 1 free use per second 

### Rollenaufteilung

| Role                       | Name    |
|----------------------------|---------|
| Scrum master          | Raffael |
| Responsible for Nominatim API Integration         | Raffael |
| Responsible for HVV API Integration  | Sasha   |
|Termin-manager (Protokollant)             | Sasha   |
| Testmanager           | Ibrahim |
| Backend Developer             | All     |