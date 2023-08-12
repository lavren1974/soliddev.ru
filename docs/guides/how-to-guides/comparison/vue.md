import { CodeTabs } from "~/components/Tabs";

<Title>Comparison with Vue</Title>

Solid is not particularly influenced by Vue design-wise, but they are comparable in approach. They both use Proxies in their Reactive system with read based auto-tracking. But that is where the similarities end. Vue's fine-grained dependency detection just feeds into a less fine-grained Virtual DOM and Component system whereas Solid keeps its granularity right down to its direct DOM updates.

Vue values easiness where Solid values transparency. Although Vue's direction with Vue 3 and the introduction of the composition API aligns more with the approach Solid takes. These libraries might align more over time depending on how they continue to evolve. Most of the examples and references to Vue in this section will be made in regards to the Composition API introduced in Vue 3.

## Template Components vs Function Components

In terms of frontend frameworks components serve as a way to split the UI into different reusable pieces of code, most applications and sites nowadays are mostly just trees of nested components.

### Vue

In Vue setting up components can be done in 2 ways either by using Templates or by using Render Functions. The most common way right now is through the use of Templates, like so:

```vue
<script lang="ts" setup>
import { ref } from "vue";

const count = ref(0);
</script>

<template>
  <button @click="count++">Count was pressed {{ count }} times</button>
</template>
```

If you are unfamiliar with it, this is the template syntax used with the composition API in Vue. HTML is used in the `<template></template>` tag and Javascript or Typescript is used in the `<script></script>` tag. There is a third tag used for styling called the `<style></style>` tag as well.

### Solid

Solid on the other hand makes use of only one kind of component, the function component. Unlike Vue, Solid doesn't have different syntaxes, it only has one. All components and pages in Solid are represented by function components. E.g:

```tsx
import { createSignal } from "solid-js";

const App = () => {
  const [count, setCount] = createSignal(0);
  return (
    <div>
      <button onclick={() => setCount((previousValue) => previousValue + 1)}>
        You clicked me {count} times
      </button>
    </div>
  );
};

export default App;
```

This is what a function component looks like. Notice the major difference between this and the template syntax used by Vue. There are also some slight differences in the way variables are used in the HTML portion e.g a single curly bracket instead of two. The syntax to declare and mutate for stateful variables is also quite different, more on this later.

## Reactivity and Statefulness

In Vue there are 2 key functions that are used to declare and manage reactive and stateful values, `ref()` and `reactive()`, `ref()` being able to store primitives such as booleans, numbers, strings and objects as well, and `reactive()` only being able to store objects. Solid has similar functions to these namely `createSignal()` and `createStore()` respectively. In this part we will be discussing about their differences in syntax and update methods.

There is not much of a difference between the syntax of `ref()` and `createSignal()` or that of `reactive()` and `createStore()`, in fact, `reactive()` and `createStore()` are similar in syntax when accessing values but different when changing values e.g:

```js
// Vue
const store = reactive({ count: 0 });
function someFunc() {
  console.log(store.count); // accessing
  store.count = store.count + 1; // setting
}

// Solid
const [store, setStore] = createStore({ count: 0 });
function someFunc() {
  console.log(store.count); // accessing
  setStore((prev) => (prev = { ...prev, count: prev.count + 1 })); // setting
}
```

While on the other hand `ref()` and `createSignal()` are different in both setting and accessing their values, e.g:

```js
// Vue
const count = ref(0);
function someFunc() {
  console.log(count.value); // accessing
  count.value = count.value + 1; // setting
}

// Solid
const [count, setCount] = createSignal(0);
function someFunc() {
  console.log(count()); // accessing
  setCount((prev) => prev + 1); // setting
}
```

**Below is a more in-depth look at how the syntax might differ in a simple counter app**

### Vue

In Vue you can make use of the `ref()` and `reactive()` functions to handle state in a Vue component. In some cases `reactive()` can serve as a simple state management alternative instead of having to make use of VueX. Here's a quick snippet of what that might look like:

```js
// store.js
import { reactive } from "vue";

export const store = reactive({
  count: 0,
});
```

Above we created our store using the `reactive()` function.

```vue
<!-- ComponentA.vue -->
<script setup>
import { store } from "./store.js";

const unreactiveLocalValue = store.count;
//Note: this will not behave reactively
</script>

<template>
  <button @click="store.count + 1">add +1 to store.count</button>
</template>
```

Above is a component which has the store imported in order to change one of the values within the store.

```vue
<!-- ComponentB.vue -->
<script setup>
import { store } from "./store.js";

const unreactiveLocalValue = store.count;
//Note: this will not behave reactively
</script>

<template>From B: {{ store.count }}</template>
```

This is a seperate component which will display the value of the count even as it is changing.

You can also make use of other reactive state such as `ref()` and `computed()`.

```js
// store.js
import { ref } from "vue";

export const storeUsingRef = ref(0);
```

```vue
<!-- ComponentA.vue -->
<script setup>
import { computed } from "vue";
import { storeUsingRef } from "./store.js";

//Note: to make use of ref() values in the script section you will have to use the .value property like so:
const reactiveLocalValue = computed(() => storeUsingRef.value);
//This will behave reactively
</script>

<template>
  <button @click="storeUsingRef + 1">add +1 to store</button>
</template>
```

```vue
<!-- ComponentB.vue -->
<script setup>
import { computed } from "vue";
import { storeUsingRef } from "./store.js";

//Note: to make use of ref() values in the script section you will have to use the .value property like so:
const reactiveLocalValue = computed(() => storeUsingRef.value);
//This will behave reactively
</script>

<template>
  <h2>From B: {{ storeUsingRef }}</h2>
  <h2>From B: {{ reactiveLocalValue }}</h2>
</template>
```

Take note of the comments within the code and how reactivity changes when `reactive()` or `ref()` is used.

### Solid

In Solid we can do something similar by making use of the `createStore()` and the `createSignal()` primitives. Remember that `createStore()` only takes in objects as values, while `createSignal()` can take in booleans, objects, strings and numbers. Here's a quick example of what it might look like if you decide to use `createStore()` as a simple state management solution

```js
// store.js
import { createStore } from "solid-js/store";

export const [store, setStore] = createStore({ count: 0 });
```

Above we created a store using `createStore()` in Solid.

```jsx
// Component A.jsx
import { setStore, store } from "./store";

const ComponentA = () => {
  const unreactiveLocalValue = store.count;
  //Note: this will not behave reactively

  return (
    <div>
      <button
        onclick={() =>
          setStore((prev) => (prev = { ...prev, count: prev.count + 1 }))
        }
      >
        add +1 to store
      </button>
    </div>
  );
};

export default ComponentA;
```

The above component simply imports the setter and getter functions of the store in order to set and get the current state of the store.

```jsx
// Component B.jsx
import { store } from "./store";

const ComponentB = () => {
  const unreactiveLocalValue = store.count;
  //Note: this will not behave reactively

  return (
    <div>
      <h2>From B: {store.count}</h2>
    </div>
  );
};

export default ComponentB;
```

This is a seperate component which serves the purpose of displaying the current state of the store and the values within.

You can also make use of the `createSignal()` primitive, like so:

```js
//store.js
import { createSignal } from "solid-js";

export const [storeUsingSignal, setStoreUsingSignal] = createSignal(0);
```

```jsx
// Component A.jsx
import { setStoreUsingSignal, storeUsingSignal } from "./store";

const ComponentA = () => {
  const reactiveLocalValue = storeUsingSignal;
  // This 👆 will behave reactively

  const unreactiveLocalValue = storeUsingSignal();
  // This 👆 will not behave reactively

  return (
    <div>
      <button onclick={() => setStoreUsingSignal((prev) => prev + 1)}>
        add +1 to store
      </button>
    </div>
  );
};

export default ComponentA;
```

```jsx
// Component B.jsx
import { storeUsingSignal } from "./store";

const ComponentB = () => {
  const reactiveLocalValue = storeUsingSignal;
  // This 👆 will behave reactively

  const unreactiveLocalValue = storeUsingSignal();
  // This 👆 will not behave reactively

  return (
    <div>
      <h2>From B: {storeUsingSignal()}</h2>
      <h2>From B: {reactiveLocalValue()}</h2>
    </div>
  );
};

export default ComponentB;
```

Take note of the comments within the code and how reactivity changes when `createStore()` or `createSignal()` is used.

## Conditional Rendering

In the world of frontend frameworks conditional rendering is the act of rendering a component or node only when a specified condition is met.

For example let's say you only want a form input to be shown if the user clicks the show/hide button and hidden if the user clicks that button again. In that case you can simply make use of a **boolean variable** called `isShowing` and if that value is true it will show the form and if false it will hide it.

Alright, now that we've briefly touched on the concept of conditional rendering let's see what this might look like in **Vue** and then in **Solid**

### Vue

If you have experience with Vue I'm sure you should be familiar with the `v-if`, `v-else` and `v-else-if` directives. This is used to conditionally render itself and anything that it is wrapping. It can be used like so:

```html
<h1 v-if="solidVotes < vueVotes">Vue is awesome!</h1>
<h1 v-else-if="solidVotes > vueVotes">Solid is more awesome 😁</h1>
<h1 v-else>Oh no! No votes 😢</h1>
```

In the code block above `Vue is awesome` is rendered if vueVotes is more than solidVotes, and `Solid is more awesome 😁` is rendered if opposite is the case, and there is a `else` at the end in case neither is the case.

### Solid

Solid has components similar to these that make conditional rendering very easy and look much cleaner. These components are namely `Show`, `Switch` and `Dynamic`. Let's take a look at how these can be used.

##### Show

This can be used to handle simple boolean values and expressions.

```jsx
<Show
  when={solidVotes() > vueVotes()}
  fallback={<h1>Vue has more votes</h1>}
>
  <h1>Solid has more votes</h1>
</Show>
```

In the above code if the `when` condition isn't satisfied it will render the fallback.

##### Switch

This can be used to handle switch cases and more complex expressions where you're dealing with more than 2 mutually exclusive outcomes.

```jsx
<Switch fallback={<p>There haven't been any votes yet</p>}>
  <Match when={solidVotes() > vueVotes()}>
    <h1>Solid has more votes</h1>
  </Match>
  <Match when={solidVotes() < vueVotes()}>
    <h1>Vue has more votes</h1>
  </Match>
</Switch>
```

In the above code if none of the `when` conditions are satisfied it will render the fallback.

##### Dynamic

This built in component can be used to render elements from a map of keys and components/elements. It takes in a prop called component and dynamically renders out which ever element or component it is passed. You can take advantage of this by passing in a component dynamically using a map and key. Here's a quick example to show you how it's used

```jsx
import { render, Dynamic } from "solid-js/web";
import { createSignal, For } from "solid-js";

const RedThing = () => <strong style="color: red">Red Thing</strong>;
const GreenThing = () => <strong style="color: green">Green Thing</strong>;
const BlueThing = () => <strong style="color: blue">Blue Thing</strong>;

const options = {
  red: RedThing,
  green: GreenThing,
  blue: BlueThing,
};

function App() {
  const [selected, setSelected] = createSignal("red");

  return (
    <>
      <select
        value={selected()}
        onInput={(e) => setSelected(e.currentTarget.value)}
      >
        {Object.keys(options).map((color) => (
          <option value={color}>{color}</option>
        ))}
      </select>
      {/* the Dynamic component 👇*/}
      <Dynamic component={options[selected()]} />
    </>
  );
}
```

In the above code the component that is chosen to render depends on the drop down option chosen which serves as a key to the map of components.

Feel free to experiment with and use which ever built in component makes you comfortable.

## List Rendering

A lot of times developers find themselves using lists more often than not. In the case where you would like to iterate over and render objects in regards to lists or arrays within your app, some frontend frameworks offer a way for you to do that fairly easily. Let's take a look at how **Vue** and **Solid** make that possible.

### Vue

In Vue you make use of the `for` directive to render a list or array of items, like so:

```html
<li v-for="item in items">{{ item.message }}</li>
```

In the above code Vue renders as many `<li></li>` elements as there are items in the items array.

### Solid

In Solid the `<For>` component is the best way to loop over an array of objects. As the array changes, `<For>` updates or moves items in the DOM rather than recreating them. Let's look at an example using the `<For>` component

```html
<For each="{items()}">
  {(item, i) => (
  <li>{item.message}</li>
  ) }
</For>
```

There is one prop in the `<For>` component called each, this is where you pass the array you wish to loop over.

## Vue vs. Solid

Here's a table differentiating hooks in Vue against hooks in Solid.

| Vue(composition)    | Vue(options)           | Solid          |
| ------------------- | ---------------------- | -------------- |
| onMounted()         | mounted                | onMount()      |
| onUnmounted()       | unmounted              | onCleanup()    |
| ref(), shallowRef() | data                   | createSignal() |
| reactive()          | data                   | createStore()  |
| computed()          | computed               | createMemo()   |
| watchEffect()       | this.$watch() or watch | createEffect() |
