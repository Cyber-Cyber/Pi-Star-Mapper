# Pi-Star-Mapper
This amateur radio/ham project uses PowerShell and Node Red to map amateur radio operators as they broadcast over your local Pi-Star hotspot.

![GitHub Logo](/media/HeatMap.jpg)

## Prerequisites
1) Node Red - (https://nodered.org/)
2) PowerShell (This is untested outside of Windows 10)
3) Free Opencagedata.com account for API access (https://opencagedata.com/)
4) Pi-Star


## How It Works
1) Queries your Pi-Star homepage to enumerate the callsign of the current/last broadcast.
2) Queries the radioid.net api for name, location, radioid related to the callsign.
3) Using the city,state, & country from radioid.net, we perform a lookup against opencagedata.com for the lat/lon.
4) Query qrz.com for the public photo linked to the callsign.
5) Wrap it all up in a json string and send it to Node Red.
6) Store the json in a hashtable so we don't have to query the internet resources (or consume your API query limit) while the script is running.
7) The World Map node lets you change the map style, add overlays like day/night and heat maps, so play around!
