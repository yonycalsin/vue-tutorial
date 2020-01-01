# Components
Component (Component) is the best implementation method of front-end on single page application (SPA). All functional modules are decomposed into individual components, each component has independent scope and can also communicate with each other.



# Understanding single page applications (SPA)
Jumping between traditional pages is achieved by refreshing and re-rendering a page. During the rendering process, external resource files must be loaded. The page is rendered in the server through a series of life cycles. In this process, It will directly affect the loading speed of the page due to hardware issues such as network speed. To solve this problem, the front end introduced the concept of components in a new design mode. Jumping between pages has become a switch between components. Reload the entire page without having to consider the life cycle of the page, and replace it with the component life cycle, which greatly improves performance.

# Vue component implementation
## Global components
```html
    <div id="app">
        <!--Use of components-->
        <global-component></global-component>
    </div>
```
```javascript
    //Component definition Vue.component (component name, (template))
    Vue.component('global-component', {
        template: '<h1>Global component</h1>'
    })

    var vm = new Vue({
        el: '#app'
    })
```
The final rendering effect
```html
    <div id="app">
        <h1>Global component</h1>
    </div>
```
## Local component
```html
    <div id="app">
        <!--Use of components-->
        <private-component></private-component>
    </div>
```
```javascript
    //Component definition Vue.component (component name, (template))
    var vm = new Vue({
        el: '#app',
        components:{
            'private-component': {
                template: '<h1>局部组件</h1>'
            }
        }
    })
```
The final rendering effect
```html
    <div id="app">
        <h1>Local component</h1>
    </div>
```
## component is a separate scope
Each component has a separate scope
```html
    <div id="app">
        <p>{{count}}</p>
        <component1/>
    </div>    
```
```javascript
    var vm = new Vue({
        el: '#app',
        data: {
            count: 10
        },
        methods: {
            increment: function(){
                this.count += 1;
            }
        },
        components:{
            'component1': {
                template: '<button v-on:click="increment">{{ count }}</button>',
                data: function(){
                    //In the component data must be function and return an object
                    return {
                        count: 0
                    }
                },
                methods: {
                    increment: function(){
                        this.count += 1;
                    }
                }
            }
        }
    })
```
The rendering result is
```html
    <div id="app">
        <p>10</p>
        <!--
            This button increments by 1 each time the p label is always 10
             The reason is that the scope of the component is separate
        -->
        <button>0</button>
    </div>    
```


## Use is in special HTML structure
For example, in the drop-down list (select) element, the child element must be option, then use is when using the component
```html
    <div id="app">
        <select>
            <option is="privateOption"></option>
        </select>
    </div>
```
```javascript
    var vm = new Vue({
        el: '#app',
        components: {
            'privateOption': {
                template: '<option value=1>1</otpion>'
            }
        }
    })
```
Rendering results
```html
    <div id="app">
        <select>
            <option value="1">1</option>
        </select>
    </div>
```

## Dynamic component-: is
```html
<div id="app" style="display: none;">
    <input type="button" value="changeLight" @click="changeLight" />
    <br/>
    <p :is="show"></p>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            show: 'red',
        },
        methods:{
            changeLight: function(){
                this.show = this.show == 'red' ? 'green' : 'red';
            }
        },
        components: {
            red: {
                template: '<h1>Red</h1>'
            },
            green: {
                template: '<h1>Green</h1>'
            }
        }
    })
</script>
```

## Component Properties
The properties of the component must be declared before use, props: ['property name' ...]
```html
    <div id="app">
        <!--Use of components-->
        <private-component title="Component properties" :text="mess"></private-component>
    </div>
```
```javascript
    // Component definition Vue.component (component name, (template))
    var vm = new Vue({
        el: '#app',
        data: {
            mess: '-Dynamic attribute'
        }
        components:{
            'private-component': {
                template: '<h1>{{title + text}}</h1>',
                props: ['title', 'text']
            }
        }
    })
```
The final rendering effect
```html
    <div id="app">
        <h1>Component Properties-Dynamic Properties</h1>
    </div>
```
## Component Custom Events
The difference with component properties is <component name v-bind: property name = "">, the property name must be declared in the component before use: props: ['property name']
Custom event: <component name v-on: custom event name = "">, the custom event name does not need to be declared, it is directly triggered by $ emit ()
```html
    <div id="app">
        <p>{{total}}</p>
        <incrementTotal v-on:count="incrementTotal"></incrementTotal>
    </div>
```
```javascript
    var vm = new Vue({
        el: '#app',
        data: {
            total: 0
        },
        methods: {
            incrementTotal: function(){
                this.total += 1;
            }
        },
        components: {
            'incrementTotal': {
                template: '<input type="button" @click="incrementTotal" value="Total" />',
                data: function(){
                    return {
                        total: 0
                    }
                },
                methods: {
                    incrementTotal: function(){
                        this.total += 1;
                        this.$emit('count')
                    }
                }
            }
        }
    })
```

## slot Distributing Content
Vue components cover rendering by default. To solve this problem, Vue proposed slot distribution

```html
    <div id="app">
        <component1>
            <h1>Sam</h1>
            <h1>Lucy</h1>
        </component1>
    </div>
```
```javascript
    Vue.component('component1', {
        template: `
            <div>
                <h1>Tom</h1>
                <slot></slot>
            </div>
        `
    })
```
The final rendering effect
```html
    <div id="app">
        <component1>
            <h1>Tom</h1>
            <!--
               If there is no <slot> </ slot> in the template defined by the component, the following two h1 tags will not exist
               In other words, <slot> </ slot> = <h1> Sam </ h1> <h1> Lucy </ h1>
               <slot> </ slot> can be placed on <h1> Tom </ h1>
            -->
            <h1>Sam</h1>
            <h1>Lucy</h1>
        </component1>
    </div>
```

### named slot
If you want to place different child elements in the component in different places, add an attribute slot = "name" to the child element, and then use the name to correspond to the position when the component is defined <slot name="name"></slot>，Other child elements without slot attributes will be uniformly distributed to <slot></slot> inside
```html
    <div id="app">
        <component1>
            <h1>Sam</h1>
            <h1 slot="lucy">Lucy</h1>
        </component1>
    </div>
```
```javascript
    Vue.component('component1', {
        template: `
            <div>
                <slot name="lucy"></slot>
                <h1>Tom</h1>
                <slot></slot>
            </div>
        `
    })
```
The final rendering effect
```html
    <div id="app">
        <component1>
            <!--<slot name="lucy"></slot> = <h1 slot="lucy">Lucy</h1>-->
            <h1>Lucy</h1>
            <h1>Tom</h1>
            <!--All other child elements without slot attributes will be distributed to the <slot> </ slot> tag-->
            <h1>Sam</h1>
        </component1>
    </div>
```

## Template writing
```html
	<template id="component1">
		<div>
		    <input type="text" v-model="name"/>
		    <p>{{name}}</p>			
		</div>
	</template>

	<div id="app">
		<component1/>
	</div>     
```
```javascript
    var vm = new Vue({
        el: '#app',
        components: {
            'component1': {
                template: '#component1',
                data: function(){
                    return {name: 'Tom'};
                }
            }
        }
    })
```