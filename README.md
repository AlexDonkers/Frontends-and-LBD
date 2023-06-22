# How to build your own Linked Building Data Frontend tool using IFC.js and Comunica
🚀 Build a web-based linked building data frontend in only 20 steps!

:computer: [This is a live demo of what we're going to build!](https://alexdonkers.github.io/Frontends-and-LBD/MyNewFrontendTool/)

**Alex Donkers**
a.j.a.donkers@tue.nl

**Jeroen Werbrouck**
jeroen.werbrouck@ugent.be

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">The Frontends-and-LBD Tutorial</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="linkedin.com/in/alexdonkers/" property="cc:attributionName" rel="cc:attributionURL">Alex Donkers</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

## Introduction

This handout teaches you how to build a simple IFC.js viewer with SPARQL query capabilities. It is created for the Frontends and LBD lecture in [SSoLDAC2023](https://linkedbuildingdata.net/ldac2023/summerschool/). The goal of this document is to increase the knowledge of frontend creation amongst Linked Building Data enthusiasts and to give researchers a toolset to kick-off their frontend development projects. Special thanks to the creators of IFC.js and Comunica for enabling these developments. Please share your results and post your questions in this GitHub repository.

The **LBDviz** tool was created by following a similar methodology. The methodology and various practical frontend tools are presented in the following work:

:page_facing_up:	_Donkers, A., Yang, D., De Vries, B. & Baken, N. (2023). A Visual Support Tool for Decision-Making over Federated Building Information. CAAD Futures 2023._

More information on interactions with federated AEC models in frontends can be found in a soon to be published paper:

:page_facing_up:	_Werbrouck, J., Verborgh, R., Pauwels, P., Beetz, J. & Mannens E. (unpublished). Facilitating Interactions with AEC Multi Models through Federated Micro Frontend Configurations_

![Image1](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/1.png)

**Resources you can use for this tutorial: OpenFlat**
[TTL](https://raw.githubusercontent.com/AlexDonkers/ofo/main/SWJ_Resources/OpenFlat/OpenFlat_Donkers.ttl) +
[IFC](https://github.com/AlexDonkers/ofo/blob/main/SWJ_Resources/OpenFlat/OpenFlat_Donkers.ifc)

## Tutorial

### Step 1
Install Visual Studio Code
In Visual Studio Code, go to Extensions and install **Live Server**.

![Image2](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/2.PNG)

### Step 2
Create a new folder for your project and open it in Visual Studio Code. 

`File > Open Folder`

### Step 3
Open a new terminal in Visual Studio Code.

`Terminal > New Terminal`

### Step 4
Run **npm init –yes** in your terminal.

`PS C:\Users\...\MyNewProjectFolder> npm init --yes`

A new package.json file will be created.

### Step 5
Create a new file named webpack.config.js, in the MyNewFrontendTool folder. You can do this in the left top of Visual Studio Code.

![Image3](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/3.png)

Paste the code below into this file.
```javascript
const path = require('path');
const NodePolyfillPlugin = require('node-polyfill-webpack-plugin');
module.exports = {
entry: './app.js',
output: {
filename: 'bundle.js',
path: path.resolve(__dirname, './'),
},
plugins: [
new NodePolyfillPlugin()
],
module: {
rules: [
{
test: /\.css$/i,
use: ['style-loader', 'css-loader'], //always put style-loader before css-loader
},
],
},
};
```
Make sure to also install webpack in the terminal:

`PS C:\Users\...\MyNewProjectFolder> npm i webpack`

`PS C:\Users\...\MyNewProjectFolder> npm i webpack-cli`

`PS C:\Users\...\MyNewProjectFolder> npm I node-polyfill-webpack-plugin`

### Step 6
Create a new file called **index.html**.

![Image4](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/4.png)

Paste the following code into the index.html file.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<link rel="icon" href="../../../favicon.ico">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="stylesheet" href="styles.css">
<title>MyNewFrontendTool</title>
</head>
<body>
<!--IFC.JS VIEWER-->
<div id="viewer-container"></div>
<script src="bundle.js"></script>
</body>
</html>
```

The `<div id=”viewer-container”></div>` is where the 3D viewer will be.

### Step 7
Create a new file called **styles.css**. This is where you’ll define the layout of your app, such as fonts, colors, and sizes.

![Image5](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/5.png)

Paste the following code in the styles.css file. Everything within #viewer-container will be added to objects with id=”viewer-container”, such as the one in the index.html file. If you find an object starting with a .asdf instead of a #asdf, everything within this object will be added to objects with class=”asdf”.
```css
*{
margin: 0;
padding: 0;
box-sizing: border-box;
}
html, body {
overflow: hidden;
font-family: Verdana, sans-serif;
}
#viewer-container {
position: fixed;
top: 0;
left: 0;
outline: none;
width: 100%;
height: 100%;
z-index: -1;
}
```

### Step 8
Create a new file called app.js. Your folder should now look like this:

![Image6](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/6.png)

Paste the following code in app.js:
```javascript
import { Color } from 'three';
import { IfcViewerAPI } from 'web-ifc-viewer';

///////////////////////////////////////////////////////////////////////
// CREATE THE VIEWER

const container = document.getElementById('viewer-container');
const viewerColor = new Color('#E2F0D9');
const viewer = new IfcViewerAPI({ container, backgroundColor: viewerColor });
viewer.grid.setGrid();
viewer.axes.setAxes();
```

Make sure to also install three and the web-ifc-viewer in the terminal:

`PS C:\Users\...\MyNewProjectFolder> npm i three@0.135`

`PS C:\Users\...\MyNewProjectFolder> npm i three@0.135 web-ifc-viewer`

### Step 9
In the package.json file, change the command in scripts to “build”: “webpack”.

Old:
```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```
New:
```json
"scripts": {
    "build": "webpack"
  },
```

### Step 10
Let’s have a look at our current app. In the Terminal in Visual Studio Code, run the following command. This will create the **bundle.js** file.

`PS C:\Users\...\MyNewProjectFolder> npm run build`

In the right bottom of Visual Studio Code, click **Go Live**. You should see something like this:

![Image7](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/7.png)

:partying_face:	Well done, you created your basic viewer component. 

### Step 11
Let’s add a button to load IFC files in our viewer. First, go to index.html and create a button. The body of your index.html should look like this:
```html
<body>
    <!--LOAD IFC BUTTON-->
    <input type="file" id="load-ifc-button"></input> 

    <!--IFC.JS VIEWER-->
    <div id="viewer-container"></div>
    <script src="bundle.js"></script>
</body>
```
In styles.css, add the following at the end of the file. This is where you can later design the button, e.g., change its shape and color. 
```css
#load-ifc-button {
    position: absolute;
    background-color: #ff0072;
    float: left;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 6px 10px;
    font-size: 12px;
    color: white;
}
```
You should now see a pink button in your application.

### Step 12
We now add the file input functionality to app.js. Paste this at the end of app.js.
```javascript	
///////////////////////////////////////////////////////////////////////
// CREATE THE LOAD IFC BUTTON

const inputButton = document.getElementById("load-ifc-button");
inputButton.addEventListener("change", async () => {
    const ifcFile = inputButton.files[0];
    const ifcURL = URL.createObjectURL(ifcFile);
    const model = await viewer.IFC.loadIfcUrl(ifcURL);
    // Create shadows
    await viewer.shadowDropper.renderShadow(model.modelID);
    viewer.context.renderer.postProduction.active = true;
  }
);
```
The app.js file should look like this:
```javascript
import { Color } from 'three';
import { IfcViewerAPI } from 'web-ifc-viewer';

///////////////////////////////////////////////////////////////////////
// CREATE THE VIEWER

const container = document.getElementById('viewer-container');
const viewerColor = new Color('#E2F0D9');
const viewer = new IfcViewerAPI({ container, backgroundColor: viewerColor });
viewer.grid.setGrid();
viewer.axes.setAxes();

///////////////////////////////////////////////////////////////////////
// CREATE THE LOAD IFC BUTTON

const inputButton = document.getElementById("load-ifc-button");
inputButton.addEventListener("change", async () => {
    const ifcFile = inputButton.files[0];
    const ifcURL = URL.createObjectURL(ifcFile);
    const model = await viewer.IFC.loadIfcUrl(ifcURL);
    // Create shadows
    await viewer.shadowDropper.renderShadow(model.modelID);
    viewer.context.renderer.postProduction.active = true;
  }
);
```

### Step 13
In Visual Studio Code, go to the node_modules folder. Then open the web-ifc folder and copy the web-ifc-mt.wasm and web-ifc.wasm files. Paste them in your main project folder. This folder now looks like this:

![Image8](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/8.png)

### Step 14
In the Terminal in Visual Studio Code, run the following command. This will update the bundle.js file.

`PS C:\Users\...\MyNewProjectFolder> npm run build`

Go to your browser to check out the app. You can click on the pink button and select an [IFC](https://github.com/AlexDonkers/ofo/blob/main/SWJ_Resources/OpenFlat/OpenFlat_Donkers.ifc) file. 

🥳 Well done! You have just created your first web-based BIM viewer.

![Image9](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/9.png)

### Step 15
Let’s make some interactive elements. Add the following to the end of app.js.
```javascript
///////////////////////////////////////////////////////////////////////
// CREATE INTERACTION WITH THE MODEL

window.onmousemove = async () => await viewer.IFC.selector.prePickIfcItem();

window.onclick = async () => await viewer.IFC.selector.pickIfcItem();

viewer.clipper.active=true;

window.onkeydown = (event) => {
    if(event.code === 'KeyP') {
        viewer.clipper.createPlane();
    }
    else if(event.code === 'KeyO') {
        viewer.clipper.deletePlane();
    }
};
```
Run `npm run build` again in the Terminal. You can now go to the tool again and import your IFC file. You can now hover over elements and click on them. You can also create 	planes to crop your model (using P) and delete those planes (using O).

![Image10](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/10.png)

### Step 16
We’re now going to bring in linked data!
First, let’s create a text area so that we can create SPARQL queries. 
Paste the following at the end of styles.css for basic layout of the query box:
```css
#query-box {
    background-color: green;
    border: none;
    width: 500px;
    outline: none;
    padding: 6px 10px;
    font-size: 12px;
    color: white;
    position: relative;
    z-index: 1;
    margin-top: 40px;
}
```
In index.html, paste the following code right after the load-ifc-button div:
```html
    <!--QUERY BOX-->
    <div id="query-box">
        <textarea id="SPARQL-input" rows="12" cols="63">
PREFIX bot: <https://w3id.org/bot#>
SELECT * WHERE {
    ?s ?p bot:Building.
    ?s ?p ?o
} 
LIMIT 100
        </textarea>        
        <textarea id="GRAPH-input" rows="2" cols="63"></textarea>
        <input type="button" class="SPARQL-submit" id="SPARQL-submit" value="Run query!" onclick="queryComunica()">
    </div>
```
As you can see, the text box is pre-filled with a query. 

### Step 17
Let’s create a results box, so that we can plot the results of our query. First, in styles.css:
```css
#results-box {
    background-color: green;
    border: none;
    width: 500px;
    outline: none;
    padding: 6px 10px;
    font-size: 12px;
    color: white;
    position: relative;
    z-index: 1;
    margin-top: 10px;
}
```
And the following in index.html, right after the query box:
```html
<!--RESULTS BOX-->
    <div id="results-box">
        <p><span id="results-box-content"></span></p>
    </div>
```

This should be your current index.html:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="icon" href="../../../favicon.ico">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>MyNewFrontendTool</title>
</head>
<body>
    <!--LOAD IFC BUTTON-->
    <input type="file" id="load-ifc-button"></input></body>
    
    <!--QUERY BOX-->
    <div id="query-box">
        <textarea id="SPARQL-input" rows="12" cols="63">
PREFIX bot: <https://w3id.org/bot#>
SELECT * WHERE {
    ?s ?p bot:Building.
    ?s ?p ?o
} 
LIMIT 100
        </textarea>
        <textarea id="GRAPH-input" rows="2" cols="63"></textarea>
        <input type="button" class="SPARQL-submit" id="SPARQL-submit" value="Run query!" onclick="queryComunica()">
    </div>

    <!--RESULTS BOX-->
    <div id="results-box">
        <p><span id="results-box-content"></span></p>
    </div>

    <!--IFC.JS VIEWER-->
    <div id="viewer-container"></div>
    <script src="bundle.js"></script>
</body>
</html>
```

### Step 18
We’re finally adding Query capabilities. First, install comunica:

`PS C:\Users\...\MyNewProjectFolder> npm i @comunica/query-sparql`

Add the following code to the end of app.js.
```javascript
///////////////////////////////////////////////////////////////////////
// COMUNICA

import { QueryEngine } from '@comunica/query-sparql'

export async function queryComunica() {
  const myEngine = new QueryEngine();
  const query = document.getElementById("SPARQL-input").value
  const graphs = document.getElementById("GRAPH-input").value.split(',')
  const bindingsStream = await myEngine.queryBindings(query, {
    sources: graphs,
  });
  const bindings = await bindingsStream.toArray();
  console.log("Results"+bindings)

  // Plot in results box

}
window.queryComunica = queryComunica;
```

Now run `npm run build` again in the terminal. You can now run SPARQL queries in your frontend tool. First, paste a link to an RDF file in the GRAPH-input box. Here’s an example link: 
https://raw.githubusercontent.com/AlexDonkers/ofo/main/SWJ_Resources/OpenFlat/OpenFlat_Donkers.ttl

Then, click on **Run query!** You should now see results of the query in the console.

![Image11](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/11.png)

### Step 19
Add code to plot these results in the results box. We do this in app.js, right after the //Plot in results box line.
```javascript
  // Plot in results box
  //clear result box
  document.getElementById("results-box-content").innerHTML = "";
  //start table component
  let tableContent = "<table>" 

  tableContent +="<tr>"
  const headers = bindings[0].entries._root.entries
  for (var y = 0; y<headers.length; y++) {
    tableContent += "<th>"+headers[y][0]+"</th>"
  }
  tableContent +="</tr>"

  for (var i = 0; i<bindings.length; i++) {
    const binding1 = bindings[i]
    const results = binding1.entries._root.entries

    //create table rows
    tableContent += "<tr>"
    for (var x = 0; x<results.length; x++) {
      if (results[x][1].termType === "Literal") {
        console.log(results[x][0] +": "+ results[x][1].value)
        
        tableContent += "<td>"+results[x][1].value+"</td>"
      }
      if (results[x][1].termType !== "Literal") {
        console.log(results[x][0] +": "+ results[x][1].value.split("#", 2)[1])
        tableContent += "<td><a target='_blank' href='"+results[x][1].value+"'>"+results[x][1].value.split("#", 2)[1]+"</a></td>"
      }
    }
    tableContent += "</tr>"
  } 
  //end
  tableContent += "</table>" 

  //add table to results box
  document.getElementById("results-box-content").innerHTML = tableContent
```
After running `npm run build` again, you can now find query results in the results box.



### Step 20
Finally, we’ll create some interaction between SPARQL and the IFC model. 
First, in **index.html**, replace the results box section by the following code:
```html
    <!--RESULTS BOX-->
    <div id="results-box">
        <p>Selected guid: <a id="selected-guid"></a></p>      
        <p><span id="results-box-content"></span></p>
    </div>
```
Then, in app.js, replace the window.onclick function in line 33 by the following code. This will fill in the GlobalId of the selected IFC element in the results box.

```javascript
window.onclick = async () => {
    const result = await viewer.IFC.selector.pickIfcItem(true);
    if(!result) return;
    const {modelID, id} = result;
    const props = await viewer.IFC.getProperties(modelID, id, true, false);
    console.log(props.GlobalId.value);
    document.getElementById("selected-guid").innerHTML = props.GlobalId.value;
    return props.GlobalId
};
```
 
Then finally, change the queryComunica function in app.js to the following:

```javascript
///////////////////////////////////////////////////////////////////////
// COMUNICA

import { QueryEngine } from '@comunica/query-sparql'

export async function queryComunica() {
  const myEngine = new QueryEngine();
  console.log(document.getElementById("selected-guid").innerHTML);
  const graphs = document.getElementById("GRAPH-input").value.split(',')
  const bindingsStream = await myEngine.queryBindings(`PREFIX bpt:    <https://w3id.org/bpt#>               
  select * where { 
    ?element bpt:hasGlobalIdIfcRoot `+JSON.stringify(document.getElementById("selected-guid").innerHTML)+` .
    ?element ?p ?o .
  } limit 100 `, {
    sources: graphs,
  });
```
Now run **npm run build**. Use the OpenFlat resources, indicated in the beginning of the file. Insert the [IFC](https://github.com/AlexDonkers/ofo/blob/main/SWJ_Resources/OpenFlat/OpenFlat_Donkers.ifc) file, then insert the link to the [TTL](https://raw.githubusercontent.com/AlexDonkers/ofo/main/SWJ_Resources/OpenFlat/OpenFlat_Donkers.ttl) file in the GRAPH-input box. Click at one of the elements in the viewer and click on **Run query!** (you might need to wait a second or so). Instead of using the SPARQL-input box, Comunica now queries nodes around the node of the clicked element, by inserting the guid of the element into the query!

![Image12](https://github.com/AlexDonkers/Frontends-and-LBD/blob/main/HandoutImages/12.png)

## Well done! 🥳
I hope you enjoyed this tutorial. For questions, please use the [Issues](https://github.com/AlexDonkers/Frontends-and-LBD/issues) section or reach out: a.j.a.donkers@tue.nl.
Follow me on [LinkedIn](https://www.linkedin.com/in/alexdonkers/) for regular research updates.

