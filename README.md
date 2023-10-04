# JSUI
Base methods for generating and manipulating HTML with vanilla javascript
O: create an element | P: select, modify and return element | U: select, modify and return group of elements

```
var P = function (selector, obj = {}) {
	return Object.keys(obj).length
		? document.querySelector(selector)
		: O(document.querySelector(selector), obj, true)
}

var U = function (selector, obj = {}) {
	return Object.keys(obj).length === 0
		? document.querySelectorAll(selector)
		: Array.from(document.querySelectorAll(selector)).forEach(x =>
			O(x, obj, true)
		)
}

var O = function (node, obj = {}, h = false) {
	return Object.keys(obj).reduce((x, c) =>
		Array.isArray(obj[c])
			? obj[c].forEach(y => x.appendChild(y)) ?? x
			: typeof obj[c] === 'object'
				? null & Object.assign(x[c], obj[c]) || x
				: Object.assign(x, { [c]: obj[c] })
	, h ? node : document.createElement(node));
}
```

#Example

```
document.body.appendChild(
	O('div', {
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
		this_name_doesnt_matter_just_the_data_type: [
			O('h1', {
				innerHTML: 'Welcome to JSUI',
				id: 'title',
				color: '#222222'
			}),
			O('p', {
				innerHTML: 'This library will remain as simple as possible to decrease the amount of characters I have to type while manipulating the DOM with vanilla JavaScript.',
				style: {
					color: '#444444'
				}
			}),
			O('button', {
				innerHTML: 'Click to do stuff',
				style: {
					marginBottom: '10px'
				},
				onclick: function () {
					U('div', {
						style: {
							color: '#c30010',
							textDecoration: 'underline'
						}
					});
					P('#title').innerHTML = this.dataset.some_stored_data;
				},
				dataset: {
					some_stored_data: 'Welcome to Vanilla JavaScript',
				}
			})
		]
	})
);
```
