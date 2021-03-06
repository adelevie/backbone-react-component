# Backbone.React.Component

Backbone.React.Component is a wrapper for React.Component and brings all the power of Facebook's React to Backbone.js.

## Dependencies
* [Backbone](http://backbonejs.org/) ([jQuery](http://jquery.com/)/[Underscore](http://underscorejs.org/))
* [React](http://facebook.github.io/react/)

## How To
### Downloading and including the script
Using [Bower](http://bower.io/)
```shell
bower install backbone-react-component
```
If you're not using [Bower](http://bower.io/) download the source from the dist folder.

Include the script on your webpage (or use [RequireJS](http://requirejs.org/)/[Browserify](http://browserify.org/))
```html
...
<script type="text/javascript" src="path_to_script/backbone-react-component-min.js"></script>
...
```

### Using Backbone.React.Component
It follows all the principles behind [React.Component](http://facebook.github.io/react/docs/component-api.html), though it binds models to the component's props and also automatically mounts the component into the component's $el.

#### Basic usage
```js
/** @jsx React.DOM */
var MyComponent = Backbone.React.Component.extend({
  render: function () {
    return <div>{this.props.test}</div>;
  }
});
var model = new Backbone.Model();
var newComponent = new MyComponent({
  el: $('body'),
  model: model
});
model.set('test', 'Hello world!');
```
The Component will listen to any model changes, making it automatically refresh using React's algorithm.

#### Mounting the component
```js
newComponent.mount();
```

#### With a collection
```js
var newComponent = new MyComponent({
  el: $('body'),
  collection: new Backbone.Collection([{helloWorld: 'Hello world!'}])
});
```
```js
var MyComponent = Backbone.React.Component.extend({
  createEntry: function (entry) {
    return <div>{entry.helloWorld}</div>;
  },
  render: function () {
    return <div>{this.props.collection.map(this.createEntry())}</div>;
  }
});
```

### With multiple models and collections
```js
var newComponent = new MyComponent({
  el: $('body'),
  model: {
    firstModel: new Backbone.Model({helloWorld: 'Hello world!'}),
    secondModel: new Backbone.Model({helloWorld: 'Hello world!'})
  },
  collection: {
    firstCollection: new Backbone.Collection([{helloWorld: 'Hello world!'}]),
    secondCollection: new Backbone.Collection([{helloWorld: 'Hello world!'}])
  }
});
```
```js
var MyComponent = Backbone.React.Component.extend({
  createEntry: function (entry) {
    return <div>{entry.helloWorld}</div>;
  },
  render: function () {
    return (
      <div>
        {this.props.firstModel.helloWorld}
        {this.props.secondModel.helloWorld}
        {this.props.firstCollection.map(this.createEntry())}
        {this.props.secondCollection.map(this.createEntry())}
      </div>;
    )
  }
});
```

### API
Besides inheriting all the methods from [React.Component](http://facebook.github.io/react/docs/component-api.html) and [Backbone.Events](http://backbonejs.org/#Events) you can find the following methods:

#### new Backbone.React.Component(props)
props is a namespace and may contain el, model and collection properties. Model and collection properties may be multiple by passing a namespace.

#### $
Inspired by Backbone.View, it's a shortcut to this.$el.find method.

#### getCollection()
Gets the collection from the component's owner.

#### getModel()
Gets the model from the component's owner.

#### getOwner()
Gets the component owner (greatest parent component).

#### mount([el = this.el], [onRender])
* el (DOMElement)
* onRender (Callback)
Mounts the component into the DOM and sets it has rendered (this.isRendered = true).

#### unmount()
Unmounts the component. Throws an error if the component doesn't unmount successfully.

#### remove()
Stops component listeners, unmounts the component and then removes the DOM element.

### Examples
* [Typewriter](https://rawgithub.com/magalhas/backbone-react-component/master/examples/typewriter/index.html)

### TO DO
* Improve models/collections requests error handling
* Improve the way how to detect bulk inserts (collection.add([])) to avoid extra calls to setPropsCollection
* Any ideas?