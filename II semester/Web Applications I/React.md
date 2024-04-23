# Introduction

React uses a **declarative approach**, which means that we don't manipulate the DOM in a direct way: we simply define **components** and how they will be rendered in the page.
React is also based on a functional paradigm: this means that **components** **are functions** and on every change the virtual DOM will update itself.

Components have **props and state**, the first ones are immutable and they describe the properties of a component, while the state are useful to keep track of information during the execution of the app: the state cannot be edit directly, but it can be modified through special calls. **Props** are passed to a component by its parent. State's changes don't affect parents component, just the children: state are often **moved up**, so they can be shared between components.
Another property of React is the **pureness** of function: a function has no side effects besides the return value.

Every time a change is made, React will **re-render** all components from scratch, even with small modification: this does not affect performance since React uses a **virtual DOM** optimized.

To initialize the react virtual DOM we use the `createRoot` React function:

```js
const container = document.getElementById('root');
const root = createRoot(container);
root.render(<h1>Hello, world!</h1>);
```

As we can see above, the argument of the `render` function is similar to HTML, but without quotes. It is called **JSX syntax** which is basically translated (with Babel) to React components invocation.

![[Pasted image 20240423102551.png]]

To **define a custom component** we simply define a function and return a graphical element with JSX syntax: components can also be **dynamic**, so it receives properties that can be printed in the component using the curly brackets notation

```js
	//WITHOUT PROPERTIES
const BlogPostExcerpt = () =>{
	return (
		<div>
			<h1>Title of the blog post</h1>
			<p>Text of the blog post</p>
		</div>
	)
}

 //WITH PROPERTIES
const BlogPostContent = (props) => {
	return (
		<div>
		<p>{props.content}</p>
		</div>
	)
}
```

We can split the components in two families: **presentation components** just displays information, while **container components** interacts with the backend.

### Use state

```js
import { useState } from "react";
function Button(props) {
	let [buttonLang, setButtonLang] = useState(props.lang) ;
	if (buttonLang === 'it')
		return <button onClick={()=>setButtonLang('en')}>Ciao!</button>;
	else
		return <button onClick={()=>setButtonLang('it')}>Hello!</button>;
}
export default Button;
```

To use a state we can define it using the `useState()` method, it return a 2-length array where the second item is a special function to edit the state. 
# React Elements

An **element** is a plain object describing a component instance or DOM node and its desired properties. A React element is a representation of a DOM element in the Virtual DOM.
It contains information about:
- the component **type**
- its **properties**
- any child element inside it

To create a React element we use the `createElement` method:

```js
React.createElement(type, props, children);
```

The **type is a string**, which represent the name of the element, the **props is an object** (defined in `{}`) and **children** is the list of child elements.

```ad-note
title: Convention on naming
Remember that DOM elements are always **lowercase**: div, li, ...
React components are always upper camel case: NavigationBar, BlogPost, ...

```

# JSX syntax



# React Components