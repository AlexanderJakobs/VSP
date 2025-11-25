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

| Role           | Name                                                                 | Expectations |
|:---------------|:---------------------------------------------------------|:-------|
| Developer      | Jakobs, Alexander<br>Agboola, Ibrahim<br>Ziegler, Luca<br>Wolpers, Raffael | • documentation matches code<br>• Java<br>• runnable from IDE<br>• English code |
| Project Owner  | Becke, Martin                                                        | • An API connection with an external service (Extern GraphQL, RESTful)<br>and ICC (Intern RPC)<br>• Loadsharing<br>• Support a service orchestration with RPC<br>• measurable goals |

## 2. Constraints
- Data is only relevant for Hamburg, Germany, and dependent <br>on the HVV (Hamburg public transport system)
- API connection to an external service (external GraphQL, RESTful)<br> and the ICC (internal RPC)
- Load sharing is supported
- Service orchestration via RPC is supported
- Geofox Thin interface, HTTP, JSON
- 1 request/second to HVV and nomination

## 3. Context & Scope