# JSUI
**Base methods for generating and manipulating HTML with vanilla javascript using a functional programming approach.**

I was always a fan of the way I could interact with the DOM in jQuery, but as time went on and javascript made some serious advancements, I found it to be unnecessary bloat to include. However, it was equally as annoying to use createElement and accept a more procedural approach in my scripts. The following code allows me to interact with the DOM just how I did in jQuery with functional style programming, but uses all native javascript properties. There is no magic. This is simply a group of methods for selecting/creating HTML nodes and assigning properties to them. The biggest edge case to account for is that **children** is not a strict term. Any object property whos data type is an array will be assigned as children. This could allow for classifying children under different property names. Potentially useful? I didn't like the idea of strictly matching a property name.

## Methods
**Create:** create an element

**Select:** select, modify and return element

**MSelect:** select, modify and return group of elements

## The Code
```
var Select = function (selector, obj = {}) {
	return Object.keys(obj).length === 0
		? document.querySelector(selector)
		: Create(document.querySelector(selector), obj, true)
}

var MSelect = function (selector, obj = {}) {
	return Object.keys(obj).length === 0
		? document.querySelectorAll(selector)
		: document.querySelectorAll(selector).forEach(x =>
			Create(x, obj, true)
		) || document.querySelectorAll(selector)
}

var Create = function (node, obj = {}, isNode = false) {
	return Object.keys(obj).reduce((x, c) =>
		Array.isArray(obj[c])
			? obj[c].forEach(y => x.appendChild(y)) || x
			: typeof obj[c] === 'object'
				? null & Object.assign(x[c], obj[c]) || x
				: Object.assign(x, { [c]: obj[c] })
	, isNode ? node : document.createElement(node));
}
```

## Minified via minify-js.com
```
var Select=function(e,t={}){return 0===Object.keys(t).length?document.querySelector(e):Create(document.querySelector(e),t,!0)},MSelect=function(e,t={}){return 0===Object.keys(t).length?document.querySelectorAll(e):document.querySelectorAll(e).forEach((e=>Create(e,t,!0)))||document.querySelectorAll(e)},Create=function(e,t={},r=!1){return Object.keys(t).reduce(((e,r)=>Array.isArray(t[r])?t[r].forEach((t=>e.appendChild(t)))||e:"object"==typeof t[r]?null&Object.assign(e[r],t[r])||e:Object.assign(e,{[r]:t[r]})),r?e:document.createElement(e))};
```

## Example
```
document.body.appendChild(
	Create('div', {
		id: 'main',
		className: 'body',
		style: {
			maxWidth: '800px',
			width: '100%',
			margin: '40px',
			boxSizing: 'border-box',
			border: '4px solid #444444',
			textAlign: 'center',
			fontFamily: 'Arial',
			margin: '0 auto'
		},
		children: [
			Create('h1', {
				innerHTML: 'Welcome to JSUI',
				id: 'title',
				style: {
					color: '#222222'
				}
			}),
			Create('p', {
				innerHTML: 'This library will remain as simple as possible to decrease the amount of characters I have to type while manipulating the DOM with vanilla JavaScript.',
				style: {
					color: '#444444'
				}
			}),
			Create('button', {
				innerHTML: 'Click to do stuff',
				style: {
					marginBottom: '10px'
				},
				onclick: function () {
					MSelect('div', {
						style: {
							color: '#c30010',
							textDecoration: 'underline'
						}
					});
					Select('#title', {
						innerHTML: this.data
					}).prepend(
						Create('div', {
							style: {
								fontSize: '10px'
							},
							innerHTML: 'inserted via selection'
						})
					);
				},
				data: 'Welcome to Vanilla JavaScript'
			})
		]
	})
);
```

## Understanding the Code (comments)
```
/*
selector: valid css selector string
obj: object properties to assign to the selected node before returning
	(if no obj is defined, just return the node)
*/
var Select = function (selector, obj = {}) {
	return Object.keys(obj).length === 0
		? document.querySelector(selector)
		: Create(document.querySelector(selector), obj, true)
}

/*
selector: valid css selector string
obj: object properties to assign to the selected node list before returning
	(if no obj is defined, just return the node list)
... .forEach has no return value so, query again after changes for the return value
*/
var MSelect = function (selector, obj = {}) {
	return Object.keys(obj).length === 0
		? document.querySelectorAll(selector)
		: document.querySelectorAll(selector).forEach(x =>
			Create(x, obj, true)
		) || document.querySelectorAll(selector)
}

/*
node: accepts a selected node from Select and MSelect (requiring isNode to be true)
	OR accepts a valid nodename string for creating an HTML node (requiring isNode to be false, default)
	isNode boolean is used for speed rather than checking if the object is an HTML node at every function call
*/
var Create = function (node, obj = {}, isNode = false) {
	// itterate all object property names (Object.keys) and reduce this list where:
	//	x is the current node
	//	c is the current object property name
	return Object.keys(obj).reduce((x, c) =>
		Array.isArray(obj[c])
			// if object property is an array, append these as children to the current node
			// **forEach has no return value, so, || x to reduce with the current appended values
			? obj[c].forEach(y => x.appendChild(y)) || x
			: typeof obj[c] === 'object'
				// if object property is an object itself, that object must be assigned to the current node via the property name
				// this changes our target object to the current node's assigned property, so null & [assignment] || x is used to null the assignment return value and then return the current node
				? null & Object.assign(x[c], obj[c]) || x
				// otherwise, the object property can simply be assigned to the current node as the target object and returned
				: Object.assign(x, { [c]: obj[c] })
	// node to be used as 'x' which all properties are reduced into
	, isNode ? node : document.createElement(node));
}
```
