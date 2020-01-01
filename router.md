# Routing
Via URL mapping to the corresponding function implementation, Vue's routing use must first be introduced into vue-router.js

## Getting Started with Basic Routing
Define component
```javascript
    const Foo = { template: '<div><h1>Foo Content</h1></div>' };
    const Bar = { template: '<div><h1>Bar Content</h1></div>' };
```
Define routing rules
```javascript
    // Each route should map a component. Where "component" can be a custom component
    // When url is http: //localhost/index.html#/foo Page will render component Foo
    // When url is http: //localhost/index.html#/bar Page will render component Bar
    const routes = [
        {path: '/foo', component: Foo},
        {path: '/bar', component: Bar}
    ]
```
use
```html
    <div id="app">
        <h1>Hello VueRouter</h1>
        <p>
			<!-- Use the router-link component to navigate. -->
			<!-- Specify the link by passing in the to attribute.-->
			<!-- <router-link> is rendered as a `<a>` tag by default -->
            <!-- The attribute `to` corresponds to the` href` attribute of the generated <a> `tag-->
            <router-link to="/foo">Foo</router-link>
            <router-link to="/bar">Bar</router-link>
        </p>
        <!--Route matching components are rendered here-->
        <router-view></router-view>
    </div>
```
```javascript
    const router = new VueRouter({
        routes // (Abbreviation) equivalent to routes: routes
    })

    new Vue({
        router
    }).$mount('#app')
```

## Routing parameter
```html
    <div id="app">
        <h1>Hello VueRouter</h1>
        <p>
            <router-link to="/user/1">User1</router-link>
            <router-link to="/user/2">User2</router-link>
        </p>
        <router-view></router-view>
    </div>
```
Get parameters through the object $ route.params
```javascript
    const User = { template: '<div><h1>{{$route.params.userid}}</h1></div>' };

    const routes = [
        {path: '/user/:userid', component: User}
    ]    

    const router = new VueRouter({
        routes
    })

    new Vue({
        router
    }).$mount('#app')
```

## Nested routes
```html
    <div id="app">
        <h1>Hello VueRouter</h1>
        <p>
            <router-link to="/floor1">First floor</router-link>
        </p>
        <router-view></router-view>
    </div>
```
```javascript
    const Floor1 = { 
        template: `
            <div>
                <h1>First floor</h1>
                <router-link to="/floor1/floor2">Second floor</router-link>
                <router-view></router-view>
            </div>` 
    };
    const Floor2 = { template: '<div><h1>Second floor</h1></div>' };    

    const routes = [
        {
            path: '/floor1',
            component: Floor1,
            children: [{
                // floor2 will be rendered in the <router-view> of Floor1
                path: 'floor2',
                component: Floor2
            }]
        }
    ]    

    const router = new VueRouter({
        routes
    })

    new Vue({
        router
    }).$mount('#app')
```

## Programmatic Navigation
Jump routing with javascript
```html
    <div id="app">
        <h1>Hello VueRouter</h1>
        <p>
            <!--Jump with `router-link` component-->
            <router-link to="/floor1">First floor</router-link>
            <!--Programmatic navigation 1: router.replace-->
            <input type="button" value="First floor" @click="router.replace('/floor1')">
            <!--Programmatic navigation 2: router.push ()-->
            <input type="button" value="First floor" @click="router.push('/floor1')">
            <!--Programmatic navigation 3: router.push ({})-->
            <input type="button" value="First floor" @click="router.push({path: '/floor1'})">            
        </p>
        <router-view></router-view>
    </div>
```

## Named Route
Add an attribute name to the routing map to name the routing mapping rule. You can use router.push ({name: 'name'}) when programmatically navigating a route.
```html
    <div id="app">
        <h1>Hello VueRouter</h1>
        <p>
            <router-link to="/floor1">First floor</router-link>
            <!--Programmatic Navigation 4: router.push ({name: 'name'})-->
            <input type="button" value="First floor" @click="router.push({name: 'floor1'})">             
        </p>
        <router-view></router-view>
    </div>
```
```javascript
    const Floor1 = { 
        template: `
            <div>
                <h1>First floor</h1>
                <router-link to="/floor1/floor2">Second floor</router-link>
                <router-view></router-view>
            </div>` 
    };
    const Floor2 = { template: '<div><h1>Second floor</h1></div>' };    

    const routes = [
        {
            path: '/floor1',
            component: Floor1,
            name: 'floor1', //name
            children: [{
                // floor2 会被渲染在 Floor1 的 <router-view> 中
                path: 'floor2',
                component: Floor2,
                name: 'floor2' //name
            }]
        }
    ]    

    const router = new VueRouter({
        routes
    })

    new Vue({
        router
    }).$mount('#app')
```

## Named view
```html
    <div id="app">
        <h1>Hello VueRouter</h1>
        <p>
            <router-view></router-view>
            <router-view name="a"></router-view>
            <router-view name="b"></router-view>
        </p>
    </div>
```
```javascript
    const router = new VueRouter({
        routes: [
            {
                path: '/',
                components: {
                    default: {
                        template: '<h1>defalut router view</h1>'
                    },
                    a: {
                        template: '<h1>a router view</h1>'
                    },
                    b: {
                        template: '<h1>b router view</h1>'
                    }
                }
            }
        ]
    })

    new Vue({
        el: '#app',
        router
    })
```