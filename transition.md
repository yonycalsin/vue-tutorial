# Transition effect
The switching of components in the SPA has a rigid hidden display feeling. In order to better user experience and let the components fade in and out when switching, Vue provides a special component transition.

## Application scenario of filter effect
- Conditional rendering (using v-if)
- Conditional display (using v-show)
- Dynamic components
- Component root node

## Transition state
- enter: defines the start state of the entry transition. Takes effect when the element is inserted.
- endter-active: defines the state of the transition. Acts during the entire transition of the element and takes effect when the element is inserted.
- enter-to: Version 2.1.8 and above Defines the end state of the transition.
- leave: defines the start state of the leave transition. Takes effect when the exit transition is triggered.
- leave-active: defines the state of the transition. Acts during the entire transition of the element and takes effect immediately after the exit transition is triggered.
- leave-to: version 2.1.8 and above Defines the end state of the leave transition.

Each state is used in CSS when combined with the name attribute of the component transition. Such as `<transition name =" fade "> </ transition>`, which corresponds to `fade-` plus each state:` fade-enter`.

## CSS transition
Use the property of the component `transition`:` name`.
```html
<style type="text/css" media="screen">
    /*Initial state*/
    .fade-enter{opacity: 0;}
    /*Ready*/
    .fade-enter-active{transition: all .5s;}
    /*Has disappeared*/
    .fade-leave-active{opacity: 0; transition: all .5s;}
</style>

<div id="app">
    <input type="button" :value="show ? 'hide' : 'show'" @click="show = !show" />
    <br/>
    <transition name="fade">
        <img src="imgs/green.jpg" v-show="show" />
    </transition>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            show: true
        }
    })
</script>
```

## CSS Animation
Use the property of the component `transition`:` name`.
```html
<style type="text/css" media="screen">
    .fade-enter-active{animation: fade-in .5s;}

    .fade-leave-active{animation: fade-out .5s;}

    @keyframes fade-in{
        from{
            opacity: 0;
        }
        to{
            opacity: 1;
        }
    }

    @keyframes fade-out{
        from{opacity: 1;}
        to{opacity: 0;}
    }
</style>

<div id="app">
    <input type="button" :value="show ? 'hide' : 'show'" @click="show = !show" />
    <br/>
    <transition name="fade">
        <img src="imgs/green.jpg" v-if="show" />
    </transition>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            show: true
        }
    })
</script>
```

## Filter for initial rendering
The transition effect on the first load, using the property of the component `transition`:` appear`appear-class``appear-active-class`.
```html
<style type="text/css" media="screen">
    .fade-enter{opacity: 0;}
    .fade-enter-active{transition: all 3s;}
</style>

<div id="app">
    <transition appear appear-class="fade-enter" appear-active-class="fade-enter-active">
        <img src="imgs/green.jpg" />
    </transition>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#app'
    })
</script>
```

## River crossing effect of multiple elements
The transitions of entry and exit that take effect simultaneously cannot meet all requirements, so Vue provides a transition mode:
-in-out: The new element transitions first, and the current element transitions away after completion.
-out-in: The current element is transitioned first, and the new element transitions into the transition after completion.

Use the property of the component `transition`:` mode`.
```html
<style type="text/css" media="screen">
    .fade-enter{opacity: 0;}
    .fade-enter-active{transition: all .5s;}

    .fade-leave-active{opacity: 0; transition: all .5s;}
</style>

<div id="app">
    <fieldset>
        <legend><h3>mode = in-out</h3></legend>
        <div>
            <input type="button" :value="red ? 'green' : 'red'" @click="red = !red" />
            <br/>
            <transition name="fade" mode="in-out">  
                <img src="imgs/red.jpg" v-if="red" key="red"/>
                <img src="imgs/green.jpg" v-else key="green"/>
            </transition>           
        </div>
    </fieldset>
    <fieldset>
        <legend><h3>mode = out-in</h3></legend>
        <div>
            <input type="button" :value="flag == 1 ? 'green' : flag == 2 ? 'yellw' : 'red'" @click="flag = flag == 1 ? 2 : flag == 2 ? 3 : 1" />
            <br/>
            <transition name="fade" mode="out-in">  
                <img src="imgs/red.jpg" v-if="flag == 1" key="red"/>
                <img src="imgs/green.jpg" v-else-if="flag == 2" key="green"/>
                <img src="imgs/yellow.jpg" v-else key="yellow" />
            </transition>               
        </div>
    </fieldset> 
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            red: true,
            flag: 1
        }
    })
</script>
```

## List (v-for) transition effects
v-for uses the transition-group component to generate the list transition effect. The component provides the attribute tag to indicate that the component will be rendered into the corresponding DOM node. Other uses are the same as the transition.
```html
<style type="text/css" media="screen">
    *{list-style: none;}
    li{width: 300px; margin-bottom: 5px; padding: 10px 20px; background-color: #ccc;}

    .list-enter{opacity: 0; transform: translateX(300px);}
    .list-enter-active{transition: all .5s;}

    .list-leave-active{transition: all .5s; opacity: 0; transform: translateX(-300px);}
</style>

<div id="app">
    <p>
        <input type="button" value="AddItem" @click="addItem">
        <input type="button" value="RemoveItem" @click="removeItem">
    </p>
    <transition-group tag="ul" name="list">
        <li v-for="(item, index) in items" :key="item">Item {{index}}</li>
    </transition-group>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            items: [1,2,3]
        },
        methods: {
            randomIndex: function(){
                return  parseInt(this.items.length * Math.random());
            },
            addItem: function(){
                this.items.splice(this.randomIndex(), 0, this.items.length + 1);
            },
            removeItem: function(){
                this.items.splice(this.randomIndex(), 1);
            }
        }
    })
</script>
```

## Custom Transition Class Name
We can customize the transition class name through the following characteristics:
- enter-class
- enter-active-class
- enter-to-class (2.1.8+)
- leave-class
- leave-active-class
- leave-to-class (2.1.8+)
They take precedence over ordinary class names, which is useful for using Vue's transition system with other third-party CSS animation libraries such as Animate.css.
```html
<link rel="stylesheet" type="text/css" href="animate.css">

<div id="app">
    <button @click="show = !show">Toggle render</button>
    <transition enter-active-class="animated jello" leave-active-class="animated bounceOutRight">
        <div v-if="show"><img src="./imgs/green.jpg" /></div>
    </transition>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            show: true
        }
    })
</script>
```

## Hook function for transition effects
In addition to using CSS transition animations to implement Vue component transitions, you can also use JavaScript hook functions to directly manipulate the DOM in the hook functions. We can declare the following hooks in the property:
```html
<transition
   v-on:before-enter="beforeEnter"
   v-on:enter="enter"
   v-on:after-enter="afterEnter"
   v-on:enter-cancelled="enterCancelled"
   v-on:before-leave="beforeLeave"
   v-on:leave="leave"
   v-on:after-leave="afterLeave"
   v-on:leave-cancelled="leaveCancelled"
   ></transition>
<script type="text/javascript">
var vm = new Vue({
    el: '#app',
    methods: {
        // Transition into
        // Set component state before transition
        beforeEnter: function(el) {
            // ...
        },
        // Set component state when transition enters completion
        enter: function(el, done) {
            // ...
            done()
        },
        // Set component state after transition into completion
        afterEnter: function(el) {
            // ...
        },
        enterCancelled: function(el) {
            // ...
        },
        // Transition away
        // Set the component state before the transition leaves
        beforeLeave: function(el) {
            // ...
        },
        // Set component state when transition exit is complete
        leave: function(el, done) {
            // ...
            done()
        },
        // Set component state after transition exit is complete
        afterLeave: function(el) {
            // ...
        },
        // leaveCancelled is only used in v-show
        leaveCancelled: function(el) {
            // ...
        }
    }
})
</script>
```