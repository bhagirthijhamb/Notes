npm install -g create-react-app
create-react-app <name of project>

// no need for to install create-react-app
npx create-react-app <name of project> 

A component is a Function or Class that produces HTML to show the user (using JSX) and handles feedback from the user (using Event Handlers)

The role of components is to either show some content or react to user interaction
The whole purpose of props is o customise these two things. Either customise how the component looks or how user interacts with it.

JSX
- A Special dialect of JS (its not HTML )
- Browsers don't understand JSX code! We write JSX then run tools to turn it into normal JS
- Very similar in form and function to HTML with a couple differences

# Three Tenets of Components
- Component Nesting
- Compoennt Reusabity
- Component Configuration (use reacts props system to make the component configurable)

# Props
- System for passing data from a parent tcompoenent to a child component
- Goal is to customise or configure a child component.

# Geolocation API 
- built into most of the browsers

# Rules of Class Components
- Must be a JavaScript Class
- Must extend (subclass) React.Component
- Must define a 'render' method that returns some amount of JSX

# App flow
- We loaded index.html inside our browser that requested the JS file with all our code inside it
- JS file is loaded by browser and executed
- A new instance of App component gets created
- App components 'constructor' funtion gets called.
- We initialize state. State object is created and assigned to the 'this.state' property
- We call geolocation service
- React calls the components render method
- App returns JSX, gets rendered to page as HTM
- ...
- We get result of geolocation!
- We update our state object with a call to 'this.setState'
- React sees that we updated the state of a component.
- React rerenders the component
- React calls our 'render' method a second time.
- React method returns some(updated) JSX
- React takes that JSX and updates content on the screen
- So App component is rendered to times. First time without the value of latitude.

# Error Handling
- If anything goes wrong with the getCurrentPosition() request, we want to rerender the component and directly tell the user - hey something bad just happened.
- So we will update the state to make the component to rerender and tell the user that something bad happened.

# Updating State
- updating state is an additive process in class based components.

# Conditional Rendering
- No latitude - No errMessage -> Show 'Loading...'
- Have latitude - No errMessage -> Show latitude
- No latitude - Have errMessage -> Show error

# Rules of State
- is a JS object that contains data relevant to a component.
- Updating state on a component causes the components to (almost) instantily rerender.
- State must be initialized when a compoennt is created
- 

# Component Lifecycle
- Constructor - Called when a new instance of our component is created. Good place for One time setup (initialize state, data loading, network request to outside api)
- render - not optonal - avoid doing anything besides returning JSX
...Content visible on screeen...
- componentDidMount - gets invoked one time, Good place to do data-loading (best practice to do it here) (we setState here which triggers component rerender)
- render
... sit and wait for updates
- componentDidUpdate - called everytime a component is updated i.e. state changes or component gets a new set of props from the parent component. Good place to do more data-loading when state/props change. Sometype of network request every time a user clicks on a button or everytime they enter a text into an input or every single time we get new props from the parent component
... sit and wait until this component is not longer shown
- componentWillUnmount - Good place to do cleanup(especially for non-React stuff)

# Other lifecycle methods
- shouldComponentUpdate
- getDerivedStateFromProps
- getSnapshotBeforeUpdate

# Controlled Vs Uncontrolled Element
- User types in input
- Callback/arrow function assigned to anChange prop gets invoked
- We call setState with the new value
- Compoennt rerenders
- Input is told what its value is (coming from state)

-What is the value of the input right now ? 
HTML world - (uncontrolled) - reach into the DOM, find the input element and pull the value out of it.
The react component had the idea of the value of the input for very small amout of time when the user had typed something to the input and we had the callback invoked. During all other time the source of truth (data) was our HTML document not inside our React component.
As a React developer we like to store information inside our React components instead of HTML dom
- Now  at any time we want to know the value of input, we can look inside our React component state and see the value there.
- We are storing the information inside the React component inside the state instead of storing it in the DOM.
- If we want the render the search bar with some default value, we can simply chnage the default/initial value of the state.
- We can manilupate whatever the user inputs there for every keypress. Let's say save the uppercase values
onChange={e => this.setState({ term: e.target.value.toUpperCase() })}

# Binding 'this' to the methods.

- Rather than defining a method on the class using an arrow function, we just pass an arrow function directly into the onChange prop

# Hooks 
- are a way to write reusable code, insead of more classic techniques like inheritance.

# useState Hook
- array destructuring

# useEffect Hook
- Allows function component to use something like lifecycle methods
- We configure the hook to run some code automatically in one of three scenarios.
- 1. When the component is rendered for the first time only
- 2. Whent he component is rendered for the first time and whenever it rerenders.
- 3. When the component is rendered for the first time and (whenever it rerenders ans some piece of data has changed).

# useEffect Second Argument
- [] - Run at initial render
- ...nothing... - Run at initial render -Run after every rerender
- [data] -Run at initial render - Run after every rerender if data has changed since last render.


# Clean-up function
- Initial Component Render
    - Func provided to useEffect called
    - Return a cleanup function
- Rerender
    - Invoke the cleanup function!
    - Func provided to useEffect called again
    - Return a cleanup function
- Rerender
    - Invoke the cleanup function!
    - Func provided to useEffect called again
    - Return a cleanup function

# Route Mapping
- When at this url - show this component
- localhost:3000/ -> Accordion
- localhost:3000/list -> Search
- localhost:3000/dropdown -> Dropdown

# window.location 
- is an object that is built into you browser tha tis updated everytime you navigate around to a diffrent url.
>window.location - it has information extracted from the current url
host: "localhost:3000"
hostname: "localhost"
href: "http://localhost:3000/ttranslate"
origin: "http://localhost:3000"
pathname: "/translate"
port: "3000"
protocol: "http:"

# Navigation
- User clicks on 'List'
- Change the URl, but don't do a full page refresh1
- Each Route could detect the URL has changed
- Route could update piece of state tracking the current pathname
- Each Route rerenders, showing/hiding compoenets approximately

Whenever the user navigates to some page they have a tendency to bookmark it and come back to it sometime in the future.
Link coppied and pasted to another tab should show the exact same content.
URL should be in sync with the content being displayed on the screen

We can change the url like this. Without making any addiional request and without chnaging any content on the screen.
Wec an chnage the url to keep it in sync with wht content we are displaying but we are not refreshing the page.
localhost:3000/dropdown
>window.history.pushState({}, '', '/traslate')
changes to 
localhost:3000/translate

# Cutom Hooks
- Bet way to create reusable code in a React project(besides components!)
- Created by extracting hook-related code out of a function component
- To make JSX reusable, we make another component.
- To make other data related logic reusable, we make custom hook (reusable function).
- Custom hook always make use of at least one primitive hook internallly
- Each custom hook should have one purpose
- Kind of an ort form!
- Data-fetching is a great thing to try to make reusable

# process For Creating Resuable Hooks
- Identify each line of code related to some single purpose
- Identify the inputs to that code.
- identify the outputs to that code.
- Extract all of the code into a separate function, receiving the inputs as arguments and returning the outputs

- If you gibe me 1. a default search term, I will fve you 1. way to search for videos 2. a list of videos

# Redux cycle
To change state of our app, we call an Action Creator -> prodces an Action object -> gets fed to Dispatch function -> Forwards the action to Middleware -> Sends action to Reducers -> creates new State -> wait until we need to update state again

# Redux is not Magic
- Redux does not automatically detect action creators being called
- Redux does not automatically detect a funcation returning an object that is an 'action' 

# General Data Loaading with REdux
- Component gets rendered onto the screeen
- Component's 'compoenntDidMount' lifecycle method get called
- we call action creator from 'componentDidMount'
<Components are genrally responsible for fetching data they need by calling an action creator, usualy froma lifecycle method>
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

 




