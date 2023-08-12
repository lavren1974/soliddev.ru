<Title>CSS Modules</Title>

CSS Modules are CSS files in which class names, animations, and media queries are scoped locally by default. CSS modules let you write your styles in CSS but Solid consumes them as Javascript objects. Once bundled class names referenced from CSS module files are given unique names in order to avoid selector name collisions. Another advantage of using CSS modules over normal CSS files is the fact that only selectors referenced are bundled.

We will be demonstrating how to make CSS Module files and how to make use of them in your Solid apps.

## Using CSS Modules

First, let’s start with creating a file named `style.module.css`, in this file you can create styles just like you would in normal CSS files. However, in CSS Module files you won’t be able to make use of HTML tag names (e.g div, h1, li, a, etc) as they will be regarded as impure selectors in CSS Module files.

**Note**: Module files are not only limited to the CSS file extension, you can also have \*.**module.(scss|sass|css)** file extensions. It’s all up to you

We can make use of our previous code in the `styles.css` file in our newly created CSS Module file since we’re only making use of class names as selectors

```css
/* styles.module.css */

.foo {
  color: red;
}

.bar {
  background-color: blue;
}
```

Let’s refactor some parts of our component file to make use of our CSS module file

```jsx
//component.jsx

import styles from “styles.module.css”

function Component() {
  return (
    <>
      <div class={`${styles.foo} ${styles.bar}`}>
        Hello, world!
      </div>
    </>
  )
}
```

If you would like to make use of only one of the styles, you can assign it to the class attribute as follows

```jsx
//component.jsx

import styles from “styles.module.css”

function Component() {
  return (
    <>
      <div class={styles.foo}>
        Hello, world!
      </div>
    </>
  )
}
```

You can also mix module syntax with normal string class names like so

```jsx
//component.jsx

import styles from “styles.module.css”

function Component() {
  return (
    <>
      <div class={`${styles.foo} container`}> // mix of css module and string class name
        Hello, world!
      </div>
    </>
  )
}
```

**Note**: If you’re wondering “how do I go about using styles with dashes in their names?” well don’t worry because we’ve got you. You can do that by using bracket notation like so `styles[“foo-with-dash”]` just like you would if you were querying keys of an object