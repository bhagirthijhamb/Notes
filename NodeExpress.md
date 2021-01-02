# Overall architecture of app. 
- How all of the components and technologies are going to communicate
- When a user navigates in the browser to our domain say Emaily.com,  we are gonna send them back a html document (index.html) and some JS files that contain a React application. These files will make some content appear on the screen. This is going to be the React side of our application
- The React side of our application does not know what to show to the user. It needs some data to show like a list of surveys/campaigns. We will use MongoDB to record and store all the different surveys and emails that we will send to people over time.
- Express API will have a bunch of business logic and will be in between React and MOngoDB.
It will take incoming request form React app, pull info form MongoDB  and send to back to React

- So the user visits Emaily.com website in the browser -> We send them the React app -> React aspplication boots up -> asks the Express API for some data -> Express API pulls some info from MongoDB -> sends it to the React app -> React app show that on the screen to the user

- Express API and React are going to interact entirely through http requests or ajax requests. Every single request is going to have some amount of json. Express APi and MongoDb is gonna interact with some internal system (mongoose)

- Node - JS runtime used to execute code outside of the browser
- Express - library that runs in the Node runtime. A little collection of functions/ helpers for making working with the http aspects of the node JS easier.

When we are running some server on our local machine. our server is going to listen to http traffic on a single individual port. Port is like a door through which http requests can be routed.(Ports on our machine 4998, 4999, 5000, 5001, 5002, 5003)
We might have a http request from a browser also running on our local machine.
We will configure Node and Express to listen to traffic that is attempting to access a very specific port on our local machine.
NodeJs will listen for http request on that port, take the information that flows in from the http request and hand it off to the Express side on our application.
Express is going to look at the req and decide what logic inside the express application we are building is goign to handle/ respond to this request.
In express we write collection of route handlers used to handle http requests that are asking for a specific service. One for authenticating user, other for logging out user, create surveys/campains etc.
The route handlers process the incoming request and generate the outgoing response.
This response is send back to running node process and node will then respond to the request with the res.

# Heroku Deployment Checklist
- Dynamic Port Binding
  Heroku tells us which port our app will use, so we need to make sure we listen to the port they tell us to
- Specify Node Environment
  We want to use a specific version of node, so we need to tell Heroku which version we want
- Specify start script
  Instruct Heroku what commnd to run to start our server running
- Create .gitignore file
  We don't want to include dependencies, Heroku will do that for us

# Steps - First Time Deploy
- Create Heroku account
- Commit our codebase to git
- Install Heroku CLI and Create App
- Deploy App with Git
- Heroku delpoys project

# Subsequent Deploys
- Commit codebase with git
- Deploy App with Git

# Environment variables
Whenever Heroku runs our application, it has the ability to inject environment variables. Environment variables are the variables that are set in the underlying runtime environment that Node is running on top of.
This is the Heroku's oppotunity to pass us runtime configuration, some configuration that Heroku only wants to tell us after we have actually deployed our application.

# User App Flow
- User signs up via Google OAuth (we start from server side building)
- User pays for email credits via stripe
- User creates a new 'campaign'
- User enters list of emails to send survey to
- We send email to list of surveyees
- Surveyees click on link in email to provide feedback
- We tabulate feedback
- User can see report of all survey responses

# Passport Library components
Passport is authentication middleware for Node.
- passport - General helpers gor handling auth in Express apps
- passport strategy - Helpers for authenticating with one very specific method (email/password, Google, Facebook, etc.) (module that helps us authenticate with one specific provider)

- mpn install --save passport passport-google-oauth20

#

index.js -> config/keys.js -> Are we doing dev or prod? 
--prod--> Use env variables
--dev--> config.dev.js (Don't commit this)

# Refactor index.js code to structure project

- server
  - config (Protected API keys and settings)
  - routes (All route handlers, grouped by purpose)
  - services (Helper modules and business logic)
  - index.js (Helper modules and business logic)

# Theeory of Authentication
What is the meaning of authentication ? Why do we care about authentication at all? What does it do for us? What does it mean to be logged into aour application
- hTTP is stateless
  We communicates between our browser and server by http request/ Ajax request. Inherently between two separate requests we make from browser to server, there is nothing that shares information b/w them or identifies them.
  - If we make one req form brower to the server and say here is a email ans password, please log me in. na the server says thats the correct email/password, lets log you in and sends the response back.
  - Then 5 min later, we do a follow up rewuest and say - can i havelaist of all my posts? By default with http server will say - well who are you, you say you are logged in, who are you ? That is because be default, inforamtion between requests is not shared.

  So how do we get around this

  - Browser -- log me in please --> Server (Ok, You're logged in. Here's a piece of info, include it with requests) -- cookie, token, whatever --> Browser
  - Browser -- Can I have a list of my posts ? (cookie, token, whatever) --> Server (Ah, user 123, sure, here are your posts)  -- posts--> Browser

  - We will use cookie based authentication
    - when we give some initial request to the server( like our express api) we will say - Hey log me in (this could be witha ny way - email/password, username, Google OAuth). User goes through the OAuth process. 
    - We are going to generate some identifying keys of information. In the response that we send back to the user for the initial OAuth request, we are going to include what is clled header inside the response that is sent back to the browser
    - the header is going to have a property called 'Set-cookie' and its going to be set to some random token. This token is goin to uniquesly identify the user.
    - When the browser gets the response and see the cookie thing in the header, it stipes the token and stores the token in the browsers memory
    - then the browser is going to automatically append that cookie with any follow up request being sent to the server.

  - So the idea behind authentication in this app is we are going to provide some identifying keys of information into the user's cookie. The cookies is going to be automaticallly be managed by the browser and included in the follow up requests.
  React side of the application is not responsible for managing the cookie/token, browser will manage it.

# email/password authentication flow
Signup (here's an email/password) -> Sign out -> Login (here's an email/password)
(this two email/password combinations are he same? Great, must be the same paeerson!)

# OAuth Flow
Signup (Here;s a google profile) -> Sign out -> Login (Here's a google profile)
(we need to find some unique identifying token in the user's Google profile. Is that consistent between logins? Use that to decide if the user is the same)

- We will use user's google id from the profile and use it forsignup and the login(compare it)
So the Oauth only purpose is to give us this unique identifying token. We will trust that this the same person by trusting Google (that this user id will not change or else we are screwed).
