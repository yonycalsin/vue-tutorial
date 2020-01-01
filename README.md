<p align="center">
  <a href="https://github.com/YoniCalsin/vue-tutorial" target="blank"><img src="https://i.ibb.co/4KHzY2q/58482acecef1014c0b5e4a1e.png" width="120" alt="Vue Tutorial Logo" /></a>
</p>

[circleci-image]: https://img.shields.io/circleci/build/github/nestjs/nest/master?token=abc123def456
[circleci-url]: https://circleci.com/gh/nestjs/nest

<p align="center">
<a href="https://github.com/yonicalsin/vue-tutorial"><img src="https://img.shields.io/spiget/stars/1000?color=brightgreen&label=Star&logo=github" /></a>
<!-- <a href="https://github.com/yonicalsin/vue-tutorial"><img src="https://img.shields.io/badge/Version-1.0.0-brightgreen?style=flat-square&logo=github" /></a> -->
<!-- <a href="https://github.com/yonicalsin/vue-tutorial"><img src="https://img.shields.io/badge/Github%20Page-Yoni%20Calsin-yellow?style=flat-square&logo=github" /></a> -->
<a href="https://github.com/yonicalsin"><img src="https://img.shields.io/badge/Author-Yoni%20Calsin-blueviolet?style=flat-square&logo=appveyor" /></a>
</p>

# Technology Point Catalog

- Meet Vue
- Know the data-driven model
- Understanding the MVVM Model
- [Template Syntax](https://github.com/YoniCalsin/vue-tutorial/blob/master/template-syntax.md)
- [Style Binding](https://github.com/YoniCalsin/vue-tutorial/blob/master/style-binding.md)
- [Basic properties when instantiating Vue](https://github.com/YoniCalsin/vue-tutorial/blob/master/basic-options.md)
- [Modifiers](https://github.com/YoniCalsin/vue-tutorial/blob/master/modifiers.md)
- [Component](https://github.com/YoniCalsin/vue-tutorial/blob/master/component.md)
- [Directive](https://github.com/YoniCalsin/vue-tutorial/blob/master/template-syntax.md#instruction)
- [Custom Instructions](https://github.com/YoniCalsin/vue-tutorial/blob/master/directive.md)
- [Animation and transition effects](https://github.com/YoniCalsin/vue-tutorial/blob/master/transition.md)
- [Routing](https://github.com/YoniCalsin/vue-tutorial/blob/master/router.md)




## Installation

Install the 1.X version first
```javascript
npm i vue
```
Create new index.html and introduce `vue.js` file
Vue is lighter than Angular, and Vue has a smaller source

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<script src="js/vue.js"></script>
	<body>
		<!--V-->
		<div id="demo">
			<p>{{name}}</p>
		</div>
	</body>
	<script>
		new Vue({
			el: "#demo",//element
			//M
			data: {
				name: "w"
			}
		})
	</script>
</html>
```
Id is usually used to bind el in new Vue, because id is unique, of course, class is also possible, but it is not recommended

## instruction
Directives, which are displayed on the sticky note with attribute values, to achieve the reuse of a certain type of method

Below is a simple comparison of Vue and Angular directives. Vue is always simpler.

| Angular instruction | Vue instruction | Vue instruction |
| - | - | - |
| ng-show | v-show ||
| hide | v-else ||
| ng-if | v-if ||
| repeat | v-for ||
| modeling | v-model ||
| click | v-on: click | @click |
| to-change | v-on: change | @change |
| keyup | v-on: keyup | @keyup |
| ng-class | v-bind: class |: class |
| ng-style | v-bind: style |: class |
| bind | v-text | `{{}}` |
| bind-html | v-html | `{{{}}}` |
| controller | no | no |
| of-app | no | no |

## Custom instructions
Vue.directive (the name of the directive, accepts an object)
### Global definition
```javascript
Vue.directive('color',{
	bind:function(){
		
	}
})
```
### Definición local
```javascript
new Vue({
	directives: {
		dirName: { //Definition directive 
	}
});
```
There are three commonly used attribute values in the object: `bind, update, unbind`, all the attributes are as follows
| Attribute Value | Usage |
|-|-|
| bind | Only called once, called when the instruction is bound to the element for the first time, this hook function can be used to define an initialization action that is performed once during the binding |
| inserted | Called when the bound element is inserted into the parent node (the parent node can be called if it exists, it does not need to exist in the document) |
| update | Called when the template containing the bound element is updated, regardless of whether the bound value changes. You can ignore unnecessary template updates by comparing the binding values before and after the update |
| componentUpdated | Called when the template containing the bound element completes an update cycle |
| unbind | Only called once, called when instruction and element unbind |

Use instructions to prefix v-

**definition:**
```javascript
Vue.directive('color',{...})
```
**use:**
```javascript
<p v-color="yellow">{{name}}</p>
```

## Built-in filter
1.x has built-in filters 2.x has removed most of them, but custom filters still have

Note that when Angular and Vue use built-in filters, the syntax for accepting parameters is different.
```javascript
//Angular syntax
{{num|currency:"￥" 1}}
//Vue syntax
{{num|currency "￥" 1}}
```

| Angular filter | Vue filter |
|-|-|
| uppercase | uppercase |
| lowercase | lowercase |
| currency | currency |
| filter | filterBy |
| limiteTo | limiteBy |
| orderBy | orderBy |
| json | json |
| None | number |
| None | date |
| None | capitalize |
| None | pluralize |
| None | debounce |

### uppercase and lowercase
Make the string all uppercase or lowercase
```javascript
//Model
message: "Wscats"
//View
{{message | uppercase}}
```

### capitalize

Capitalize the first letter
```javascript
//Model
message: "wscats"
//View
{{message | capitalize}}//Wscats
```

### currency
Make numbers into money format and decimal point
```javascript
{{message | currency '￥' "1"}}
```
parameter:
1. The first parameter is the currency symbol `String` Default value:" $ "
2. The second parameter is the number of decimal places. Default: 2

### pluralize
If there is only one parameter, the plural form simply adds an "s" to the end. If there are multiple parameters, the parameters are treated as an array of strings, corresponding to one, two, three ... plural. If there are more values than parameters, the last parameter is used
```javascript
{{message | pluralize 'item'}}
```

### debounce
1. Restrictions: need to use in @
2. Parameter: milliseconds Default: 300
3. Function: Wrap the processor and let it delay the execution of xxx ms. The default delay is 300 ms. The wrapped processor will delay at least xxx ms after the call. If it is called again before the delay ends, the delay time is reset to xxx ms.
```javascript
disappear: function(){
	document.getElementById("btn").style.display= "none";
}
<button id="btn" @click="disappear|debounce 10000">Click on me and i will disappear after 10 seconds</button>
```

### limitBy
1. Restriction: need to be used in v-for (that is, array)
2. Two parameters:
   1. The first parameter: the quantity obtained
   2. The second parameter: offset


 ```javascript
 <li v-for="item in items|limitBy 10 0">{{item}}</li>
 ```

### filterBy
1. Restriction: need to be used in v-for (that is, array)
2. Three parameters:
   1. The first parameter: the string to search
   2. The second parameter: in (optional, specify the search position)
   3. The third parameter: (optional, array format)

```javascript
<li v-for="item in items|filterBy 'Wscats' in 'name' 'dada' ">{{item}}</li>
```
### orderBy
1. Restriction: need to be used in v-for (that is, array)
2. Three parameters:
   1. The first parameter: String, Array, Function type, the string to search
   2. The second parameter: accept string. The optional parameter order determines the result in ascending order (order> = 0) or descending order (order <0). The default is ascending order, which is different from Angular accepting boolean values. Vue accepts positive and negative value


## Custom filters
1. The global method `Vue.filter ()` registers a custom filter, which must be placed before Vue instantiation
2. The filter function always takes the value of the expression as the first argument. Quoted parameters are treated as strings, and unquoted parameters are evaluated as expressions
3. Two filter parameters can be set, provided that the two filters do not conflict
4. The data input by the user can also be processed before being returned to the model

These are the following situations:

The filter function always takes the value of the expression as the first argument. Quoted parameters are treated as strings, and unquoted parameters are evaluated as expressions.
```javascript
<p>{{num | WscatsCal 10 20}}</p>
```
Add two filters, be careful not to conflict
```javascript
<p>{{num | sum | currency }}</p>
```
The data input from input can also be processed before being returned to the model
```javascript
<input type="text" v-model="message | change">
```
The global method `Vue.filter ()` registers a custom filter and must be placed before Vue instantiation
```javascript
Vue.filter("WscatsCal", function(){})
new Vue({});
```
The above example is registered directly on the Vue global. Other instances that do not use this filter will also be forced to accept it. In fact, the filter can be registered inside the instance, and only registered in the instance that uses it.
In other words, Vue can register filters globally or locally, which is an advantage over Angular.


## Components
1. The first step is to define `Vue.extend` with a constructor
2. The second step is to register `Vue.component`
3. Place the component's label on the corresponding view

### Steps to create a component
```javascript
var Wscats = Vue.extend({//Step one: define
	template: '<p>我是Oaoafly Wscats</p>'
})
Vue.component('myWscats', Wscats)//Step 2: Register to Vue (here is global registration)
var myDemo = new Vue({//Step three: create the instantiation
	el: '#demo'
})
```
In addition to the global registration components above, you can also register components locally, as shown in the following code
```javascript
<div id="demo">
	<p>{{name}}</p>
	<first-compenent></first-compenent>
</div>
//JS
var Wscats = Vue.extend({
	template: "<p> This is the first component </ p>"
}) 
//registered
Vue.component("firstCompenent", aaa)
new Vue({
	el: "#demo",
	data: {
		name: "Component DEMO"
	},
	components: {
		firstCompenent: Wscats
	}
})
```
Templates can be written outside using the <template> tag, which makes it easier to write code
Note that here must be id, not using class
```javascript
<template id="#yonicb">
	<div>My name is Yoni Calsin</div>
</template>
```
```javascript
components: {
	yonicb: {
		template: '#yonicb'
	}
}
```

## Vuex
Install vuex
```
npm install vuex
```
Introduce JS, remember to introduce it behind vue
```javascript
<script src="js/vuex.js"></script>
```
Define vuex state management repository
```javascript
var store = new Vuex.Store({
	//Where data is stored
	state: {
		name: "news",
		author: "yonivb",
		searchName: ""
	},
	mutations: {
		//Method of changing the value
		changeName: function(state, a) {
			state.name = a
		},
		changesearchName: function(state, a) {
			state.searchName = a
		}
	},
	//Method of obtaining data
	getters: {
		getName: function(state) {
			return state.name
		}
	}
})
```
Inject the store into the constructor
```javascript
new Vue({
	el: "#demo",
	components: {},
	store //Injected into store
})
```
Get the value of state in vuex
Get value with `this. $ Store.state.searchName`
```javascript
computed: {
		searchName: function() {
			//vuex state data
			return this.$store.state.searchName
		},
	},
	//You can also use methods to get
	methods: {
		getSearchName: function() {
			//vuex state data
			this.searchName = this.$store.state.searchName
		},
	},
```
Set the value of state in vuex
The first parameter of the commit is the function that triggers mutations in vuex, let it help us modify the corresponding value
```javascript
this.$store.commit('changesearchName',this.searchName)
```

## License

Nest is [MIT licensed](LICENSE).