# Embeddable IO Widgets

## Environmentally-Enabled <span style="white-space:nowrap">Input-Output Model</span>

<!--
In RStudio
Tools > Install Packages > devtools

OR

install.packages(‘devtools’)          
library(devtools)

Rstudio has devtools intalled already, so maybe just library(devtools) to call in the package
-->

<style>
</style>

<div class="floatright">
<img src="../img/logo/epa.png" style="width:100%; max-width:200px; margin-left:30px">
</div>


### Widget Starter Samples

[Local Industry Evaluator](../../localsite/info/) - Contains multiple widgets - Leaflet and JQuery  
[Local Map - PPE Suppliers](../../localsite/map/#show=suppliers) - Leaflet and JQuery  
[Local Map - Farm Fresh Produce](../../localsite/map/#show=farmfresh) - Leaflet and JQuery  

**Sector List (BEA Commodities/Industries)**  
Developed in React using the [USEEIO-widgets repo](https://github.com/USEPA/useeio-widgets/).  
[Goods & Services - Industry List and Mosaic Heatmap](../build/sector_list.html?view=mosaic&count=50)  
[Goods & Services - Bars for one indicator](../build/sector_list.html#view=mosaic&indicators=WATR&showvalues=true)   
[Goods & Services - Inflow and Outflow - Rubber tire manufacturing example](../build/iotables.html#sectors=326210&page=5)  
[Industry Impact Bars with Configuration](../build/impact_chart_config.html)  
[More Input-Output Widgets](../build/)  


[Impact Bubble Chart](bubble/) - D3 and JQuery  
[Sankey Chart](sankey/) - D3 with Python prep  

View widgets built using React at [https://model.earth/io/build](https://model.earth/io/build)  
The [io/build](https://github.com/modelearth/io/tree/master/build) GitHub folder contains a copy of the [useeio-widgets React nodeJS build](https://github.com/USEPA/useeio-widgets/wiki) which is documented below.  

<!--
If your local widgets reference the "useeio" folder, they may need to be updated occasionally as parameters change. For stability, point your local widgets at one of the [numbered backups](https://model.earth/eeio/build.2020.002) or copy the useeio folder into your project.


([old version](https://model.earth/eeio/build.2020.001), [pre-React](https://model.earth/eeio/build.2020.003) and [new version](useeio)) 
-->


## Widget front-end settings

Using the static output, you can set parameters in the URL or javascript to control the display of the widgets.  

We've copied a static version of the widgets into the "modelearth/io" repo. 
You can use the Github links for embedding.

<!--
from the [GitHub source code](https://github.com/USEPA/useeio-widgets)
-->

## Build the EEIO Widgets (React)

The USEEIO widgets may be built using the steps below, or you may embed a [pre-built static copy](../build).  

<!--After building the widgets, you will need an API key to download the industry sector data JSON files, or you can copy the JSON files from the pre-built static copy. Post an issue to request a key.  -->

---

To build the widgets you'll need a current version of
[Node.js](https://nodejs.org) installed. Make sure that the `node` and `npm`
commands are available in your systems path (you can test this via `node -v` and
`npm -v` on the command line which should give you the respective version of
these tools). 

The first step is to install the build tools and dependencies.
Use <code>cd useeio-widgets</code> if you are working with a direct fork.  

```
cd io
npm install
```


The above will add a node_modules folder.  

You can ignore errors (about 11), including "Error: `gyp` failed with exit code: 1".  

If you receive a "high severity vulnerabilities" warning, run the following as advised:  

	npm audit fix

<!--
pre-React and with React, ignored:
	`gyp` failed with exit code: 1
-->
Then build the widget libraries inside your local useeio-widgets folder:

```
npm run build
```

This should create a `build` folder with a `lib` sub-folder containing small JavaScript libraries used by the USEEIO widgets.  


### Generate Local JSON files

Once built, the `build` folder contains example HTML files that demonstrate the usage of these widgets. 
[View&nbsp;examples](https://model.earth/io/build/)
  
To view these examples locally, you'll need some data that you can download from the Staging instance of the
[USEEIO API](https://github.com/USEPA/USEEIO_API) via the following:

**Sandbox test server (staging)**

```
npm run download -- --endpoint https://smmtool.app.cloud.gov/api
```
This will mirror the static data of the Staging API into the `build/api` folder in two folders: USEEIO and GAUSEEIO.  The second folder contains data for Georgia, the first state using the USEEIO model.  

Sometimes you may need to run a second time to populate build/api/GAUSEEIO/demands table. (Aug 2020)  

You may optionall [request the key](https://github.com/USEPA/USEEIO_API/wiki/Use-the-API) to the production API to run the following:  

```
npm run download -- --endpoint https://api.edap-cluster.com/useeio/api --apikey [Add API key here]
```

After generating build/api folder from the production API:  
1. Duplicate USEEIOv1.2 to USEEIO for existing script in non-React widgets.  
2. Duplicate USEEIOv1.2 to GAUSEEIO since GA data currently only resides on the staging server.  


Note: Every 90 days the staging server requires a reboot, contact Wes to restart.  
The '/api' address always returns 404, so use the <a href="https://smmtool.app.cloud.gov/">enpoint overview</a> to see if it is online.  

You now have two options for viewing the widgets locally.

<b>Option 1:</b> Start a server using the command <code>npm run server</code>. 
Then open the default port (8080) at http://localhost:8080 in your browser to see the widgets.  Your command window will become inoperable since it is running a server.  Open a new command window to issue further commands.  

<b>Option 2:</b> View at the following URL if the useeio-widgets folder resides in your webroot:

[http://localhost:8887/useeio-widgets/build](http://localhost:8887/useeio-widgets/build)  

How to set up [your webroot](../../localsite/start/).  

<!--
Note that the production instance requires an API key.
npm run download -- --endpoint https://path/to/api --apikey an-optional-api-key
<b>Important:</b> Change the folder created in `build/api` folder from `USEEIOv1.2` to `GAUSEEIO`.
-->


## How to Modify Widgets within VS Code

To make updates in the NodeJS source code, fork the [USEEIO-widgets](https://github.com/USEPA/useeio-widgets/) repo and save in your local webroot (where you've [pointed](../../localsite/start/) http://localhost:8887/)  

Edit the files that reside in useeio-widgets/src. (Avoid editing files in useeio-widgets/build, these will be overwritten when you run the build.)

[Configure your VS Code Editor](https://code.visualstudio.com/docs/setup/setup-overview) so running `code .` within the <b>USEEIO-widgets folder</b> launches the editor.  IMPORTANT: Avoid running in the parent folder, or your VS Code editor will not allow you to run subsequent commands inside its terminal.  

Open a command shell window within VS Code (Ctrl + \` backtick) or (View > Terminal) and type the following: 

	npm run build

Use the up-arrow to run the line above again after making a change.  

View the output of your build at [http://localhost:8887/useeio-widgets/build](../../useeio-widgets/build) 

Learn more in the VS Code [Node.js Tutorial](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).  

If you prefer not to build your javascript each time, the [Community repo](https://model.earth/community) is a good place to edit static javascript pages directly.  The [impact map](https://model.earth/community/impact) is a good page to work on by adding map layers and location filters. Additional filters are visible when viewed on localhost. [Getting started steps](../../localsite/start/).


Testing this:  
[LiveReload](https://www.logicbig.com/tutorials/misc/typescript/project-auto-refresh-with-live-reload.html) will refresh your browser as you edit.  Install using the [Extension Marketplace](https://code.visualstudio.com/docs/editor/extension-gallery)  

Get under the hood! Mess with our [Python Samples](../../community/resources/useeio) and 
[add a new technology to the matrix using RStudio](../naics).

<!--
From the following:
https://stackoverflow.com/questions/18428374/commands-not-found-on-zsh

1. Use a good text editor like VS Code and open your .zshrc file (should be in your home directory. 

Command+Shift+H
Command+Shift+Dot

if you don't see it, be sure to right-click in the file folder when opening and choose option to 'show hidden files').

2. find where it states: export PATH=a-bunch-of-paths-separated-by-colons:

3. insert this at the end of the line, before the end-quote: :$HOME/.local/bin

-->





## Sustainable Communities Web Challenge

[Get involved with our 2021 Sustainable Communities Web Challenge](https://model.earth/community/challenge) - $10,000&nbsp;in&nbsp;awards!  
