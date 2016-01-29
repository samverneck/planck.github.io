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
