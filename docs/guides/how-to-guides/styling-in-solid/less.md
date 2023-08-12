<Title>Less</Title>

LESS stands for Leaner CSS. LESS is a CSS preprocessor based on Javascript, LESS gives you the ability to make use of mixins and other programmatical tools. It helps in making your styling code cleaner and less redundant.

Let's take a look at how you can make use of LESS in your Solid apps

## Installing The Dependency

In order to make use of LESS files in your Solid app you'll need to install the dependency as a development dependency, like so

```bash
npm install --save-dev less
pnpm i --dev less           # Using pnpm
yarn add --dev less         # Using yarn
```

## Using LESS In Your App

Let's create a `.less` file in our `src` directory and name it `styles.less`

```less
//styles.less

.foo {
  color: red;
}

.bar {
  background-color: blue;
}
```

Take note of how the basic syntax is very similar to that of CSS. If you would like to declare variables in LESS you can do so as you usually would, for example

```less
//styles.less

@plainred: red;
@plainblue: blue;

.foo {
  color: @plainred;
}

.bar {
  background-color: @plainblue;
}
```

Write your LESS styles like you would anywhere else. Let's change the file extension of the `styles.css` import to `.less`.

```jsx
//component.jsx

import "./styles.less";

function Component() {
  return (
    <>
      <div class="foo bar">Hello, world!</div>
    </>
  );
}
```

By using `.less` as our styles file extension instead of `.css`, Vite (the build tool Solid uses) automatically recognized we are importing a LESS file and compiled LESS to CSS on demand.
