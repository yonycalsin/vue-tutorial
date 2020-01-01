# Custom instructions
In some cases, the instructions provided to us by Vue are very insufficient. For example, there is an input box that needs to pop up the date control when clicked. This function is not supported by Vue, but Vue supports our custom instructions.

There are many types of custom instructions and component definitions. There are also global and local instructions.

## Global instructions
```html
    <div id="app">
        <!--A custom instruction v-global is used here-->
        <input type="text" value="" v-global />
    </div>
```
```javascript
   // Register a global custom instruction v-global, without prefixing the instruction name v-
   // When the element uses the v-global instruction, the corresponding function will be triggered
   // parameter element: the element using the directive
    Vue.directive('global',  function(element){
        // Insert triggering hook function by default
        element.value = "World Peace";
        element.focus();
    })

    var vm = new Vue({
        el: '#app'
    })
```

## Local instruction
```html
    <div id="app">
        <!--A custom instruction v-private is used here-->
        <input type="text" value="" v-private />
    </div>
```
```javascript
    var vm = new Vue({
        el: '#app',
        directives: {
            //Register a local instruction private, no need to prefix the instruction name v-
            // When the element uses the v-private instruction, the corresponding function will be triggered
            // Parameter element: the element using the directive
            private: function(element){
                element.style.background = '#ccc';
                element.value = "World Peace";
            }
        }
    })
```

## Hook function
Hook functions can be understood as the life cycle of instructions
```html
    <div id="app">
        <!--A custom instruction v-global is used here-->
        <input type="text" v-model="text" v-demo="{color:'red'}">
    </div>
```
```javascript
    Vue.directive('demo', {
        // Called when the bound element is inserted into the parent node
        // This method is triggered by default Vue.directive ('demo', function (element) ())
        // fired after bind
        // parameter element: the element using the instruction
        // Parameter binding: Use the attribute object of the directive
        // parameter vnode: the entire Vue instance
        inserted: function(element, binding, vnode){
            console.log('inserted');
        },
        // Only called once, called when the instruction is first bound to the element,
        // Use this hook function to define an initialization action that is performed once during binding
        // Triggered before inserted
        bind: function(element, binding, vnode){
            console.log('bind');
            element.style.color = binding.value.color
        },
        // Called when the template containing the bound element is updated, regardless of whether the bound value changes
        update: function(element, binding, vnode){
            console.log('update');
        },
        //Called when the bound element's template completes an update cycle.
        componentUpdated: function(element, binding, vnode){
            console.log('componentUpdated');
        }
    })

    var vm = new Vue({
        el: '#app',
        data:{
            text: 'Hook function'
        }
    })
```

## Case: Custom Date Control
This is implemented using boostrap's datepicker plugin
```html
    <div id="app">
        <!--Use the datepicker plugin directly in the jQuery environment-->
        <input type="text" id="datepicker" data-date-format="yyyy-mm-dd"/>
        <!--Use Vue custom directive v-datepicker-->
        <input type="text" v-datepicker data-date-format="yyyy-mm-dd"/>
        <input type="button" value="Save" @click="save">
        <span>{{dataform.birthday}}</span>
    </div>
```
```javascript
    //Before using Vue, the datepicker plugin was used in the jQuery environment.
    $('#datepicker').datepicker();

    //Use Vue custom directive v-datepicker
    Vue.directive('datepicker', function(element, binding, vnode){
        // data = dataform.birthday
        $(element).datepicker({
            language: 'zh-CN',
            pickTime: false,
            todayBtn: true,
            autoclose: true
        }).on('changeDate', function(){
            // Two-way binding v-model is invalid because the value is not entered manually in input
            // So you need to manually change the data model of the instance
            var data = $(element).data('model');
            if(data){
                // datas = ['dataform', 'birthday']
                var datas = data.split('.');
                //context = vm
                var context = vnode.context;
                //Automatically add loop properties
                datas.map((ele, idx) => {
                    //The last attribute is directly assigned
                    if(idx == datas.length - 1){
                        context[ele] = element.value
                    } else {
                        //Adding attributes dynamically
                        context = context[ele]
                    }
                })
            }
        })
    })

    var vm = new Vue({
        el: '#app',
        data: {
            dataform: {}
        },
        methods: {
            save: function(){
                // Update dataform with $ set
                // More use of $ set is introduced below
                this.$set(this.dataform)
            }
        }
    })
```

## $ set Introduction
When the structure of the instance object data is set first, for example: data: {dataform: {}}, when you want to add an attribute username later, this username will not be automatically bound to the view, so $ set (original object, new Property name, property value) to bind to the view
```html
    <div id="app">
        <input type="button" value="set" @click="set">
        <span>{{dataform.username}}</span>
    </div>
```
```javascript
    var vm = new Vue({
        el: '#app',
        data: {
            dataform: {}
        },
        methods: {
            set: function(){
                // Direct assignment is not displayed on the view
                // this.dataform.username = '123'
                // Use $ set instead to update the view.
                this.$set(this.dataform, 'username', '123')
            }
        }
    })
```