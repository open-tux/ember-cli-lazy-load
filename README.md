# Ember-ClI-Lazy-Load

This README outlines the details of collaborating on the Ember-CLI-Lazy-Load addon [here](https://github.com/B-Stefan/ember-cli-lazy-load-example) you find an [exmaple app](https://github.com/B-Stefan/ember-cli-lazy-load-example) that use this addon: 

## Installation

* `ember install ember-cli-lazy-load` 

## Getting started 


1. Configure your bundles in config/bundes.js 

```javascript 

        //index: {
        //    files: [
        //        "**/templates/index.js",
        //        "**/controllers/index.js",
        //        "**/components/my-item/**.js"
        //    ],
        //
        //    routes: ["index", "..."]
        //},
        //about: {
        //    files: [
        //        "**/templates/about.js",
        //        "**/controllers/about.js",
        //        "**/components/my-cat/**.js"
        //    ],
        //    dependencies: ["index"],
        //    routes: ["about", "more routes for this bundle "]
        //}
```


2. Modify your config/environment.js and include there the bundle files 

```
    var bundles = require("./bundles")
    module.exports = function(environment) {
      var ENV = {
        bundles: bundles(environment),
```

3. Modify your ember-cli-build.js to use the custom bundle build flow. 

```
var EmberApp = require("ember-cli-lazy-load/ember-app");
var bundles = require("./config/bundles")();

module.exports = function(defaults) {

  var app = new EmberApp(defaults, {
    // Add options here
    bundles: bundles
  }


````

4. Add Mixin to your routes 

To enable the automatic loading of the bundles when the user change the route add the mixin to your route: 

```

import Ember from "ember";
import LazyRouteMixin from 'ember-cli-lazy-load/mixins/lazy-route';

export default Ember.Route.extend(LazyRouteMixin,{

   
});

```

if you already override the beforeModel function pleas ensure that you excecute the super call before the rest and wait for the result. `

```


import Ember from "ember";
import LazyRouteMixin from 'ember-cli-lazy-load/mixins/lazy-route';

export default Ember.Route.extend(LazyRouteMixin,{

  beforeModel: function(transition, queryParams){
          return this._super(transition,queryParams).then(()=>{
              console.log("code after the bundle load");
          });
      }

   
});


``
 
5. Done 


## Services / Mixin

###bundle-loader Service

The bundle loader provide the ability to load bundles. 

* loadBundle(bundleName:string):Promise() - Load the bundle and there dependencies 


###lazy-route Mixin 
The mixin override the beforeModel(transition, queryParams) function. 
If you use this function already in you route be sure that you call first this method and wait for the promise!  


* findBundleByRouteName(routeName:string):string - get the bundle for the route, undefined if no bundle found 
* beforeModel(transition, queryParams):Promise  - loads the bundle if there is one for this route 


## Running

* `ember server`
* Visit your app at http://localhost:4200.

## Running Tests

* `npm test` (Runs `ember try:testall` to test your addon against multiple Ember versions)
* `ember test`
* `ember test --server`

## Building

* `ember build`

For more information on using ember-cli, visit [http://www.ember-cli.com/](http://www.ember-cli.com/).
