# Pi-Star Mapper
This amateur radio/ham project uses PowerShell and Node Red to map amateur radio operators as they broadcast over your local Pi-Star hotspot.
This has been tested with Pi-Star v4.1.1 and DMR on Brandmeister.

[Node Red World Map]
![GitHub Logo](/media/HeatMap.jpg)

[PowerShell Terminal Logger]
![GitHub Logo](/media/terminalLog.jpg)

## Prerequisites
1) NodeJS - (https://nodejs.org/)
2) Node Red - (https://nodered.org/)
3) node-red-contrib-web-worldmap - (https://flows.nodered.org/node/node-red-contrib-web-worldmap)
4) node-red-dashboard - (https://flows.nodered.org/node/node-red-dashboard)
5) PowerShell (tested on Windows 10)
6) Free Opencagedata.com account for API access (https://opencagedata.com/)
7) Pi-Star - (https://www.pistar.uk/)


## How It Works
1) pistar-mapper.ps1 queries your Pi-Star homepage to enumerate the callsign of the current/last broadcast.
2) pistar-mapper.ps1 queries the radioid.net api for name, location, radioid related to the callsign.
3) Using the city,state, & country from radioid.net, pistar-mapper.ps1 performs a lookup against opencagedata.com for the lat/lon.
4) pistar-mapper.ps1 queries qrz.com for the public photo linked to the callsign.
5) pistar-mapper.ps1 wraps all the data in a json string and send it to Node Red.
6) Store the json in a hashtable so we don't have to query the internet resources (or consume your API query limit) while the script is running. You can monitor this in the PowerShell window title.
7) Open the Node Red World Map and watch the map update as new callsigns come in.
8) The World Map node lets you change the map style, add overlays like day/night and heat maps, so play around!

## Setup
### Configure pistar-mapper.ps1
Open pistar-mapper.ps1 in your favorite text editor and locate the #### Change these variables section. Below you'll be able to modify the $OpenCageAPIKey to match your specific API key.  
$PiStar variable needs to have your Pi-Star's IP address or hostname.  
$photoIcon comes with a base64 encoded image and is used if the callsign doesn't have an image published on qrz.com. You can change this out with another base64 image or change it to a url of an image.  
$nodeRedHttp should be changed to reflect your Node Red's IP address and match the http input node inside of your Node Red flow. The default is localhost, so you should only need to change this if the pistar-mapper.ps1 and Node Red are being ran on different machines.  
$sleepTimer is how often Pi-Star Mapper checks for new callsigns in Pi-Star.  
$ttl tells Node Red's map how long to leave a pin in the map. If you run this for a long time, it can cause your map to be very crowded over time. (Tip: Enabling the heat map layer in the top right of Node Red will give you a long term idea of where stations are located as long as you don't refresh the page).

### Configure Node Red
Copy the contents of nodered-pi-star-mapper.json and then open Node Red. Click on the 'hamburger button' in the top right and select import. Paste the contents from nodered-pi-star-mapper.json into that screen and click OK. You should now see something that looks like this:
![GitHub Logo](/media/nodered.PNG)
If you want to change any of the variables in a node, double click to edit it's properties. Tip: After you run pistar-mapper.ps1, if you click on the debug icon on the right side of Node Red, you should see the update come through as they're published. If you see nothing, check the $nodeRedHttp variable in the pistar-mapper.ps1 file.

## How To Run
Open a PowerShell window and navigate to the directory you saved the pistar-mapper.ps1 file. Simply type '.\pistar-mapper.ps1' followed by the 'enter' key. This should start the script which will display a log of time, callsign, name, talkgroup number and name. Next you'll want to open a web browser and go to http://ip:1880/pistarmapper (where you replace ip with the IP address that you installed Node Red on). After the page loads, the next time a new callsign comes in, it should automatically update the map!

# 73!
