Before we jump into the Mozilla Web of Things Gateway (GW), let’s get a clear overview of the architecture. This way, we can show how the different parts of GW fit into the big picture.

This section is a work in progress so may change often. You can also get involved and learn more on the [Mozilla Discourse portal](https://discourse.mozilla.org/c/iot).

The system has 2 main parts, the backend server side, and the front end client side.
* Backend server based on NodeJS and using express framework for routing.
* Front end single page web application where routing is controlled via the lightweight page.js library.

Architecture Diagram 1:

![Mozilla WoT Gateway Architecture](https://github.com/mozilla-iot/wiki/blob/master/wiki/images/wot_gw.png)

*Proper image link is currently not working. As interim, you can see it here:*  
![Mozilla WoT Gateway Architecture](https://raw.githubusercontent.com/mozilla-iot/wiki/c104bfe39323e975fd9c6a79dcc84ca34d83b576/images/wot_gw.png) 


### Server Side Components

***

#### Section 1

The Node.js server (See section 1 in diagram) is started in /src/app.js. This will either start the server in http or https mode depending on how the system is configured.
Please note starting the server is not done with the usual:

	/> nodejs app.js
	
WebPack compiles the app.js file into the /build/gateway.js file. In shell terms, this looks like `webpack && node build/gateway.js`. We bundle this process into the start script, meaning that the server can be started with only:

	/> npm start

The app.js is responsible for starting the server and binding the express middleware framework to the server. 
This start point controls how tunnels are created and closed in the application.

#### Section 2

The application is configured to use sqlite for simplicity. It is expected that other DB's will be used for production grade systems.
The DB setup is done from /src/db.js. This contains setup and migration functions for the DB.

#### MVC 

The GW is loosely based on the Model View Controller paradigm where /src/model, /src/controllers and /srv/view contain the relevant MVC's for the system.
It is important to note there is only one main 'view' which is the /src/views/index.html file. 
Once this file is served to the client - it is the client which controls most content from this point on. There is no direct correlation between a route end point in the controller files and the hyperlinks in the index.html file. 

e.g. The settings menu is controlled by the /src/controllers/settings_controller.js and a routes are defined here. However hyper links calling those function are not contained within rendered pages. This is explained later in client side.

### Client Side

***

#### Section 3


From the client side home page, the majority of calls from index.html are interpreted and routed from /static/js/router.js. This uses the [page.js library](https://github.com/visionmedia/page.js) to control routing.
The router then hands off control to the relevant functions to deal with that route. e.g. the /settings route is dealt with the App.showSettings. This uses the SettingsScreen module in /static/js/settings.js to then route calls to the server.
You can see this in fetch calls to '/settings/tunnelinfo' in settings.js.


#### Section 4

The client side will drive changes in the DOM for the index.html file by hiding or showing different menu options. This means that the application is based around the single web page model where pages are dynamically manipulated by the client side.
The server is used to supply data that populates the client. You can find out more here about [SPA'S](https://en.wikipedia.org/wiki/Single-page_application)
