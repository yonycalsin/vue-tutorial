# Template syntax
The template syntax has become the best implementation of the V layer in the data-driven mode of the front end.

## Interpolation
``` html
<div id="app">
  <!-- Text When data.message is changed, the content of the corresponding interpolation will also change automatically-->
  <fieldset>
    <legend>text</legend>
    <div>{{message}}</div>
  </fieldset>

  <!-- Pure HTML {{}} this form will eventually be interpreted as text. If you want to enter HTML structure, use v-html = "object"-->
  <fieldset>
    <legend>pure HTML</legend>
    <div v-html="rawHtml"></div>
  </fieldset>		

  <!-- Attribute Any element (including custom attributes) can be bound to the object: attribute name (or v-bind: attribute name) = "object"-->
  <fieldset>
    <legend>Attributes</legend>
    <img :src="src" alt="" />
    <img v-bind:src="'../imgs/red.jpg'" alt="" />
  </fieldset>	

  <!-- js expression {{}} can be used to interpret js expressions-->
  <fieldset>
    <legend>js expression</legend>
    <div>{{1 + 1}}</div>
    <div>{{status ? 'YES' : 'NO'}}</div>
    <div>{{message.split('').reverse().join('')}}</div>
  </fieldset>	
</div>
```

``` javascript
var vm = new Vue({
  el: '#app',
  data: {
    message: 'I am text',
    rawHtml: '<h1>I am h1 tag</h1>',
    src: '../imgs/green.jpg',
    status: true,
  }
})
```

## abbreviation
### v-bind abbreviation
``` html
  <!--Complete writing-->
  <img v-bind:src="'../imgs/red.jpg'" alt="" />
  <!--abbreviation-->
  <img :src="src" alt="" />
```
### v-on abbreviation
``` html
  <!--Complete syntax-->
  <button v-on:click="greet">Greet</button>
  <!--Abbreviation syntax-->
  <button @click="greet">Greet</button>  
```

## instruction
Directive, in other words, a custom attribute of an element, a custom attribute prefixed with v- in Vue, the attribute value is an object or a js expression

<table>
  <thead>
    <tr>
      <th>instruction</th><th>Types of</th><th>usage</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>v-text</td><td>string</td><td><!--v-text-->
      
  ``` html
    <span v-text="msg"></span>
    <!--Effect is equivalent to-->
    <!--v-text Weights higher than {{}}-->
    <span>{{msg}}</span>
  ```
  
   </td></tr>
    <tr><td>v-html</td><td>string</td><td><!--v-html-->
      
  ``` html
    <div v-html="html"></div>
  ```
  
   </td></tr> 
    <tr><td>v-show</td><td>boolean</td><td><!--v-show-->
      
  ``` html
    <!--The value of show directly affects whether the div is displayed in the document-->
    <div v-show="show"></div>
  ```
  
   </td></tr>    
    <tr><td>v-if</td><td>boolean</td><td><!--v-if-->
      
  ``` html
    <!--The value of status directly affects the existence of the div in the document-->
    <div v-if="status"></div>
  ```
  
   </td></tr>
    <tr><td>v-else-if</td><td>boolean</td><td><!--v-else-if-->
      
  ``` html
    <div v-if="flag == 1">1</div>
    <!--Must be followed by a v-if or v-else-if element-->
    <div v-else-if="flag == 2">2</div>
  ```
  
   </td></tr>      
    <tr><td>v-else</td><td>No expression needed</td><td><!--v-else-->
      
  ``` html
    <div v-if="flag == 1">1</div>
    <div v-else-if="flag == 2">2</div>
    <!--Must be followed by a v-if or v-else-if element-->
    <div v-else>2</div>
  ```
  
   </td></tr>    
    <tr><td>v-for</td><td>Array | Object | Number | String</td><td><!--v-for-->
      
  ``` html
    <!--
      data = 3
      The result is 3 divs.
      The values are classified as 1, 2, 3
      The values of index are 0, 1, 2
    -->
    <div v-for="(value, index) in data">
      <span v-text="value"></span>
      <span>{{index}}</span>
    </div>
    <!--Can also be written like this-->
    <div v-for="value in data">
      <span v-text="value"></span>
    </div>

    <!--
      data = "abc"
      The result is data.length divs,
      The values are classified as a, b, c
      The values of index are 0, 1, 2
    -->
    <div v-for="(value, index) in data">
      <span v-text="value"></span>
      <span>{{index}}</span>
    </div>   
    <!--Can also be written like this-->
    <div v-for="value in data">
      <span v-text="value"></span>
    </div>

    <!--
      data = {name: 'dk', age: 18}
      The result will generate a number of divs for the data attribute,
      value is classified as dk, 18
      The values of key are name, age
    -->
    <div v-for="(value, key) in data">
      <span v-text="key"></span>
      <span>{{value}}</span>
    </div>
    <!--Can also be written like this-->
    <div v-for="value in data">
      <span v-text="value"></span>
    </div>

    <!--
      data = [{name: 'dk1', age: 18}, {name: 'dk2', age: 20}]
      The result is data.length divs,
      The values of obj are classified as data [0], data [1]
      The values of index are 0, 1
    -->
    <div v-for="(obj, index) in data">
      <span v-text="JSON.stringify(obj)"></span>
      <span>{{index}}</span>
    </div>    
    <!--Can also be written like this-->
    <div v-for="obj in data">
      <span v-text="JSON.stringify(obj)"></span>
    </div>    
  ```
  
   </td></tr> 
    <tr><td>v-on</td><td>Function</td><td><!--v-on-->
      
  ``` html
    <!--Click event directly binds a method-->
    <button v-on:click="say1">say1</button>
    <!--Abbreviation-->
    <!--The click event uses inline statements-->
    <button @click="say2('Called say2', $event)">say2</button>     
  ```
  
   </td></tr> 
       <tr><td>v-bind</td><td>Object</td><td><!--v-bind-->
      
  ``` html
    <img v-bind:src="'imgs/red.jpg'" />
    <!--Abbreviation-->
    <img :src="'imgs/yellow.jpg'" />
  ```
  
   </td></tr>    
      <tr><td>v-model</td><td>表单元素的值</td><td><!--v-model-->
      
  ``` html
    <!--For form elements only, two-way binding-->
    <input type="text" v-model="mess"/>
  ```
  
   </td></tr>  
      <tr><td>v-pre</td><td>No expression needed</td><td><!--v-pre-->
      
  ``` html
    <!--{{}} Does not compile when the string is output-->
    <span v-pre>{{mess}}</span>
  ```
  
   </td></tr>  
      <tr><td>v-cloak</td><td>No expression needed</td><td><!--v-cloak-->
      
  ``` html
    <!--
      mess = 'abc'
      ({mess}) will be displayed when the span has not been parsed by vue
      123 will be displayed after parsing
      Unfriendly display of the process used to resolve these two conversions
      This is especially the case when the page loads too slowly
    -->
    <span v-cloak>{{mess}}</span>
  ```
  
   </td></tr>     
      <tr><td>v-once</td><td>No expression needed</td><td><!--v-once-->
      
  ``` html
    <!--The content is only explained once and will not be mapped to the span again when the mess is changed-->
    <span v-once>{{mess}}</span>
  ```
  
   </td></tr>              
  </tbody>
</table>