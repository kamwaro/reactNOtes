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

<h1>Hello, world!</h1>
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

      const element = {
          <h1>
          Hello, {formatName(user)}!
          </h1>
      }

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

- Dont put quotes around curly braces when embedding a Javscript expression in an attribute.
- You should either use quotes (for string values) or curly braces for expressions, but not both in the same attribute.

        Warning:
            - Since JSX is closer to Javscript than to HTML, ReactDOM uses camelCase property naming convention instead of HTML attribute names.
            - For example, class becomes className in JSX, and tabIndex becomes tabIndex.

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
-

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

##### React only Update what's necessary

- React DOM compares the elements and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.
- Even though we create an element describing the whole UI tree on every tick, only the text node whose contents have changed gets updated by ReactDOM
- Thinking about how the UI should look at any given moment, rather than how to change it over time, eliminates a whole class of bugs.

### Components and Props