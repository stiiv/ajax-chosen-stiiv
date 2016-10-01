# ajax-chosen-stiiv
updated version for chosen library v1+ of ajax-chosen made by https://github.com/meltingice 

- original documentation at https://github.com/meltingice/ajax-chosen/edit/master/README.md
- NOTE: this is a copied and edited version for documentation you can find on link above

## How to Use

This plugin exposes a new jQuery function named `ajaxChosenStiiv` that we call on a `select` element. The first argument consists of the options passed to the jQuery $.ajax function. The `data` parameter is optional, and the `success` callback is also optional.

The second argument is a callback that tells the plugin what HTML `option` elements to make. It is passed the data returned from the ajax call, and you have to return an array of objects for which each item has a `value` property corresponding to the HTML `option` elements' `value` attribute, and a `text` property corresponding to the text to display for each option. In other words:

	[{"value": 3, "text": "Ohio"}]

becomes:

	<option value="3">Ohio</option>

or for grouping:

	[{
		group: true,
		text: "Europe",
		items: [
			{ "value": "10", "text": "Stockholm" },
			{ "value": "23", "text": "London" }
		]
	},
	{
		group: true,
		text: "Asia",
		items: [
			{ "value": "36", "text": "Beijing" },
			{ "value": "20", "text": "Tokyo" }
		]
	}]

becomes:

        <optgroup label="Europe">
            <option value="10">Stockholm</option>
            <option value="23">London</option>
        </optgroup>
        <optgroup label="Asia">
            <option value="36">Beijing</option>
            <option value="20">Tokyo</option>
        </optgroup>

### Options

There are some additional ajax-chosen-stiiv specific options you can pass into the first argument to control its behavior.

* `minTermLength`: minimum number of characters that must be typed before an ajax call is fired
* `afterTypeDelay`: how many milliseconds to wait after typing stops to fire the ajax call
* `jsonTermKey`: the ajax request key to use for the search query (defaults to `term`)
* `keepTypingMsg`: encourage user to keep typing while `minTermLength`option has been reached message (defaults to `Keep typing...`)
* `lookingForMsg`: looking for "term" message (defaults to `Looking for`)

## Example Code

``` js
$("#example-input").ajaxChosen({
	type: 'GET',
	url: '/ajax-chosen/data.php',
	dataType: 'json'
}, function (data) {
	var results = [];
	
	$.each(data, function (i, val) {
		results.push({ value: val.value, text: val.text });
	});
	
	return results;
});
```
To have the results grouped in `optgroup` elements, have the function return a list of group objects instead:

``` js
$("#example-input").ajaxChosenStiiv({
	type: 'GET',
	url: '/ajax-chosen/grouped.php',
	dataType: 'json'
}, function (data) {
	var results = [];

	$.each(data, function (i, val) {
		var group = { // here's a group object:
			group: true,
			text: val.name, // label for the group
			items: [] // individual options within the group
		};

		$.each(val.items, function (i1, val1) {
			group.items.push({value: val1.value, text: val1.text});
		});

		results.push(group);
	});

	return results;
});
