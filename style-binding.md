# Binding class
## Object Syntax
v-bind: class = "{style name: expression whose result is boolean}", if the expression is true, the element class = "style name", otherwise the element class = ""
``` html
    <div :class="{classNam1: 1 == 1, className2: 1 == 2}"></div>
```
Rendering results
``` html
    <div class="classNam1"></div>
```

Can also be written as
``` html
    <div :class="classObject"></div>
```
``` javascript
    data: {
        classObject: {
            className1: false,
            className2: true
        }
    }
```
Rendering results
``` html
    <div class="className2"></div>
```

You can also calculate properties
``` html
    <div :class="classObjectComputed"></div>
```
``` javascript
    computed: {
        classObjectComputed: function(){
            return{
                className1: true,
                className2: true
            }
        }
    }
```
Rendering results
``` html
    <div class="className1 className2"></div>
```

## 数组语法
v-bind: class = "[]", the array element can be an expression or a string, if it is a string, it is directly output as the style name
``` html
    <div :class="[class1, class2, 'className3', active ? 'className4' : '']"></div>
```
``` javascript
    data: {
        class1: 'className1',
        class2: 'className2',
        active: true
    }
```
Rendering results
``` html
    <div class="className1 className2 className3 className4"></div>
```

You can also use the data object method, the parsing logic is the same as the object syntax
``` html
    <div :class="[{className1: 1 == 1, className2: 1 == 2}, 'className3' ]"></div>
```
Rendering results
``` html
    <div class="className1 className3"></div>
```

# Binding style
## Object Syntax
In the object, the property name of the CSS should be expressed in camel case: fontSize resolves to font-size
``` html
    <div :style="{color: color, fontSize: fontSize, backgroundColor: '#ccc'}"></div>
```
``` javascript
    data: {
        color: 'red',
        fontSize: '12px'
    }
```
Rendering results
``` html
    <div style="color: red, font-size: 12px; background-color: #ccc"></div>
```
## Array syntax
``` html
    <div :style="[styleObject, {backgroundColor: '#ccc'}]"></div>
```
``` javascript
    data: {
        styleObject: {
            color: 'red',
            fontSize: '12px'
        }
    }
```
Rendering results
``` html
    <div style="color: red, font-size: 12px; background-color: #ccc"></div>
```