
# Redux cycle
To change state of our app, we call an Action Creator -> prodces an Action object -> gets fed to Dispatch function -> Forwards the action to Middleware -> Sends action to Reducers -> creates new State -> wait until we need to update state again

# Redux is not Magic
- Redux does not automatically detect action creators being called
- Redux does not automatically detect a funcation returning an object that is an 'action' 

# General Data Loaading with REdux
- Component gets rendered onto the screeen
- Component's 'compoenntDidMount' lifecycle method get called
- we call action creator from 'componentDidMount'
<Components are generally responsible for fetching data they need by calling an action creator, usualy froma lifecycle method>
- Action creator runs code to make an API request
- API responds with data
- Action creator returns an 'action' with the fetched data on the 'payload' property
The dispatch method id going to dispatch that action and send it off to the reducers inside our app
<Action creators are responsible for making API requests/ initiate the data fatching process. This is where Redux-Thunk comes into play>
- Some reducers sees the action, returns the data off the 'payload'
- Because we generated some new state object, redux/react-redux cause our React app to be rendered
<We get fetched data into a component by generating new state in our redux store, then getting that into our component through mapStateToProps>

export const fetchPosts = async () => {
  const response = await jsonplaceholder.get('/posts');
  return {
    type: 'FETCH_POSTS',
    payload: response
  }
}
# Whats wrong with 'fetchPosts' action creator?
- Action creators must return plain JS objects with a type property- We are not!
- By the time our actions gets to a reducer, we won't have fetched our data.

# Step when we run our application
1. Postlist.js
componentDidMount(){
    this.props.fetchPosts()
}
2. Probably happening in React-Redux
store.dispatch(
    fetchPosts()
)
Any time we call an action creator that returns an action, we must take that action and send it to the dispatch function on our store.
3. actions/index.js
export const fetchPosts = async () => {
  const response = await jsonplaceholder.get('/posts');
  return {
    type: 'FETCH_POSTS',
    payload: response
  }
}
- Returns the action object

# Synchonous action creators
- Instantly returns an action with data ready to go
# Asyncronous action creators
- Takes some amount of time for it to get its data ready to go.
- If you want to have async action creators in your application we you need to have a middleware.

# Middleware in Redux
- Functon that gets called with every action we dispatch
- Has the ability to STOP, MODIFY or otherwise mess around with actions
- Most popular use of middleware is for dealing with async actions

# redux-thunk
Middleware to help us make requests in a redux application
- to deal with Asynchronous action creators

# Normal rules
- Action Creator must return action objects
- Action must have a type property
- Actions can optionally have a 'payload'

# Rules with Redux Thunk
- Action Creators can return action objects
        or
- Actions creators can return functions. If you return a function, Redux-Thunk is gonna automatically invoke/call that for you.
- If an action object gets returned, it must have a type
- If an action object gets returned, it can optionally have a 'payload'

//-----------------

Action Creator -> "Something" either an object or function -> dispatch ->
# Redux thunk
"Something" -> Hi Something! Are you a function -> yes -> 
Great! I'm gonna invoke/call you with the 'dispatch' and 'getState' functions. Go ahead and 'dispatch' actions at your leisure ->
 -> Function invoked with 'dispatch' ->
 -> We wait for our request to finish ->
 -> Request complete, dispatch action manually (special power with redux-thunk) -> 
 # -----
 -> New Action -> dispatch -> 
# Redux thunk
"Something" -> Hi Something! Are you a function -> no -> 
 # -----
 -> Reducers

 // If the action creator returns a function, Redux-thunk invokes that function and it passes into it the 'dispatch' and 'getState' functions as arguments.
 // the dispatch function is the same dispatch function we hav been talking about quite a bit.
 // We can pass actions into the dispatch function. Those actions will be sent to all the middleware and evntually forward it to the reducers.
 // the dispatch has an unlimited power to initiate changes to the data on the Redux side of the app
 // getState function can be called ont he redux store and it will return all the data in the redux store.
 // so these two arguments functions that the function receives have unlimited power over everything that goes in our redux application.
 // through dispatch we can change any data we want.
 // through getState we can read/access any data we want.   


 # Rules of Reducers
 - Must return any value besides 'undefined' (this is JS wherein any function(reducers) must have a return statement with value other than undefined)
 - Produces 'state' or data to be used inside of your app using only previous state and the action
 - Must not return reach 'out of itself' to decide what value to return ( reducers are pure)
 - Must not mutate its input 'state' argument

 // when you start up a redux application, each reducer is going to be automaticaly called up one time. SO its an automatic invocation that essentilly allows the reducers to specify some default state value
 // During this initialization process the first time the reducers gets called, its goingto receive two arguments - the first argument is going to have a vvalue of undefined and the second argument is going to be an action object (Action #1)
 selectedSongReducer(undefined, { type: 'fdsfdsf' })
 // the reducer takes these two arguments and returns some state value.

// providing default value for selectedSong
 const selectedsongReducer = (selectedSong = null, action) => {
    // ES1025 syntax for 
    if(selectedSong === undefined){
        selectedSong = null
    }
 }

// the second time the reducer gets called, the first argument is no longer going to be undefined instead its going to be whatever the last tiem the reducer returned

// React-router
When we created our application and loaded it in the browser, we created an instance of the BrowserRouter component.
The BrowserRouter internally creates an object of its own called the history object. This history object is going to look at the address bar and its going to extract just that portion of the url that reacrt-router cares about. Just after the domain name or the port.
The history object is then gonna comunicate that path over to BrowserRouter.
BrowserRouter will then communicated that path to the components
Components ar ethen gonna decide to show themselves or hide themselves depending on the path inside url that the user is visiting and the path property that was passsed to it when it was created

# Email/Password Authentication
- We store a record in a databas witht he user's email and password
- When the user tries to login, we compare email/pw with whats stored in  DB
- A user us 'logged in' when they enter the correct email/pw

# OAuth Authentication
- User authenticates with outside service provider
- User authorizes our app to access their information
- Outside provider tells us about the user
- We are trusting the outside provider to correctly handle identification of a user
- OAuth can be used for 
    1. user identification in our app and
    2. our app making actions on behalf of user

# OAtuth for Servers
- Results in 'token' that a server can use to make requests on behalf of the user
- Usually used when we have an app that needs to access user data when they are not logged in
- Difficult to setup because we need t store a lot info about the user

# OAuth for JS Browser Apps
- Results in a 'token' that a browser app can use to make requests on bahalf of the user
- Usually used when we have an app that only need to access data while they are logged in 
- Very easy to setup thats to Google's JS lib to automate flow

# Steps for Setting up OAuth
- Create a new project at console.developers.google.com/
- Set up an OAuth confirmation screen
- Generate an OAutg Client ID
- Install Google's API library, intialize it with  the OAuth Client ID
- Make sure the lib gets called any time the user clicks on the 'login with Google' button

# REST Conventions
- Predefined system for defining different routes in an API that work with a given type of records.

# Intentional Navigation
User clicks on a 'Link' component
# Programmatic Navigation
We run code to forcibly navigate the user through pur app

# Time line
- User submits the form
- We make request to backend API to create the stream
- Time passes....
- API responds with success  or error
- We either show error to the user or navigate them back to list of streams
 

 BrowserRouter (listens to  'history' object for changes tot he URL)
history object (keeps track of the address in the address bar in your browser)
Any time the address changes the addressbar is going to communicatet he change to the BrowserRouter.
history object os not only about watching the address bar. history object also has the ability to chnage the address bar as well. 
Thats how we are goig to do the programmatic navigation.

# Selection Reducer 
When a user clicks on a stream to edit, use  a 'selectionReducer' to record what stream is being edited
# URL-based selection 
Put the ID of the stream being edited in the URL

With React-Router, each component needs to be designed to work in isolation (fetch its own data!)





