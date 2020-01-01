# Event modifier
Add some general restrictions on events, such as preventing the event from bubbling, Vue provides specific writing for such event restrictions, called modifiers
Usage: v-on: event.modifier

```html
    <!--Stop events from bubbling.-->
    <div id="div1" class="stop" @click.stop="event1(1)">
    <!--Use event capture mode.-->
    <div id="div4" class="stop" @click.capture="event1(4)">
    <!--The event only acts on itself, similar to a blocked event bubbling-->
    <div id="div7" class="stop" @click.self="event1(7)">
    <!--Prevent browser default behavior.-->
    <a href="https://github.com/yonicalsin" target="_blank" @click.prevent="prevent">dk's github</a>
    <!--Only works once.-->
    <a href="https://github.com/yonicalsin" target="_blank" @click.once="prevent">dk's github</a>
    <!--Modifiers can be concatenated.click.prevent.once-->
    <a href="https://github.com/yonicalsin" target="_blank" @click.prevent.once="prevent">dk's github</a>
```

# Form modifiers

``` html
<div id="app">
   <fieldset>
      <legend>
         <h3>Stop events from bubbling.</h3>
      </legend>
      <p>Sequence: {{stop}}</p>
      <div id="div1" class="stop" @click.stop="event1(1)">
         <span>div1</span>
         <div id="div2" @click.stop="event2(2)">
            <span>div2</span>
            <div id="div3" @click.stop="event3(3)">
               <span>div3</span>
            </div>
         </div>
      </div>
   </fieldset>
   <fieldset>
      <legend>
         <h3>Use event capture mode.</h3>
      </legend>
      <p>Order: {{capture}}</p>
      <div id="div4" class="stop" @click.capture="event1(4)">
         <span>div4</span>
         <div id="div5" @click.capture="event2(5)">
            <span>div5</span>
            <div id="div6" @click.capture="event3(6)">
               <span>div6</span>
            </div>
         </div>
      </div>
   </fieldset>
   <fieldset>
      <legend>
         <h3>The event only acts on itself.</h3>
      </legend>
      <p>order:{{self}}</p>
      <div id="div7" class="stop" @click.self="event1(7)">
         <span>div7</span>
         <div id="div8" @click.self="event2(8)">
            <span>div8</span>
            <div id="div9" @click.self="event3(9)">
               <span>div9</span>
            </div>
         </div>
      </div>
   </fieldset>
   <fieldset>
      <legend>
         <h3>Prevent browser default behavior.</h3>
      </legend>
      <a href="https://github.com/yonicalsin" target="_blank" @click.prevent="prevent">dk's github</a>
   </fieldset>
   <fieldset>
      <legend>
         <h3>Only works once.</h3>
      </legend>
      <a href="https://github.com/yonicalsin" target="_blank" @click.once="prevent">dk's github</a>
   </fieldset>
   <fieldset>
      <legend>
         <h3>Modifiers can be concatenated.click.prevent.once</h3>
      </legend>
      <a href="https://github.com/yonicalsin" target="_blank" @click.prevent.once="prevent">dk's github</a>
   </fieldset>
</div>
<script type="text/javascript">
   var vm = new Vue({
   	el: '#app',
   	data:{
   		stop: '',
   		capture: '',
   		self: ''
   	},
   	methods: {
   		randomColor: function(){
                  var r = parseInt(Math.random() * 255);
                  var g = parseInt(Math.random() * 255);
                  var b = parseInt(Math.random() * 255);
                  var color = 'rgb(' + r + ',' + g + ', ' + b + ')';
                  return color;
              },
              changeBackground: function(id){
   			if(id < 4){
   				this.stop += ('div' + id + ' | ');
   			} else if(id < 7) {
   				this.capture += ('div' + id + ' | ');
   			} else {
   				this.self += ('div' + id + ' | ');
   			}
   			document.getElementById('div' + id).style.background = this.randomColor();
              },
   		event1: function(id){
   			this.changeBackground(id);
   		},
   		event2: function(id){
   			this.changeBackground(id);
   		},
   		event3: function(id){
   			this.changeBackground(id);
   		},															
   		prevent: function(event){
   			event.target.style.color = this.randomColor();
   		}
   	}
   })
</script>
```