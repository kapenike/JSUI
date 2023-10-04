# JSUI
Base methods for generating and manipulating HTML with vanilla javascript

```
var $ = function (selector) {
	return document.querySelector(selector);
}

var O = function (node, obj = []) {
	return [document.createElement(node)].map(elem =>
		Object.keys(obj).reduce((x, c) =>
			Array.isArray(obj[c])
				? obj[c].forEach(y => x.appendChild(y)) ?? x
				: typeof obj[c] === 'object'
					? Object.assign(x[c], obj[c]) & null || x
					: Object.assign(x, { [c]: obj[c] })
		, elem)
	)[0];
}
```
