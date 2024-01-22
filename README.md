# JSUI
Base methods for generating and manipulating HTML with vanilla javascript

**Create:** create an element

**Select:** select, modify and return element

**MSelect:** select, modify and return group of elements

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
		)
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

#Example

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
				color: '#222222'
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
