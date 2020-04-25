## Vuejs reminder

### Intro

#### Starting
- Hello world example
https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-hello-world?file=/index.html
- or, importing it
```html
<!-- development version, includes helpful console warnings -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!-- production version, optimized for size and speed -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

#### Declarative
```html
<div id="app">
  {{ message }}
</div>
```
```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
Now, we can use ```app.message``` wherever we need
A directive example ( -v prefix ), in other words,
"keep this element’s `title` attribute up-to-date with the `message` property on the Vue instance"
```html
<div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>
```
```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'You loaded this page on ' + new Date().toLocaleString()
  }
})
```
Again, ```app2.message = 'some new message'``` 'll update the title of the span

#### Conditional
```html
<div id="app-3">
  <span v-if="seen">Now you see me</span>
</div>
```
```js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```
Now we can ```app3.seen = !app3.seen```
R: transitions avaialable https://vuejs.org/v2/guide/transitions.html
R: https://vuejs.org/v2/guide/transitions.html#Transitioning-Between-Components

#### Loops
`v-for` directive can be used for displaying a list of items using the data from an Array:
```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
```js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```
Now we can `app4.todos.push({ text: 'New item' })` & have the list displayed update

#### User input
use the `v-on` directive to attach event listeners that invoke methods on our Vue instances
```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```
```js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
In this method we update the state of our app without touching the DOM - all DOM manipulations are handled by Vue, and the code you write is focused on the underlying logic.

The `v-model` directive that makes two-way binding between form input and app state a breeze:
```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
```js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```

#### Composing with Components
it’s an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components.
A component is essentially a Vue instance with pre-defined options. Registering a component in Vue is straightforward:
```js
// Define a new component called todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})

var app = new Vue(...)
```
Then we can compose it in another component’s template:
```html
<ol>
  <!-- Create an instance of the todo-item component -->
  <todo-item></todo-item>
</ol>
```
This would render the same text for every todo, which is not super interesting.

But we can also pass data from the parent scope into child components. Let’s modify the component definition to make it accept a [prop](https://vuejs.org/v2/guide/components.html#Props):
```js
Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
Now we can pass the todo into each repeated component using `v-bind`:
```html
<div id="app-7">
  <ol>
    <!--
      Now we provide each todo-item with the todo object
      it's representing, so that its content can be dynamic.
      We also need to provide each component with a "key",
      which will be explained later.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```
