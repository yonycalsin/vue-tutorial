# Instance element el
The instance element refers to the container element compiled when Vue is instantiated, or the element container that Vue acts on.
``` html
    <div id="app"></div>
```
``` javascript
    var vm = new Vue({
        el: '#app'
    })
```
You can also specify other selectors for instance elements
``` html
    <div class="app"></div>
```
``` javascript
    var vm = new Vue({
        el: '.app'
    })
```
There can be multiple instance elements
``` html
    <div id="app1"></div>
    <div id="app2"></div>
```
``` javascript
    var vm = new Vue({
        el: '#app1'
    })
    var vm = new Vue({
        el: '#app2'
    })    
```
If there are multiple identical instance elements, only the first one will take effect
``` html
    <div id="app"></div>
    <!--This is only for normal html output and will not be compiled by Vue-->
    <div id="app"></div>
```
``` javascript
    var vm = new Vue({
        el: '#app'
    })
```
You can also specify no instance elements during instantiation and manually mount them later with $ mount ()
``` html
    <div id="app"></div>
```
``` javascript
    var vm = new Vue({
        //option
    })

    vm.$mount("#app");
    
```
You can get instance elements through instances
``` javascript
    var vm = new Vue({
        el: '#app'
    })

    console.log(vm.$el)
```

# Data object data
Acts as the M (Model) data model layer in the MVVM pattern, which is more reflected in mapping the M layer to the V layer
``` html
    <div id="app">
        <!--The result is: text-->
        <span>{{key1}}</span>
        <!--The result is: 11-->
        <span>{{key2 + key3}}</span>
        <!--The result is: key4_1-->
        <span>{{key4.key4_1}}</span>
        <!--The result is: {"key5_1": "key5_1"}-->
        <span>{{JSON.stringify(key5[0])}}</span>
    </div>
```
``` javascript
    var array = [{key5_1: "key5_1"}];
    var vm = new Vue({
        el: '#app',
        data: {
            key1: 'text',
            key2: 1,
            key3: 10,
            key4: {
                key4_1: 'key4_1'
            },
            key5: array
        }
    }
```
Can get data objects through instances
``` javascript
    var vm = new Vue({
        el: '#app',
        data: {}
    })

    console.log(vm.$data)
```

# Event handler methods
Elements can be bound via v-on: event || @event, this in the event points to instance vm by default
``` html
    <div id="app">
        <input type="button" @click="count += 1" value="Listening for events">
        <span>{{count}}</span>
    </div>
```
``` javascript
    var vm = new Vue({
        el: '#app',
        data: {
            count: 0
        }
    })
```
The above situation is only suitable for very simple processing, complex processing may need to be handled separately. The above situation is only suitable for very simple processing, and complex processing may need to be handled separately.
``` html
    <div id="app">
        <input type="button" value="confirm" @click="counter"/>
        <p>Confirm button was clicked {{ counter }} Times.</p>
    </div>
```
``` javascript
    var vm = new Vue({
        el: '#app',
        data: {
            count: 0
        },
        methods: {
            counter: function(){
                this.count += 1;
            }
        }
    })
```
In js events, there will be an event object by default. Vue inherits the secondary encapsulation of the event object on the event, renames it to $ event, and passes it by default in the event.
``` html
    <div id="app">
        <input type="button" @click="eventer" count="1" value="event Object">
        <span>{{count}}</span>
    </div>
```
``` javascript
    var vm = new Vue({
        el: '#app',
        data: {
            count: 0
        },
        methods: {
            eventer: function(event){
                var _count = event.target.getAttribute('count') * 1;
                this.count += _count;
                event.target.setAttribute('count', _count);
            }
        }
    })
```
In many cases, you need to pass parameters. Vue can also pass parameters in events.
``` html
    <div id="app">
        <input type="button" @click="parameter(10, $event)" value="Event passing parameters">
        <span>{{count}}</span>
    </div>
```
``` javascript
    var vm = new Vue({
        el: '#app',
        data: {
            count: 0
        },
        methods: {
            parameter: function(n, event){
                this.count += n;
                event.target.setAttribute('disabled', true);
            }
        }
    })
```
[Event effect preview](https://dk-lan.github.io/vue-erp/VueBasic/VueBasicOptions/methods.html)

# Computed property computed
computed mainly operates on the attributes of data, the pointer of this points to the instance vm by default
``` html
    <p>{{reversedMessage}}</p>
```
``` javascript
    data: {
        message: 'ABC'
    },
    computed: {
        reversedMessage: function(){
            return this.message.split('').reverse().join('')
        }
    }
```
The rendering result is
``` html
    <p>CBA</p>
```
There are two properties in computed properties by default, one is get, we call it getter, and the other is set, we call it setter, so it can also be written like this:
``` javascript
    data: {
        message: 'ABC'
    },
    computed: {
        reversedMessage: {
            get: function(){
                return this.message.split('').reverse().join('')
            }
        }
    }
```
When we call ((reversedMessage)) in the V layer, reversedMessage.get () will be triggered automatically.

The logic of the setter is the same. When the corresponding property is changed, the set method is automatically triggered.
``` html
    <div id="app">
        <!--Called fullName.get ()-->
        <p>{{fullName}}</p>
        <input type="text"  v-model="newName">
        <!--The changeName event changes the value of fullName, so fullName.set () is triggered automatically-->
        <input type="button" value="changeName" @click="changeName">
    </div>
```
``` javascript
    var vm = new Vue({
        el: '#app',
        data: {
            firstName:'DK',
            lastName: 'Lan',
            newName: ''
        },
        computed: {
            fullName:{
                get: function(){
                    return this.firstName + '.' + this.lastName
                },
                set: function(newValue){
                    this.firstName = newValue
                }
            }
        },
        methods: {
            changeName: function(){
                this.fullName = this.newName;
            }
        }
    })
```
Vue has a dependency cache based on the corresponding property on the getter, which means that if the same property is called multiple times, get will only be executed once. The event will be called every time it is triggered, of course, it will be called again when the property value is changed.
``` html
    <div id="app">
        <!--fullName.get is only called一次-->
        <p>{{fullName}}</p>
        <p>{{fullName}}</p>
        <p>{{fullName}}</p>
        <!--ChangeName is called every click-->
        <input type="button" value="changeName" @click="changeName('Vue')">
    </div>
```
``` javascript
    var vm = new Vue({
        el: '#app',
        data: {
            firstName:'DK',
            lastName: 'Lan',
            newName: ''
        },
        computed: {
            fullName:{
                get: function(){
                    return this.firstName + '.' + this.lastName
                },
                set: function(newValue){
                    this.firstName = newValue
                }
            }
        },
        methods: {
            changeName: function(txt){
                this.newName = txt;
                //If you change the value of this.fullName here, the corresponding getter will be triggered automatically again
            }
        }
    })
```

# Listening event
Vue Provides a listener for a single property, which is automatically triggered when the property changes. Improper use of this item will affect performance, so use it with caution.
```html
    <input type="text" v-model="a">
    <p>Old value:{{aOldVal}}</p>
    <p>New value:{{aNewVal}}</p>    
```
```javascript
    {
        data: {
            a: 1
        },
        watch: {
            a: function (newVal, oldVal) {
                //Automatically trigger this method
                console.log('new: %s, old: %s', newVal, oldVal)
            },
        }    
    }
```
You can also put methods in the data object
```javascript
    {
        data: {
            a: 1,
            changeA: function (newVal, oldVal) {
                //Automatically trigger this method
                console.log('new: %s, old: %s', newVal, oldVal)
            }
        },
        watch: {
            a: 'changeA'
        }    
    }
```
The difference between watch and compute:
1. computed creates new attributes, watch listens to existing attributes of data
2.Compute will generate dependency cache
3. When watch listens to computed, watch is invalid in this case and only triggers computed.setter
```javascript
    {
        computed: {
            a: {
                get: function(){
                    return '';
                },
                set: function(newVal){
                    //Will trigger this
                    console.log('set val %s', newVal);
                }
            }				 
        },
        watch: {
            a: function(){
                //Will not be triggered
                console.log('watch');
            }
        }    
    }
```