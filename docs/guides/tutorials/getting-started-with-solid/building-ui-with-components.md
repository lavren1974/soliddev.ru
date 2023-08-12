import {
  PrevSection,
  NextSection,
  PrevNextSection,
} from "~/components/NextSection";
import { CodeTabs } from "~/components/Tabs";
import { FrameworkAside, Aside } from "~/components/configurable/Aside";
import HelloWorld from "./snippets/HelloWorld.mdx";
import HelloWorldExport from "./snippets/HelloWorldExport.mdx";
import HelloWorld2 from "./snippets/HelloWorld2.mdx";
import HelloWorld3 from "./snippets/HelloWorld3.mdx";
import IndexJS from "./snippets/indexjs.mdx";
import IndexTS from "./snippets/indexts.mdx";
import IndexHTML from "./snippets/indexhtml.mdx";
import App from "./snippets/App.mdx";
import BookList1 from "./snippets/bookshelf/BookList1.mdx";
import AddBook1 from "./snippets/bookshelf/AddBook1.mdx";
import App1 from "./snippets/bookshelf/App1.mdx";
import App2js from "./snippets/bookshelf/App2js.mdx";
import App2ts from "./snippets/bookshelf/App2ts.mdx";
import { IfTS } from "~/components/configurable/IfConfig";

# Building User Interfaces with Components

## What are components?

A component is a function that defines a piece of your user interface. They're the building blocks of a Solid app.

Let's build our first component. This component will display `Hello World!` inside a `<div>` element.
To do this, we need to create a file called <IfTS fallback="HelloWorld.jsx">HelloWorld.tsx</IfTS>
inside our src folder. Then copy the following contents:

<CodeTabs
  js={[{ name: "HelloWorld.jsx", component: HelloWorld }]}
  ts={[{ name: "HelloWorld.tsx", component: HelloWorld }]}
/>

There are a couple of things to note in our first component:

- Our component name, `HelloWorld`, is title-cased (sometimes referred to as _Pascal_ case), with _*H*ello_ and _*W*orld_ capitalized.
  This helps us differentiate components from other JavaScript functions.
- Our component returns something that looks a lot like HTML, but isn't&mdash;it's _JSX_.

<FrameworkAside framework="react">
While Solid components look like React components, they differ in a few distinct ways. Solid component functions _only run once_ to set up and never run again. If you're used to placing a calculation in the function body of a React component and expecting it to recompute when the component re-renders, you may need to take a different approach with Solid. Don't worry&mdash; we'll cover all the best practices throughout this tutorial.

Like React, you can include as many components as you like within a file. You can even write a component _inside_ of another!

</FrameworkAside>

<FrameworkAside framework="vue">
In Vue, components have _lifecycles_ that are important to how you work with them. 
In Solid, components are simply functions that group your code. They
don't have lifecycles, and Solid's rendering system is based on the _reactive_ features you use&mdash;we'll get
to that!

In Solid, you can include as many components as you like within a file. You can even write a component _inside_ of another!

</FrameworkAside>
<FrameworkAside framework="svelte">

In Svelte, components have _lifecycles_ that are important to how you work with them.
In Solid, components are simply functions that group your code. They
don't have lifecycles, and Solid's rendering system is based on the _reactive_ features you use&mdash;we'll get
to that!

Svelte uses Single-File Components, but Solid does not. You can include as many components as you like within a file. You can even write a component _inside_ of another!

</FrameworkAside>
<FrameworkAside framework="angular">
In Angular, components have _lifecycles_ that are important to how you work with
them. In Solid, components are simply functions that group your code. They
don't have lifecycles, and Solid's rendering system isn't based on the
boundaries of your components but on the reactive features you use&mdash;we'll get
to that! Another thing to note: While you're able to write multiple Angular components
in the same file using the `template` component decorator property, Solid takes it
to another level.
In Solid, not only can you include as many components as you like
within a file, you can even write a component _inside_ of another!
</FrameworkAside>

### Using a component

Let's get this component on the screen. Export it from <IfTS fallback="HelloWorld.jsx">HelloWorld.tsx</IfTS> by replacing `function` with `export function`.
Then, let's replace the boilerplate in <IfTS fallback="App.jsx">App.tsx</IfTS> with code that imports and displays our component:

<CodeTabs
  js={[
    { name: "HelloWorld.jsx", component: HelloWorldExport },
    { name: "App.jsx", component: App, default: true },
  ]}
  ts={[
    { name: "HelloWorld.tsx", component: HelloWorldExport },
    { name: "App.tsx", component: App, default: true },
  ]}
/>

We used our component just like any other element: after importing the `HelloWorld` function, we can now use `<HelloWorld />`.

<Aside>
  By building with components, we're able to separate and reuse code. Components
  let us create building blocks that we can put together to form new pieces.
</Aside>

## What is JSX?

As we learned above, Solid components return JSX. JSX is an HTML-like syntax used in Solid JavaScript code to represent part of the Document Object Model (DOM).
In our <IfTS fallback="HelloWorld.jsx">HelloWorld.tsx</IfTS> example above, we returned the following JSX from our component:

```tsx
<div>Hello World!</div>
```

This JSX represents a `<div>` element with the text `Hello World!` inside. It's just as if we had written it in an HTML document.
As a means of showing you the power and flexibility of Solid, let's add some inline styling to our `<div>`:

```tsx
<div style="background-color: #2c4f7c; color: #FFF;">Hello world!</div>
```

This will render a div with a blue background color and white text:

<div style="background-color: #2c4f7c; color: #FFF;">Hello world!</div>

Trying to create a stylized div using JavaScript _without_ JSX would be more complex:

```ts
const div = document.createElement("div");
div.style["background-color"] = "#2c4f7c";
div.style.color = "#FFF";
```

<FrameworkAside framework="vue">
  Vue has its own "template language" in a designated file type called `.vue`.
  `.jsx` serves a similar role in Solid, but isn't directly integrated into
  JavaScript. We can write a JSX element anywhere we could write a JavaScript
  expression, and there's no enforced separation between template and data.
</FrameworkAside>

<FrameworkAside framework="svelte">
  Svelte has its own "template language" in a designated file type called
  `.svelte`.`.jsx` serves a similar role in Solid, but isn't directly integrated
  into JavaScript. We can write a JSX element anywhere we could write a
  JavaScript expression, and there's no enforced separation between template and
  data.
</FrameworkAside>
<FrameworkAside framework="angular">
  Instead of JSX, Angular has its own "template compiler" that takes template
  strings and compiles them into JavaScript instructions. This serves a similar
  role to JSX, but isn't directly integrated into JavaScript. In Solid, you can
  write a JSX element anywhere you could write a JavaScript expression, and
  there's no enforced separation between template and data.
</FrameworkAside>

### JSX + JavaScript

JSX becomes even more powerful when used alongside JavaScript expressions. For example, we can use variables to represent some of the text content in our `<div>`:

```tsx
const name = "Solid";

<div style="background-color: #2c4f7c; color: #FFF;">Hello {name}!</div>;
```

Notice how we can add `{name}` where it used to say `World`. This JavaScript expression allows us to modify the content of our `<div>`:

<div style="background-color: #2c4f7c; color: #FFF;">Hello Solid!</div>

We can take this a step further and specify our div's `{style}` attribute as a JavaScript object:

```tsx
const name = "Solid";
const style = { "background-color": "#2c4f7c", color: "#FFF" };

<div style={style}>Hello {name}!</div>;
```

With JSX, we have code that lets us take advantage of the dynamic nature of JavaScript with a declarative HTML-like syntax.

As we progress through these tutorials, we'll discover how Solid's reactive system combines with JSX to improve the experience of crafting user interfaces.

<FrameworkAside framework="vue">

This syntax of binding HTML and attribute values are two different syntaxes in Vue. The above might look something like this in Vue:

```html
<div :style="style">Hello {{name}}!</div>
```

</FrameworkAside>

Let's update our initial `HelloWorld` component with what we learned in our JSX exploration:

<CodeTabs
  js={[{ name: "HelloWorld.jsx", component: HelloWorld2 }]}
  ts={[{ name: "HelloWorld.tsx", component: HelloWorld2 }]}
/>

### Using fragments

One rule when using JSX is that a component function must return a single element. For example, the following is valid JSX because there is a single `<div />` that contains the rest of the JSX:

```tsx
<div>
  <h1>Welcome</h1>
  <HelloWorld />
</div>
```

But the following is _not_ valid JSX because there is no single containing element:

```tsx
<h1>Welcome</h1>
<HelloWorld />
```

However, we may not actually want a `div` surrounding this code when it's rendered to the DOM.
_JSX fragments_ give us a way to write valid JSX and also not actually render a containing element to the DOM:

```tsx
<>
  <h1>Welcome</h1>
  <HelloWorld />
</>
```

<FrameworkAside framework="vue">
  Vue 2 worked similarly: a component needed to have a single root element.
</FrameworkAside>

<FrameworkAside framework="angular">
  Angular has a similar API to Fragments with
  [`ng-container`](https://angular.io/api/core/ng-container).
</FrameworkAside>

### JSX is compiled

JSX is syntax on top of JavaScript and is not directly part of any JavaScript standard. Therefore, JSX can't be run directly in the browser (or most other JavaScript runtimes). Solid and other frameworks that use JSX require their code to be run through a compiler. If you're using a starter template you don't have to worry about this&mdash;everything will just work out of the box.

<Aside type="note">
  Remember how we discussed that component function names are title-cased? This
  is so the compiler understands the difference between native DOM elements
  (like `div`) and custom components (like `HelloWorld`).
</Aside>

<Aside type="advanced" collapsible title="Read more about the compiler">
  Solid's JSX compiler doesn't just compile JSX to JavaScript; it also extracts reactive values (which we'll get to later in the tutorial) and makes things more efficient along the way.

This is more involved than React's JSX compiler, but much less involved than something like Svelte's compiler. Solid's compiler doesn't touch your JavaScript, only your JSX.

If you're curious about how Solid and other frameworks use compilation, check out [A Look at Compilation in JavaScript Frameworks](https://dev.to/this-is-learning/a-look-at-compilation-in-javascript-frameworks-3caj) by the creator of Solid.

</Aside>
## Mounting Solid to the DOM

How does our `App` component make it on to the actual HTML page that we show our users?

To add a Solid application to an HTML document, we first designate a containing element in our HTML and assign it an `id`. That usually looks like this:

```html
<body>
  <div id="root"></div>
</body>
```

Then, we call Solid's `render` function, passing in our `App` component and this element:

```jsx
render(() => <App />, document.getElementById("root"));
```

<FrameworkAside framework="vue">
  This is similar to Vue's
  [`app.mount`](https://vuejs.org/guide/essentials/application.html#mounting-the-app)
  function.
</FrameworkAside>

See how this plays out in our project in the `index.html` file, which creates the div, and <IfTS fallback="src/index.jsx" code>src/index.tsx</IfTS>, which calls that `render` function:

<CodeTabs
  js={[
    { name: "src/HelloWorld.jsx", component: HelloWorld2 },
    { name: "src/App.jsx", component: App },
    { name: "src/index.jsx", component: IndexJS, default: true },
    { name: "index.html", component: IndexHTML },
  ]}
  ts={[
    { name: "src/HelloWorld.tsx", component: HelloWorld2 },
    { name: "src/App.tsx", component: App },
    { name: "src/index.tsx", component: IndexTS, default: true },
    { name: "index.html", component: IndexHTML },
  ]}
/>

<Aside type="note">

You'll want to make sure the first argument passed to the render function is a function that returns a component `() => <Component />` rather than just the component itself.

</Aside>

<FrameworkAside framework="react">
React defaults to having a `StrictMode` component added to the root of your app, which [changes the render behavior of your code depending on whether you're in a development mode or not](https://beta.reactjs.org/learn/keeping-components-pure#side-effects-unintended-consequences:~:text=Detecting%20impure%20calculations%20with%20StrictMode).

Solid has no such behavioral differences between production and development rendering.

</FrameworkAside>

## Composing multiple components together

In most cases, our applications are big enough that we will want to split them into _multiple_ components. Let's move on from our `HelloWorld` example to build something a little bigger&mdash;a virtual bookshelf. Our bookshelf will allow us to add books, mark books as "read", and eventually fetch book titles from other services on the Internet.

Given the intuitive nature of JSX, we can plan out our application a bit. Our main `Bookshelf` will comprise two top-level components:

- A `<BookList />` component, which lists all the books in our bookshelf
- An `<AddBook />` component, which will allow us to add a book to our bookshelf

While we'll eventually want to make our bookshelf interactive, let's first create non-interactive versions of each component. First, we can make the `BookList` component inside and list a couple of books alongside their authors:

<CodeTabs
  js={[{ name: "BookList.jsx", component: BookList1 }]}
  ts={[{ name: "BookList.tsx", component: BookList1 }]}
/>

Next, we create an `AddBook` component. This will be a form that eventually allows us to add a book to our `BookList` list.

<CodeTabs
  js={[
    { name: "AddBook.jsx", component: AddBook1 },
    { name: "BookList.jsx", component: BookList1 },
  ]}
  ts={[
    { name: "AddBook.tsx", component: AddBook1 },
    { name: "BookList.tsx", component: BookList1 },
  ]}
/>

Finally, we can _compose_ our `BookList` and `AddBook` components together. Let's create a `Bookshelf` component within <IfTS code fallback="App.jsx">App.tsx</IfTS>

<CodeTabs
  js={[
    { name: "App.jsx", component: App1 },
    { name: "AddBook.jsx", component: AddBook1 },
    { name: "BookList.jsx", component: BookList1 },
  ]}
  ts={[
    { name: "App.tsx", component: App1 },
    { name: "AddBook.tsx", component: AddBook1 },
    { name: "BookList.tsx", component: BookList1 },
  ]}
/>

### Component relationships

If a component includes another, we call it a _parent_ component.
In our `Bookshelf` example, the `Bookshelf` component is the _parent_ of both `BookList` and `AddBook` because those two components are within `Bookshelf`. Accordingly, `BookList` and `AddBook` are _children_ of `Bookshelf`. You may already be familiar with this terminology; it's how we tend to refer to HTML element relationships in our DOM tree.

### Passing props between components

Components can communicate with their child components by passing properties, often shortened to `props`. To demonstrate the power of `props`, let's customize the bookshelf by adding a name. To do this, add a `props` parameter to the bookshelf function. Then, add a `name` attribute when we use the component:

<CodeTabs
  js={[
    { name: "App.jsx*", component: App2js },
    { name: "AddBook.jsx", component: AddBook1 },
    { name: "BookList.jsx", component: BookList1 },
  ]}
  ts={[
    { name: "App.tsx*", component: App2ts },
    { name: "AddBook.tsx", component: AddBook1 },
    { name: "BookList.tsx", component: BookList1 },
  ]}
/>

Component functions only ever take one argument, if any: the props object. The properties on this object are set when we use the component and provide attributes.

<FrameworkAside framework="react">

In React, it's common to use _destructuring assignment_ when accessing props inside a component. For example, our `Bookshelf` component may be written like this in React:

```tsx
function Bookshelf({ name }) {
  return (
    <div>
      <h1>{name}'s Bookshelf</h1>
      <Books />
      <AddBook />
    </div>
  );
}
```

**But destructuring `props` is usually a bad idea in Solid**. Under the hood, Solid uses _proxies_ to hook into `props` objects to know when a prop is accessed. When we destructure our props object in the function signature, we immediately access the object's properties and lose reactivity.

</FrameworkAside>

### Progress!

Thus far, we learned about Solid components, JSX, and built the foundation of a handy bookshelf app. Next, we'll bring our bookshelf to life using Solid's reactive system!