# Project 2: The Mars App

## Setup and Deploying 

For this project, I created two separate repositories: one for the frontend, using create-react-app to generate starter code, and one for the backend, using a json-server template to generate the backend code. 

This structure, initially recommended, facilitated the distribution of the backend on Heroku and the frontend on Netlify, once the project was completed. 

## Backend: RESTful API with JSON Server 

**v1:**

Once the JSON Server was installed, I set the following Endpoints: /log, /manifests, /curiosity, /opportunity, /spirit. Subsequently, I proceeded to load the information of the Photo Manifests of the rovers: Curiosity, Opportunity and Spirit, obtained from the NASA API, separating from each Photo Manifest the summary data of the activity of the rover from the detail of the photos by sol (and corresponding earth date). 

At this point it is important to point out that the Perseverance rover was not included in this version because the NASA API page referred to alternative mechanisms for obtaining the photos taken by said rover. However, when I investigated in more detail and went to the API repository, in the README.md they include the Perseverance using the same GET Request structure as the other rovers and provided additional detail on the cameras this rover has. Also, as you may have noticed in the previous screenshots for the particular case of the Curiosity rover, and of course it would apply to the Perseverance, the last available date (max_date) of photos is updated continuously since both rovers are operational on the surface of Mars, which I indicate in advance. 

**v2: Present Version**

This version not only incorporates the Perseverance rover but also directly updates the state variables manifests (summary manifest) and data (detail manifest) of Curiosity and Perseverance with the information obtained through the GET Request Fetch to the NASA API, which allows to have the ranges of dates and details continuously updated. The Curiosity data endpoint is maintained and the Perseverance endpoint is incorporated without data in the db.json for reasons of compatibility with the rest of the components.

## Single Page Application (SPA) and React 

Traditionally the method used when changing something on a web page is to load new pages entirely. Today’s websites, especially Single Page Applications (SPAs), use client-side routing to route to resources that load part of the program or structurally change the entire view of the application when an action is performed, such as clicking a link or detecting changes to an input. 

When we create a project, we will have an index.html file in the public folder. All the code we write in our App component, which acts as the root component, is rendered in this HTML file. Therefore, there is only one HTML file in which our code will be processed. So as a single page app, when you navigate to a new component using React Router, the index.html will be rewritten with the component logic. 

## Components and index.js File 

_**index.js:**_

It is the entry point of our applications in JavaScript. Inside it the BrowserRouter component is wrapped around our App component which allows us to use the Route component and other React Router components anywhere in our app. 

_**App.js:**_

_**v1:**_
App component contains:  

* The state variables that we will be using 

* The GET requests lookup functions to the db.json endpoints, called within the useEffect hook, including their loading into the corresponding state variable 

* The callback function used by the Rover component. Once the date input change event is detected, the state variables: cameraSelected, photos, and isValidDate are reset, the length of the input date is checked, and the input is validated against the relevant state variable that stores the detail manifest of the rover, establishing if in addition to being a valid date, there is a camera that took a photo that day 

* The callback function used by the Camera component. Once the camera selection event is detected, the URL used by the GET Request Fetch is built by incorporating the selected rover, the valid date and the selected camera. The photo state variable is updated with all image URLs 

* The definition of the routes. In total there are 6 Client Side Routes including the nested Route `:roverId` whose value is the name of each rover

_**v2:**_
As mentioned above, this release adds the Perseverance rover and directly updates the Curiosity and Perseverance state variable manifests.

_**NavBar.js:**_

NavBar is a shared component that inserts common content on all pages, such as a navigation menu. The NavBar component has `<NavLink>`, it is used to set the URL and keep track of browsing history. 

_**Home.js:**_

_**v1:**_
Home component renders an image of the planet Mars and displays the following message: "The Opportunity to see Mars, rejoices the Spirit and arouses the Curiosity"

_**v2:**_
The message is changed to the following: "With Perseverance one obtains the Opportunity to see Mars, rejoices the Spirit and arouses Curiosity"

_**About.js:**_ 

_**v1:**_
About component renders an information table of the cameras that the Curiosity, Opportunity and Spirit rovers have, as well as cards with the main characteristics of the rovers missions

_**v2:**_
It incorporates a table with the cameras specifically for the Perseverance rover, as well as reference images of the location of the cameras on the rovers. Also, credit is given that all this information comes from NASA

_**Rovers.js:**_

Rovers component generates a second navigation bar with the names of each rover 

_**Rover.js:**_

_**v1:**_ Rover component uses the useParams hook to obtain the name of the selected rover from the route and thus determine the corresponding manifest summary, and the useLocation hook to detect route location changes and proceed to reset the corresponding state variables 

Subsequently, this component proceeds to render a controlled input field where the user enters the desired date. Once it determines that the date is valid after invoking the callback function, there are cameras that took photos that day, it proceeds to call the Camera component, to render the valid options, and the Photos component for proper processing

_**v2:**_ The explanatory note on the reasons for not displaying camera options is placed prior to the validation of the valid date so that it can be observed on all occasions

_**Camera.js:**_

Camera component displays the valid camera options for the selected rover and day, with the user’s selection the callback function is invoked to build the GET request fetch and its execution that will bring the URLs of the images 

_**Photos.js:**_

Photos component extracts the id and URL of each image and delivers them to the Photo component for display and storage in the db.json 

_**Photo.js:**_

_**v1:**_
Photo component is responsible for making a POST request fetch to the db.json of the id and URL of each image, as well as the display of each image

_**v2:**_
As a mechanism to correct certain erratic behaviors when performing the POST Request fetch and the subsequent update in the state variable `log`, instead of directly passing the new value, we pass a callback function

_**Logger.js:**_

Logger component is responsible for displaying in a table the information of the NASA ID and URL of each image shown to the user 

_**NoMatch.js:**_

NoMatch component displays 404 page if the URL does not exist or the URL might have been changed

## Additional improvements in version 2 of the Mars App

_**index.css:**_

**v1:**
No responsive design using FlexBox

**v2:**
Responsive design using FlexBox

_**_redirect File:_**

**v2:**
Configured a `redirects` file, placed in the public folder, with content: `/* /index.html 200`, required by Netlify to avoid Routes errors

## Links

* The Mars Exploration Rovers v1 Netlify: [Mars App v1](https://serene-palmier-576e25.netlify.app/)

* The Mars Exploration Rovers v2 Netlify: [Mars App v2](https://elegant-cuchufli-1029d0.netlify.app/)