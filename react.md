# MAIN CONCEPTS IN REACT

1. Hello world
2. Introducing JSX
3. Rendering elements
4. Components and Props
5. State and Lifecycle
6. Handling events
7. Conditional Rendering
8. Lists and Keys
9. Forms
10. Lifting State Up
11. Composition vs Inheritance
12. Thinking in React

### 1. Hello World

- The smallest React example looks like this:

ReactDOM.render(

<h1>Hello, world!</h1>,
document.getElementById('root')
);

- It displays a heading saying "Hello, world!" on the page.

###### How I read The guide

-This guide examines the building blocks of React apps: elements and components.

- Once I master those two, I can create complex apps from small reausable pieces.

### 2. Introducing JSX

- Consider this variable declaration:
  const element = <h1>Hello, world!</h1>;

- This is called JSX, and it is a syntax extension to Javascript.
- Its recommended to be used with React to describe what the UI should look like.
- JSX produces React "elements".

#### Why JSX?

- React embraces the fact that rendering logic is inherently coupled with other UI logic:
  1. How events are handled.
  2. How the state changes over time.
  3. How the data is prepared for display.

* Instead of artificially separating technologies by putting markup and logic in separate files, React separates concerns with loosely coupled units called "components" that contain both.
* React doesn't require using JSX, but most people find it helpful as a visual aid when working with UI inside the Javsacript code.
* It also allows React to show more useful error and warning messages.

#### Embedding expressions in JSX

- In the example below, we declare a variable called name and then use it inside JSX by wrapping it in curly braces:

  const name = 'Josh Perez'
  const element = <h1>Hello, {name}</h1>

  ReactDOM.render(
  element,
  document.getElementById('root')
  )

* You can put any valid Javascript expression inside the curly braces in JSX. For example, 2+2, user.firstName, or formatName(user) are all valid Javascript expressions.
* In the example below, we embed the result of calling a Javscript function,
  formatName(user), into an <h1> element.

      function formatName(user) {
          return user.firstName + ' '+ user.lastName;
      }

      const user = {
          firstName: 'Harper',
          lastName: 'Perez'
      }

      const element = (
          <h1>
          Hello, {formatName(user)}!
          </h1>
      )

ReactDOM.render(
element,
document.getElementById('root')
)

- JSX is split over multiple lines for readability.
- Its recommended to wrap JSX in parentheses to avoid the pitfalls of automatic semicolon insertion.

#### JSX is an Expression TOO

- After compilation, JSX expressions become regular Javascript function calls and evaluate to Javascript objects.
- This means that you can use JSX inside of if statements and for loops, assign it to variables, accept it as arguments, and return it from functions:

function getGreeting(user){
if(user) {
return <h1>Hello, {formatName(user)}!</h1>
}
return <h1>Hello, Stranger.</h1>
}

#### Specifying Attributes with JSX

- You may use quotes to specify string literals as attributes:
  const element = <div tabIndex="0"></div>

- You may also use curly braces to embed a Javscript expression in an attribute:
  const element = <img src={user.avatarUrl}></img>

- Dont put quotes around curly braces when embedding a Javascript expression in an attribute.
- You should either use quotes (for string values) or curly braces for expressions, but not both in the same attribute.

        Warning:
            - Since JSX is closer to Javscript than to HTML, ReactDOM uses camelCase property naming convention instead of HTML attribute names.
            - For example, class becomes className in JSX, and tabindex becomes tabIndex.

#### Specifying Children with JSX

- If a tag is empty, you may close it immediately with />, like XML.
  const element = <img src={user.avatarUrl} />;

- JSX tags may contain children:
  const element = (
   <div>
  <h1>Hello!</h1>
  <h2>Good to see you here.</h2>
  </div>
  )

#### JSX Prevents Injection Attacks

- Its safe to embed user input in JSX:

const title = response.potentiallyMaliciousInput;
// This is safe

const element = <h1>{title}</h1>

- By default, React DOM escapes any values embedded in JSX before rendering them.Thus it ensures that you can never inject anything that's not explicitly written in your application.
- Everything is converted to a string before being rendered.
- This helps prevent XSS (cross-site-scripting attacks).

#### JSX Represents Objects

- Babel compiles JSX down to React.createElement() calls.

- The examples below are both identical:

  const element = (
    <h1 className="greeting">
    Hello, world!
    </h1>   
    );

  const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
  );

React.createElement() performs a few checks to help you write bug-free code but essentially it creates an object like this:

// Note: This structure is simplified

    const element = {
    type:"h1",
    props: {
        className: 'greeting',
        children: 'Hello, world!'
    }
    }

- These objects are called "React elements".
- -You can think of them as descriptions of what you want to see on the screen.
- React reads these objects and uses them to construct the DOM and keep it to date.

### 3. Rendering elements

- ELements are the smallest building blocks of React apps.
- An element describes what you want to see on the screen.

const element = <h1>Hello, world</h1>;

- Unlike browser DOM elements, React elements are plain objects, and are cheap to create.
- React DOM takes care of updating the DOM to match the React elements.
- Elements are what components are made of.

##### Rendering an element into the DOM;

- Let's say there is a <div> somewhere in your HTML file:

<div id="root"></div>

- We call this a "root" DOM node because everything inside it will be managed by React DOM.

- Applications built with just React usually have a single root DOM node.
- If you are integrating React into an existing app, you may have as many isolated root DOM nodes as you like.

- To render a React element into a root DOM node, pass both to ReactDOM.render():

const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root));

##### Updating the Rendered Element:

- React elements are immutable;
- Once you create an element, you cant change its children or attributes.
- An element is like a single frame in a movie: it represents the UI at a certain point in time.

- With our knowledge so far, the only way to update the UI is to create a new element, and pass it to ReactDOM.render().

- Consider this ticking clock example:

function tick(){
const element = (

<div>
<h1>Hello, world!</h1>
<h2>It is {new Date().toLocaleTimeString()}</h2>
</div>
);

    ReactDOM.render(element, document.getElementById('root'));

}

setInterval(tick, 1000);

- It calls ReactDOM.render() every second from a setInterval() callback

  N/B:
  In practice, most React apps only call ReactDOM.render() once.

##### React only Updates what's necessary

- React DOM compares the elements and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.
- Even though we create an element describing the whole UI tree on every tick, only the text node whose contents have changed gets updated by ReactDOM
- Thinking about how the UI should look at any given moment, rather than how to change it over time, eliminates a whole class of bugs.

# 4. Components and Props

- Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

- Conceptually, components are like Javascript functions. They accept arbitrary inputs called "props" and return React elements describing what should appear on the screen.

### Function and Class Components

- The simplest way to define a component is to write a Javascript function:

function Welcome(props) {
return <h1>Hello, {props.name}</h1>;
}

- This function is a valid React component because it accepts a single "props" (which stands for properties) object argument with datat and returns a React element.
- Such components are called "function components" because they are literally Javacript functions.

- You can also use an ES6 class to define a component:

class Welcome extends React.Component {
render(){
return <h1>Hello, {this.props.name}</h1>
}
}

- The above two components are equivalent from React's point of view.

- Function and Class components both have some additional features.

### Rendering a Component

- Previously, we only encountered React elements that represent DOM tags:

const element = <div />;

- However, elements can also represent user-defined components:

const element = <Welcome name="Sara" />;

- When React sees an element representing a user-defined component, it passes JSX attributes and children to this component as a single object. We call this object "props".

- For example, this code renders "Hello, Sara"

  function Welcome(props) {
  return <h1>Hello, {props.name}</h1>
  }

  const element = <Welcome name="Sara" />;
  ReactDOM.render(
  element,
  document.getElementById('root')
  );

* Let's recap what happens in the above example:

1. We call ReactDOM.render() with the <Welcome name="Sara" /> element.
2. React calls the Welcome component with {name: 'Sara'} as the props.
3. Our Welcome component returns a <h1>Hello, Sara</h1> element as the result.
4. React DOM efficiently updates the DOM to match <h1>Hello, Sara</h1>.

## NOTE:

- Always start component names with a capital letter.
- React treats components starting with lower case letters as DOM tags.
- For example, <div/> represents a HTML div tag, but <Welcome /> represents a component and requires Welcome to be in scope.

### Composing Components

- Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail.
- A button, a form a dialog, a screen: in React apps, all those are commonly expressed as components.

- For example, we can create an App component that renders Welcome many times;

function Welcome(props) {
return <h1>Hello, {props.name} </h1>
}

function App() {
return (

<div>
<Welcome name="Sara">
<Welcome name="Cahal">
<Welcome name="Edite">
</div>
)
}

ReactDOM.render(
<App />,
document.getElementById('root')
);

- Typically, new React apps have a single App component at the very top.
- However, if you integrate React into an existing app, you might start bottom-up with a small component like Button and gradually work your way to the top of the view hierarchy.

### Extracting Components

- You can split components into smaller components.

#### EXample

                        function formatDate(date) {
                    return date.toLocaleDateString();
                  }

                  function Comment(props) {
                    return (
                      <div className="Comment">
                        <div className="UserInfo">
                          <img
                            className="Avatar"
                            src={props.author.avatarUrl}
                            alt={props.author.name}
                          />
                          <div className="UserInfo-name">
                            {props.author.name}
                          </div>
                        </div>
                        <div className="Comment-text">{props.text}</div>
                        <div className="Comment-date">
                          {formatDate(props.date)}
                        </div>
                      </div>
                    );
                  }

                  const comment = {
                    date: new Date(),
                    text: 'I hope you enjoy learning React!',
                    author: {
                      name: 'Hello Kitty',
                      avatarUrl: 'https://placekitten.com/g/64/64',
                    },
                  };


                  ReactDOM.render(
                    <Comment
                      date={comment.date}
                      text={comment.text}
                      author={comment.author}
                    />,
                    document.getElementById('root')
                  );

- It accepts author (an object), text (a string), and date (a date) as props, and describes a comment on a social media website.

*        function Comment(props) {
           return (
            <div className="Comment">
              <div className="UserInfo">
               <img className="Avatar"
              src={props.author.avatarUrl}
              alt={props.author.name}
              />
              <div className="UserInfo-name">

  ==> {props.author.name}
  </div>
  </div>
  <div className="Comment-text">
  ==> {props.text}
  </div>
  <div className="Comment-date">
  ==> {formatDate(props.date)}
  </div>
  </div>
  );
  }

* This component can be tricky to change because of all the nesting, and it is also hard to reuse individual parts of it.

* First, we will extract Avatar:

function Avatar(props) {
return (
<img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
      >
)
}

- The Avatar doesn't need to know that it is being rendered inside a Comment. This is why we have given its prop a more generic name: `user` rather than author.

- Its recommended naming props from the component's own point of view rather than the context in which it us being used.

- We can now simplify Comment a tiny bit

                    function Comment(props) {
                    return (
                      <div className="Comment">
                        <div className="UserInfo">

  ====> <Avatar user={props.author} />
  <div className="UserInfo-name">
  {props.author.name}
  </div>
  </div>
  <div className="Comment-text">
  {props.text}
  </div>
  <div className="Comment-date">
  {formatDate(props.date)}
  </div>
  </div>
  );
  }

* Next, we will extract a UserINfo component that renders an Avatar next to the user's name:

function UserInfo(props) {
return (

<div className="UserInfo">
<Avatar user={props.user} />
<div className="UserInfo-name">
{props.user.name}
</div>
</div>
);
}

- This lets us simplify Comment even further:

            function Comment(props){
              return (
                <div className="Comment">

  ===> <UserInfo user={props.author} />
  <div className="Comment-text">
  {props.text}
  </div>
    
   )
  }

  - EXtracting components might seem like grunt work at first, but having a pallete of reusable components pays off in larger apps.
  - A good rule of thumb is that if a part of your UI is used several times (Button, Panel, Avatar), or is complex enough on its own (App, FeedStory, Comment), it is a good candidate to be a reusable component.

### Props are Read-Only

- Whether you declare a component as a function or a class, it must never modify its own props. Consider this sum function:

  function sum(a, b){
  return a + b;
  }

- Such functions are called "pure" because they do not attempt to change their inputs, and always return the same result for the same inputs.

- In contrast, the function below is impure because it changes its own input:

function withdraw(account, amount){

      account.total -= amount;

}

- React is pretty flexibble but it has a single strict rule:

- `All React components must act like pure functions with respect to their props.`

- Of course, application UIs are dynamic and change over time. Therefore React introduced a anew concept of "state". State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.

## 5. State and LIfecycle:

- Consider the ticking clock example from one of the previous sections.
- In rendering Elements, we have only learned one way to update the UI. WE call ReactDOM.render() to change the rendered output:

function tick(){
const element = (

<div>
<h1>Hello, world!</h1>
<h2>It is {new Date().toLocaleTimeString()}</h2>
</div>
);

ReactDOM.render(
element,
document.getElementById('root')
);
}

setInterval(tick, 1000);

- In this section, we will learn how to make the Clock component truly reusable and encapsulated.
- It will set upu its own timer and update itself every second.

- We can start by encapsulating how the clock looks:

function Clock(props){
return (

<div>
<h1>Hello, world!</h1>
<h2>It is {props.date.toLocaleTimeString()}</h2>
</div>
)
}

function tick(){
ReactDOM.render(
<Clock date={new Date()} />,
document.getElementById('root')
);
}

setInterval(tick, 1000);

- However, it misses a crucial requirement: the fact that the Clock sets up a timerand updates the UI every second be an implementation detail of the Clock.
- Ideally we want to write this once and have the CLock update itself:

  ReactDOM.render(
  <Clock />,
  document.getElementById('root')
  );

* To implement this, we need to add "state" to the Clock component.

* State is similar to props, but it is private and fully controlled by the component.

#### Converting a Function to a Class

- You can convert a function component like Clock to a class in five steps:

1. Create an ES6 class, with the same name, that extends React.Component.
2. Add a single empty method to it called render()
3. Move the body of the function into the render() method.
4. Replace props with this.props in the render() body.
5. Delete the remaining empty function declaration.


    class Clock extends React.Component{
      render(){
        return (
          <div>
          <h1>Hello, world!</h1>
          <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
          </div>
        )

      }
    }

- Clock is now defined as a class rather than a function.

- The render method will be called each time an update happens, but as long as we render <Clock /> into the same DOM node, only a single instance of the Clock class will be used.

- THis lets us use additional features such as local state and lifecycle methods.

#### Adding Local State to a Class

- We will move the date from props to state in three steps:

1.  Replace `this.props.date` with `this.state.date` in the `render()` method:

        class Clock extends React.Component {
          render(){
            return (
              <div>
              <h1>Hello, world!</h1>
              <h2>It is {this.state.date.toLocaleTimeString()}</h2>
              </div>
            )
          }
        }

2.  Add a class constructor that assigns the initial `this.state:`

class Clock extends React.Component {
constructor(props){
super(props)
this.state = {date: new Date()};
}

    render(){
      return (
        <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
        </div>
      )
    }

}

==> Note how we pass `props` to the base constructor:
constructor(props){
super(props);
this.state = {date: new Date()};
}

- Class components should always call the base constructor with props.

3. Remove the `date` prop from the <Clock /> element:

ReactDOM.render(
<Clock />,
document.getElementById('root')
);

- We will later add the timer code back to the component itself.
- The result looks like this:

class Clock extends React.Component {
constructor(props) {
super(props);
this.state = {date: new Date()}
}

render() {
return (

<div>
<h1>Hello, world!</h1>
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
</div>
)
}
}

ReactDOM.render(<Clock />, document.getElementById('root'))

- Next, we'll make the Clock set up its own timer and update itself every second.

#### Adding Lifecycle Methods to a Class

- In applications with many components, it's very important to free up resources taken by the components when they are destroyed.
- We want to set up a timer whenever the Clock is rendered to the DOM for the first time.
- This is called "mounting" in React.
- We also want `to clear that timer` whenever the DOM produced by the Clock is removed.
- This is called "unmounting" in React.
- We can declare special methods on the component class to run some code when component mounts and unmounts:

class Clock extends React.Component {
constructor(props){
super(props);

    this.state = {date: new Date()};

}

componentDidMount(){

}

componentWillUnmount(){

}

render(){
return (

<div>
<h1>Hello, world!</h1>
<h2>It is {this.state.date.toLocaleTimeString()}</h2>
</div>
)
}
}

- These methods are called "lifecycle methods".

- The componentDidMount() method runs after the component output has been rendered to the DOM. This is a good place to set up a timer:

componentDidMount() {
this.timerID = setInterval(
() => this.tick(), 1000
)
}

- Note how we save the timer ID right on `this (this.timerID`.

- While `this.props` is set up by React itself and `this.state` has a special meaning, you are free to add additional fields to the class manually if you need to store something that doesn't participate in the data flow (like a timer ID).

- We will tear down the timer in the `componentWillUnmount()` lifecycle method:

componentWillUnmount(){
clearInterval(this.timerID);
}

- Finallly, we will implement a method called tick() that the Clock component will run every second.

- It will use `this.setState()` to schedule updates to the component local state:

          Class Clock extends React.Component {
            constructor(props){
              super(props);
              this.state = {date: new Date()}

              componentDidMount(){
                this.timerID = setInterval(
                  () => this.tick(), 1000
                );
              }

              componentWillUnmount(){
                clearInterval(this.timerID);
              }

              tick(){
                this.setState({
                  date: new Date()
                })
              }

              render(){
                return (
                  <div>
                  <h1>Hello, world!</h1>
                  <h2>It is {this.state.date.toLocaleTimeString()}</h2>
                  </div>
                )
              }
          }

          ReactDOM.render(
            <Clock />,
            document.getElementById('root')
          );

* Now the clock ticks every second.
* Let's quickly recap what's going on and the order in which the methods are called:

1. When <Clock /> is passed to ReactDOM.render(), React calls the constructor of the Clock component. Since Clock needs to display the current time, it initializes `this.state` with
   an object including the current time. We will later update this state.

2. React then calls the Clock component's render() method. This is how React learns what should be displayed on the screen. React then updates the DOM to match the Clock's render output.

3. When the Clock output is inserted in the DOM, react calls the ComponentDidMount lifecycle method. Inside it, the Clock component asks the browser to set up a timer to call the component's tick() method once a second.

4. `Every second the browser calls the tick()method. Inside it, the Clock component schedules a UI update by calling setState() with an object containing the current time. Thanks to the setState() call, React knows the state has changed, and calls the render method again to learn what should be on the screen. This time,`this.state.date`in the render() method will be different, and so the render output will include the updated time. React updates the DOM accordingly.`

5. If the Clock component is ever removed from the DOM, react calls the componentWillUnmount() lifecycle method so the timer is stopped.

#### Using State Correctly

- There are three things you should know about setState();

##### Do Not Modify State Directly

- For example, this will not re-render a component:

// Wrong
this.state.comment = 'Hello';

- Instead, use setState();

// Correct
this.setState({comment: 'Hello'});

- The only place where you can assign `this.state` is the constructor.

##### State Updates May Be Asynchronous

- React may batch multiple setState() calls into a single update for performance.
- Because `this.props` and `this.state` maybe updated asynchronously, you should not rely on their values for calculating the nex state.
- For example, this code may fail to update the counter:

// Wrong

this.setState({counter: this.state.counter + this.props.increment});

- To fix it, use a second form of setState() that accepts a function rather than an object.
  That function will receive the previous state as the first argument,and the props at the time the update is applied as the second argument:

// Correct

this.setState((state, props) => ({
counter: state.counter + props.increment
}));

- We used an arrow function above, but it also works with regular functions:

// Correct

this.setState(function(state, props){
return {
counter: state.counter + props.increment
}
});

##### State Updates are Merged

- When you call `setState()`, React merges the object you provide into the current state.
- For example, your state may contain several independent variables:

constructor(props){
super(props);
this.state = {
post: [],
comments:[]
};
}

- Then you can update them independently with separate `setState() calls`:

componentDidMount() {
fetchPosts().then(response => {
this.setState({
posts: response.posts
});
})
}

fetchComments().then(response => {
this.setState({
comments: response.comments
})
})

- The merging is shallow, so this.setState({comments}) leaves `this.state.posts` intact, but completely replaces `this.state.comments`

##### The Data Flows Down

- Neither parent nor child components can know if a certain component is stateful or stateless,and they shouldn't care whether it is defined as a function or a class.
- This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.

- A component may choose to pass its state down as props to its child components:

<h2>It is {this.state.date.toLocaleTimeString()}</h2>

- This also works for user-defined components:

<FormattedDate date={this.state.date} />

- The FormattedDate component would receive the date in its props and wouldn't know whether it came from the Clock's state, from the Clock's props, or was typed by hand:

  function FormattedDate(props){
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>
  }

- This is commonly called "top-down" or "unidirectional" data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components "below" them in the tree.

- If you imagine a component tree as a waterfall of props, each component's state is like an additional water source that joins it at an arbitrary point but also flows down.

- To show that all components are truly isolated, we can create an App component that renders three <Clock>s;

      function App() {
        return (
          <div>
          <Clock />
          <Clock />
          <Clock />
          </div>
        )
      }

      ReactDOM.render(
        <App />,
        document.getElementById('root')
      );

- Each Clock sets up its own timer and updates independently.

- In React apps, whether a component is stateful or stateless is considered an implementation detail of the component that may change over time. You can use stateless components inside stateful components, and vice versa.

## 6. Handling Events

- Handling events with React elements is very similar to handling events on DOM elements.
- There are some syntax differences:

  - React events are named using camelCase. rather than lowercase.
  - With JSX you pass a function as the event handler, rather than a string.

- For example, the HTML:

<button onclick="activateLasers()">
Activate lasers
</button>

is slightly different in React:

<button onClick={activateLasers}>
Activate Lasers
</button>

- Another difference is that you cannot return `false` to prevent default behavior in React.
- You must call `preventDefault` explicitly. For example, with plain HTML, to prevent the default link behaviour of opening a new page, you can write.

<a href="#" onclick="console.log('The link was clicked.') return false">
Click me
</a>

-In React, this could instead be:

function ActionLink(){
function handleClick(e){
e.preventDefault()
console.log('The link was clicked.')
}

    return (<a href="#" onClick={handleClick}>
              Click me
          </a>)

}

- Here `e` is a synthetic event. React defines these synthetic events according to the W3C spec, so you dont need to worry about cross-browser compatibilty.

- When using React, you generally don't need to call addEventListener to add listeners to a DOM element after it's created.Instead,just provide a listener when the element is initially rendered.

- When you define a component using an ES6 class, a common pattern is for an event handler to be a method on the class.
- For example, the toggle component below renders a button that lets the user toggle between "ON" and "OFF" states.

class Toggle extends React.component {
constructor(props){
super(props);

    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);

}

                      handleClick(){
                        this.setState(state => ({
                          isToggleOn:!state.isToggleOn
                        }))
                      }

render(){
return (
<button onClick={this.handleClick}>
{this.state.isToggleOn ? 'ON' : 'OFF'}
</button>
)
}

ReactDOM.render(
<Toggle />, document.getElementById('root')
);
}

- You have to be careful about the meaning of `this` in JSX callbacks. In Javascript, class methods are not bound by default. If you forget to bind `this.handleClick` and pass it to onClick, `this` will be `undefined` when the function is actually called.

- This is not React-specific behaviour; it is a part of how functions work in Javascript. Generally, if you refer to a method without () after it, such as onClick={this.handleClick}, you should bind that method.

- If calling bind annoys you, there are ways you can get around this. If you are using the experimental `public class fields syntax`, you can use class fields to correctly bind callbacks.


    Class LoggingButton extends React.Component {
      // This syntax ensures  `this` is bound within handleClick
      // Warning: this is *experimental* syntax

      handleClick = () => {
        console.log('This is:' this);
      }

      render(){
        return (
          <button onClick={this.handleClick}>
          Click
          </button>
        );
      }
    }

- This syntax is enabled by default in Create React App.
- If you aren't using class fields syntax, you can use an `arrow function` in the callback:

class LoggingButton extends React.Component {
handleClick(){
console.log('this is', this)
}

render(){
// This syntax ensures `this` is bound within handleClick

    return (
      <button onClick={() => this.handleClick()}>
      Click me
      </button>
    )

}
}

- The problem with this syntax is that a different callback is created each time the LogginButton renders. In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering.
- Its generally recommended BINDING IN THE CONSTRUCTOR OR USING CLASS FIELDS SYNTAX , to avoid this sort of performance problem.

##### Passing Arguments to Event Handlers

- Inside a loop, it is common to want to pass an extra parameter to an event handler.For example, if `id` is the row ID, either of the following would work.

<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>

<button onClick={this.deleteRow.bind(this, id)}>Delete Row </button>

- The above two lines are equivalent, and use `arrow functions` and `Function.prototype.bind` respectively.

- In both cases, the `e` argument representing the React event will be passed as a second argument after the ID. With an arrow function, we have to pass it explicitly, but with `bind` any further arguments are automatically forwarded.

## 7. Conditional Rendering

- In React, you can create distinct components that encapsulate behaviour you need.Then, you can render only some of them, depending on the state of your application.

- Conditional rendering in React works the same way conditions work in Javascript. Use Javascript operators like `if` or the `conditional operator` to create elements representing the current state, and let React update the UI to match them.

- Consider these two components:

      function UserGreeting(props) {
        return <h1>Welcome back!</h1>
      }

      function GuestGreeting(props){
        return <h1>Please sign up.</h1>;
      }

- We'll create a Greeting component that displays either of these components depending on whether a user is logged in:


      function Greeting(props) {
        const isLoggedIn = props.isLoggedIn;
        if(isLoggedIn){
          return <UserGreeting />;
        }

        return <GuestGreeting />;
      }

      ReactDOM.render(
        // Try changing to isLoggedIn={true}
        <Greeting isLoggedIn={false} />,
        document.getElementById('root')
      )

- This example renders a different greeting depending on the value of `isLoggedIn` prop.

#### Element Variables

- You can use variables to store elements.
- This can help you conditionally render a part of the component while the rest of the output doesn't change.

- Consider these two new components representing Logout and Login buttons:

function LoginButton(props){
return (
<button onClick={props.onClick}>
Login
</button>
);
}

function LogoutButton(props){
return (
<button onClick={props.onClick}>
Logout
</button>
);
}

- In the example below, we will create a stateful component called `LoginControl`
- It will render either <LoginButton /> or <LogoutButton /> depending on its current state. It will also render a <Greeting /> from the previous example:

class LoginControl extends React.Component {
constructor(props){
super(props);
this.handleLoginClick = this.handleLoginClick.bind(this);
this.handleLogoutClick = this.handleLogoutClick.bind(this);
this.state = {isLoggedIn: false};
}

handleLoginClick(){
this.setState({isLogggedIn: true});
}

handleLogoutClick(){
this.setState({isLoggedIn: false});
}

render(){
const isLoggedIn = this.state.isLoggedIn;
let button;
if(isLoggedIn){
button = <LogoutButton onClick={this.handleLogoutClick} />
} else{
button = <LoginButton onClick={this.handleLoginClick} />
}

    return (
      <div>
      <Greeting isLoggedIn={isLoggedIn}>
      {button}
      </div>
    )

}
}

ReactDOM.render(
<LoginControl />,
document.getElementById('root')
);

- While declaring a variable and using an `if` statement is a fine way to conditionally render a component, sometimes you might want to use a shorter syntax. There are a few ways to inline conditions in JSX, explained below.

#### Inline If With Logical && Operator

- You may embed any expressions in JSX by wrapping them in curly braces. This includes the Javascript logical && operator. It can be handy for conditionally including an element.


        function Mailbox(props){
          const unreadMessages = props.unreadMessages;
          return (
            <div>
              <h1>Hello!</h1>
              {unreadMessages.length > 0 && <h2>You have {unreadMessages.length} unread messages. </h2>}
              </div>
          );
        }

        const messages = ['React', 'Re: React', 'Re:Re: React'];

        ReactDOM.render(<Mailbox unreadMessages={messages} />, document.getElementById('root'));

- It works because in JavaScript, `true && expression` always evaluates to `expression`, and `false && expression` always evaluates to `false`.
- Therefore, if the condition is `true`, the element right after && will appear in the output. If it is `false`, React will ignore and skip it.

###### Inline If-Else with Conditional Operator

- Another method for conditionally rendering elements inline is to use the Javascript conditional operator `condition ? true: false`.
- In the example below, we use it to conditionally render a small block of text.

  render(){
  const isLoggedIn =this.state.isLoggedIn;
  return (
  <div>
  The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
  </div>
  )
  }

- It can also be used for larger expressions although it is less obvious what's going on:

render(){
const isLoggedIn = this.state.isLoggedIn;
return (

<div>
{isLoggedIn ? <LogoutButton onClick={this.handleLogoutClick} /> : <LoginButton onClick={this.handleLoginClick}>}
</div>
);
}

- Just like in Javascript, it is up to you to choose an appropriate style based on what you and your team consider more readable. Also remember that whenever conditions become too complex, it might be a good time to `extract a component`.

#### Preventing Component from Rendering

- In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return `null` instead of its render output.

- In the example below, the <WarningBanner /> is rendered depending on the value of the prop called `warn`. If the value of the prop is `false`, then the component does not render.

function WarningBanner(props){
if(!props.warn){
return null;
}

return (

<div className="warning">
Warning!
</div>
);
}

class Page extends React.Component {
constructor(props){
super(props);
this.state = {showWarning: true};
this.handleToggleClick = this.handleToggleClick.bind(this);
}

handleToggleClick(){
this.setState(state => ({showWarning: !state.showWarning}))
}
render(){
return (

<div>
<WarningBanner warn={this.state.showWarning}>
<button onClick={this.handleToggleClick}>
{this.state.showWarning > 'Hide': 'Show'}
</button>
</div>
)
}
}

ReactDOM.render(
<Page />,
document.getElementById('root')
)

- Returning `null` from a component's `render` method does not affect the firing of the component's lifecycle methods.
- For instance `componentDidUpdate` will still be called.

## 8. Lists and Keys

- First, let's review how you transform lists in Javascript

- Given the code below, we use the map() function to take an array of `numbers` and double their values. We assign the new array returned by map() to the variable doubled and log it:

const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number \* 2);
console.log(doubled);

- This code logs [2, 4, 6, 8, 10] to the console.

- In React, transforming arrays into lists of `elements` is nearly identical.

#### Rendering Multiple Components:

- You can build collections of elements and include them in JSX using curly braces {}.
- Below, we loop through the `numbers` array using the JavaScript map() function. We return a <li> element for each item. Finally, we assign the resulting array of elements to listItems:

      const numbers = [1, 2, 3, 4, 5];

      const listItems  = numbers.map((number) => <li>{ number }</li>)

* We include the entire `listItems` array inside a <ul> element, and `render it to the DOM`.

* ReactDOM.render(
  <ul>{listItems}</ul>,document.getElementById('root')
);

* This code displays a bullet list of numbers between 1 and 5.

### Basic List Component:

- Usually you would render lists inside a `component`.
- We can refactor the previous example into a component that accepts an array of `numbers` and outputs a list of elements.

function NumberList(props) {
const numbers = props.numbers;
const listItems = numbers.map((number) => <li>{number}</li>);
return (

<ul>{listItems}</ul>
)
}

const numbers = [1,2,3,4,5];

ReactDOM.render(
<NumberList numbers={numbers} />,
document.getElementById('root')
);

- When you run this code, you'll be given a warning that a key should be provided for list items. A "key" is a special string attribute you need to include when creating lists of elements. We'll discuss why it's important in the next section.

- Let's assign a `key` to our list items inside `numbers.map()` and fix the missing key issue.

            function NumberList(props) {
              const numbers = props.numbers;
              const listItems = numbers.map((number) => <li key={number.toString()}>
              {number}
              </li>
              );

              return (
                <ul>{listItems}</ul>
              );
            }

            const numbers = [1,2,3,4,5]
            ReactDOM.render(
              <NumberList numbers={numbers} />,
              document.getElementById('root')
            );

#### Key

- Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:

      const numbers = [1,2,3,4,5];
      const listItems= numbers.map((number) => <li key={number.toString()}>{number}</li>);

* The best way to pick a key is to use a string that uniquely identifies a list item among its siblings. Most often you would use IDs from your data as keys:

const todoItems = todos.map((todo) => <li key={todo.id}>{todo.text}</li>);

- When you don't have stable IDs for rendered items, you may use the item index as a key as a last resort:

        const todoItems = todo.map((todo, index) =>
        // Only do this if items have no stable IDs
        <li key={index}>
        {todo.text}
        </li>
        );

* We don't recommend using indexes for keys if the order of items may change. This can negatively impact performance and may cause issues with component state.
* If you choose not to assign an explicit key to list items then React will default to using indexes as keys.

#### Extracting Components with Keys

- Keys only make sense in the context of the surrounding array.
- For example, if you extract a `ListItem` component, you should keep the key on the <ListItem /> elements in the array rather than on the <li> element in the `ListItem` itself.

#### EXample: Incorrect Key Usage

          function ListItem(props){
            const value = props.value;

            return (
              // Wrong! There is no need to specify the key here:
              <li key={value.toString()}>
              {value}
              </li>
            );
          }

          function NumberList(props){
            const numbers = props.numbers;
            const listItems = numbers.map((number) =>
            // Wrong! The key should have been specified here:
            <ListItem value={number} />
            );

            return (
              <ul>
              {listItems}
              </ul>
            )
          }

          const numbers = [1,2,3,4,5];

          ReactDOM.render(
            <NumberList numbers={numbers} />,
            document.getElementById('root')
          );

### EXample: Correct Key Usage

          function ListItem(props) {
            // Correct! There is no need to specify the key here:

            return <li>{props.value}</li>
          }

          function NumberList(props){
            const numbers = props.numbers;
            const listItems = numbers.map((number) =>
            // Correct! Key should be specified inside the array.
            <ListItem key={number.toString() value={number} />
            );

            return (
              <ul>
              {listItems}
              </ul>
            );
          }

          const numbers = [1, 2, 3, 4 , 5];

          ReactDOM.render(
            <NumberList numbers={numbers} />,
            document.getElementById('root)
          )

- A good rule of thumb is that elements inside the map() call need keys.

#### Keys Must Only Be Unique Among Siblings

- Keys used within arrays should be unique among their siblings. However they dont need to be globally unique. We can use the same keys when we produce two different arrays:

            function Blog(props) {
              const sidebar = (
                <ul>
                {props.posts.map((post) => <li key={post.id}>{post.title} </li>)}
                </ul>
              );

              const content = props.posts.map((post) => <div key={post.id}><h3>{post.title}</h3><p>{post.content}</p></div>);

              return (
                <div>
                {sidebar}
                <hr />
                {content}
                </div>
              );
            }

            const posts = [
              {id:1, title:"Hello world", content: "Welcome to learning React!"},
              {id:2, title:'Installation', content: 'You can install React from npm' }
            ];

            ReactDOM.render(
              <Blog posts={posts} />,
              document.getElementById('root')
            )

- Keys serve as a hint to React but they don't get passed to your components. If you need the same value in your component, pass it explicitly as a prop with a different name:

const content = posts.map((post) => <Post key={post.id}
id={post.id}
title={post.title}
/>)

- With the example above, the `Post` component can read `props.id`, but not `props.key`

#### Embedding map() in JSX

- In the examples above we declared a separate `listItems` variable and included it in `JSX`:

      function NumberList(props){
        const numbers = props.numbers;
        const listItems = numbers.map((number) => <ListItem key={number.toString()} value={number} />);

        return (
          <ul>
          {listItems}
          </ul>
        );
      }

- JSX allows `embedding any expression` in curly braces so we could inline the map() result.

        function NumberList(props){
          const numbers = props.numbers;
          return (
            <ul>
            {numbers.map((number) => <ListItem key={number.toString()}>)} value={number}
            </ul>
          )
        }

- Sometimes this results in clearer code, but this style can also be abused.
- Like in Javscript, it is up to you to decide whether it is worth extracting a variable for readability. Keep in mind that if the map() body is too nested, it might be a good time to extract a component.

## 9. Forms

- HTML form elements work a little bit differently from other DOM elements in React, because form elements naturally keep some internal state. For example, this form in plain HTML accepts a single name:


        <form>
        <label>
        Name:
        <input type="text" name="name"/>
        </label>
        <input type="submit" value="Submit" />
        </form>

- This form has the default HTML form behaviour of browsing to a new page when the user submits the form. If you want this behaviour in React, it just works. But in most cases, it's convenient to have a Javascript function that handles the submission of the form and has access to the data that the user entered into the form. The standard way to achieve this is with a technique called "controlled components".

###### Controlled Compoents

- In HTML, form elements such as <input>, <textarea>, and <select> typically maintain their own state and update it based on user input. In React, mutable state is typically kept in the state property of components and only updated with `setState()`.

- We can combine the two by making the React state be the 'single source of truth'. Then the React component that renders a form also controls what happens in the form on subsequent user input. An input form element whose value is controlled by React in this way is called a "controlled component".
- For example, if we want to make the previous example log the name when it is submitted, we can write the form as a controlled component:

          class NameForm extends React.Component {
            constructor(props){
              super(props);
              this.state = {value: ''};

              this.handleChange = this.handleChange.bind(this);
              this.handleSubmit = this.handleSubmit.bind(this);
            }

            handleChange(event) {
              this.setState({value: event.target.value})
            }

            handleSubmit(event){
              alert('A name was submitted: ' + this.state.value);

              event.preventDefault();
            }

            render(){
              return (
                <form onSubmit={this.handleSubmit}>
                <label>
                Name:
                <input type="text" value={this.state.value} onChange={this.handleChange} />
                </label>
                <input type="submit" value="Submit" />
                </form>
              )
            }
          }

* Since the `value` attribute is set on our form element, the displayed value will always be `this.state.value`, making the React state the source of truth. Since `handleChange` runs on every keystroke to update the React state, the displayed value will update as the user types.

* Within a controlled component, the input's value is always driven by the React state. While this means you have to type a bit more code, you cannow pass the value to other UI elements too, or reset it from other event handlers.

#### The textarea Tag

- In HTML, a <textarea> element defines its text by its children:

    <textarea>
    Hello there, this is some text in a text area
    </texarea>

In React,a <textarea> uses a `value` attribute instead. This way, a form using a <textarea> can be written very similarly to a form that uses a single-line input:

class EssayForm extends React.Component {
constructor(props) {
super(props);
this.state = {
value: 'Please write an essay about your favourite DOM element.'
};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);

}

handleChange(event){
this.setState({value: event.target.value})
}

handleSubmit(event){
alert('An essay was submitted: ' + this.state.value);
event.preventDefault();
}

render(){
return (
<form onSubmit={this.handleSubmit}>
<label>
Essay:
<textarea value={this.state.value} onChange={this.handleChange} />
</label>
<input type="submit" value="Submit">
</form>
);
}
}

- Notice that `this.state.value` is initialized in the constructor, so that the text area starts off with some text in it.

#### The select Tag

- In HTML, <select> creates a drop-down list. For example, this HTML creates a drop-downlist of flavors:

<select>
<option value="grapefruit">Grapefruit</option>
<option value="lime">Lime</option>
<option selected value="coconut">Coconut</option>
<option value="mango">Mango</option>
</select>

- Note that the Coconut option is intially selected,because of the `selected` attribute. React, instead of using this `selected` attribute, uses a `value` attribute on the root `select` tag.
- This is more convenient in a controlled component because you only need to update it in one place. For example;

            class FlavorForm extends React.Components {
              constructor(props){
                super(props);
                this.state = {value: 'coconut'};

                this.handleChange = this.handleChange.bind(this);
                this.handleSubmit = this.handleSubmit.bind(this);

                handleChange(event){
                    this.setState({value: event.target.value})
                }

                handleSubmit(event){
                  alert('Your favorite flavor is: ' + this.state.value);
                  event.preventDefault();
                }

              render(){
                return (
                <form onSubmit={this.handleSubmit}>
                  <label>
                  Pick your favourite flavor:
                  <select value={this.state.value} onChange={this.handleChange}>
                  <option value="grapefruit">Grapefruit</option>
                  <option value="lime">Lime<option>
                  <option value="coconut">Coconut</option>
                  <option value="mango">Mango</option>
                  </select>
                  </label>
                  <input type="submit" value="Submit" />
                  </form>
                );
              }
              }
            }

* Overall, this makes it so that <input type="text">, <textarea>, and <select> all work very similarly- they all accept a `value` attribute that you can use to implement a controlled component.

```N/B::
  You can pass an array into the `value` attribute, allowing you to select multiple options in a `select` tag:

  <select multiple={true} value={['B', 'C']}>
```

#### The file input Tag

- In HTML, an <input type="file"> lets the user choose one or more files from their device storage to be uploaded to a server or manipulated by JavaScript via the `File API`.

      <input type="file" />

- Because its value is read-only, it is an `uncontrolled` component in React.

#### Handling Multiple Inputs

- When you need to handle multiple controlled `input` elements, you can add a name attribute to each element and let the handler function choose what to do based on the value of `event.target.name`.

`For example:`

class Reservation extends React.Component {
constructor(props){
super(props);
this.state = {
isGoing: true,
numberOfGuests: 2
};

    this.handleInputChange = this.handleInputChange.bind(this);

}

handleInputChange(event){
const target = event.target;
const value = target.name === 'isGoing' ? target.checked : target.value;
const name = target.name;

    this.setState({
      [name]: value
    })

}

render(){
return (
<form>
<label>
Is going:
<input name='isGoing' type="checkbox" checked={this.state.isGoing} onChange={this.handleInputChange} />
</label>
<br />
<label>
Number of guests:
<input name="numberOfGuests" type="number" value={this.state.numberofGuests} onChange={this.handleInputChange} />
</label>
</form>
);
}
}

- Note how we used the ES6 `computed property name` syntax to update the state key corresponding to the given input name:

this.setState({
[name]: value
});

- It is equivalent to this ES5 code:

      var partialState = {};
      partialState[name] = value;
      this.setState(partialState)

- Also, since `setState()` automatically `merges a partial state into the current state`, we only needed to call it with the changed parts.

#### Controlled Input Null Value

- Specifying the value prop on a `controlled component` prevents the user from changing the input unless you desire so. If you've specified a `value` but the input is still editable, you may have accidentally set `value` to `undefined` or `null`.
- The following code demonstrates this. (The input is locked at first but becomes editable after a short delay.)

        ReactDOM.render(<input value="hi" />, mountNode)

        setTimeout(function(){
          ReactDOM.render(<input value={null} />, mountNode);
        }, 1000);

#### Alternatives to Controlled Components

- It can sometimes be tedious to use controlled components, because you need to write an event handler for every way your data can change and pipe all of the input state through a React component. This can become particularly annoying when you are converting a preexisting codebase to React, or integrating a React application with a non-React library. In these situations, you might want to check out `uncontrolled components`, an alternative technique for implementind input forms.

#### Fully-Fledged SLoutin
