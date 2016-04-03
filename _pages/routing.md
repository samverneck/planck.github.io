---
layout: page
title: Routing
permalink: /routing/
order: 1
share: false
---

Planck app supports REST API by default. Fastest way to create routes to your resource is using ```resource``` helper. It creates GET/POST/PUT/PATCH/DELETE routes and links them to your controller. You also can specify ONLY block with http methods to use with this resource. Resources can contain sub-resources.
Other way to specify route is to use ```route``` helper and provide all options: path, controller, methods. You can include your ```route``` helpers inside ```resource```.

Here is an example of resources/routes, can be handled by Planck:

```javascript
app = await new App();

class MyRouter extends Router.RouterHTTP{
    constructor(resource, route){
        super();
        resource('users', () => {
            resource('friends', ['update']);
            route('admin', 'admin.getInfo');
            route('/adminFromUsers');
        });
        resource('groups', () => {
            resource('posts', ['create', 'read'], () => {
                resource('comments');
                resource('likes', ['read']);
            });
        });

        route('/admin', 'admin.getInfo');
        route('/my/password', 'users.password', ['get', 'post']);

        route.get('/my/info', 'users.info');
        route.post('/my/info', 'users.info');
        route.put('/my/info', 'users.info');
        route.patch('/my/info', 'users.info');
        route.delete('/my/info', 'users.info');

        route(); //also fail
        route('/fail');
        route('/fail2', 'onlyControllerName');
    }
}
await app.use(MyRouter);
```

### Router structure
Router is a class, extended from Planck's ```RouterHTTP```. In constructor of this class you should describe all routes, used in application. When Planck constructs your router, it passes some helpers as parameters, which you can use to describe your routes. This parameters are position agnostic, you can write their names in constructor's arguments it in any order. These parameters are:
* Resource - function to describe REST resource. It takes 1 required and 2 optional parameters:
  * name (String): resource name
  * methods (Array of String): array with names of HTTP methods to generate within this resource. HTTP methods are always associates with controllers methods:
    * GET /resource - ```readList```
	* GET /resource/:id - ```read```
	* POST /resource - ```create```
	* PUT/PATCH /resource/:id - ```update```
	* DELETE - ```delete```
  * callback (Function)
* Route
  * url
  * controller/method
  * methods

  Route also
* rawRouter - plain [Express](http://expressjs.com/) app. You can use it if you want to use something specific, like [Passport](http://passportjs.org/) authorization.
