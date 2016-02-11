#Extend Object

I use [Lodash](https://lodash.com) in my development for its ease of integration and the powerful utilities it offers.

Lodash provides a function [assignIn](https://lodash.com/docs#assignIn) that I use for overriding library options.

I know there are other ways of doing this, but since I already have Lodash included, I use it.

```js
var appObj = {
  // Whatever you're going to need for your function
};

var optionObj = {
  width: 200,
  content: "Lorem ipsum dolor sit amet..."
};

var fncName = (data, opts) {
  var defaultOpts = {
    width: 100,
    height: 200,
    title: "Something Fun"
  };

  var options = _.assignIn(defaultOpts, opts);

  // Whatever the rest of your function would normally do with the data
}

fncName(appObj, optionsObj);
```

Your new `options` variable should now look like this

```js
options: {
  width: 200,
  height: 200,
  title: "Something Fun",
  content: "Lorem ipsum dolor sit amet..."
}
```

Congratulations you just passed your new options to your function!

_Note: this can also be done with [jQuery](http://api.jquery.com/jQuery.extend/) since v1.1.4 using `$.extend(obj2, obj1);`_
