<Title>Styling in Solid</Title>

Styling Solid apps is the same as styling HTML. So if you’ve ever used CSS or inline styles, you’ll already know how to style your Solid apps. 💡 This guide will teach you the basic concepts for styling your Solid apps as well as some advanced APIs such as `classList` and `JSX.CSSProperties`.

We also have guides for [CSS Modules](css-modules), [Sass](sass), [Tailwind CSS](tailwind-css), [UnoCSS](unocss), and [WindiCSS](windicss).

## The Basics

We use the `class` and `style` attributes to style elements. The `class` attribute, referred to as classes, are useful for declaratively styling one or more elements using CSS (Cascading Stylesheets). The `style` attribute, referred to as inline styles, are useful for imperatively styling one and only one element.

In general, we advise you to use classes as much as possible as they are statically optimized and self-documenting. That being said, inline styles are useful for prototyping, setting CSS variables using runtime values, and overriding classes. Use what makes sense for you and don’t be afraid to experiment. 🙂

## Hands On with Inline Styles

The `style` attribute provides a way to imperatively style one and only one element and set CSS variables using runtime values. Solid supports using the `style` attribute as both strings and objects. In practice, this means you can write `<div style="color: red;">` and `<div style={{ "color": "red" }}>`. These inputs provide the same outputs.

Style strings are more concise and portable (you can use them in and outside of Solid). Style objects are more verbose but offer benefits such as intellisense for autocompletion and type safety (if you are using an IDE such as VS Code — no added extensions are needed). If you want portability, styling strings are probably better for you. If you want intellisense such as autocompletion, type safety, etc. style objects are probably better for you.

Note that style strings and style objects always use `kebab-case` syntax. This means `<div style={{ backgroundColor: "red" }}>` interpolates as `<div style="backgroundColor: red;">` which is invalid CSS. Solid always interpolates CSS properties as-is.

This code should give you an idea of what styling using style strings and style objects looks like:

```jsx
// This component is styled using inline styles as strings.
function StyledStringComponent() {
  const backgroundColor = "yellow";
  const fontWeight = "bold";

  return (
    <>
      <div style={`background-color: ${backgroundColor};`}>
        <h1 style={`color: red; font-weight: ${fontWeight};`}>
          Hello World in red
        </h1>

        <h1 style="color: rgb(0, 255, 0); padding: 20px;">
          Hello World in green
        </h1>

        <h1 style="color: blue;">Hello World in blue</h1>
      </div>
    </>
  );
}

// This component is styled using inline styles as objects.
function StyledObjectComponent() {
  const backgroundColor = "yellow";
  const fontWeight = "bold";

  return (
    <>
      <div style={{ "background-color": backgroundColor }}>
        <h1 style={{ color: "red", "font-weight": fontWeight }}>
          Hello World in red
        </h1>

        <h1 style={{ color: "rgb(0, 255, 0)", padding: "20px" }}>
          Hello World in green
        </h1>

        <h1 style={{ color: "blue" }}>Hello World in blue</h1>
      </div>
    </>
  );
}
```

Note that every CSS property can be used as inline styles, ranging from simple `background-color` all the way to complex `mask-mode`.

## Hands On with Classes

The `class` attribute provides a way to declaratively style one or more elements using CSS (Cascading Stylesheets). Additionally, Solid supports `classList` to conditionally toggle classes based on runtime values.

In general, we advise you to use classes as much as possible as they are statically optimized and self-documenting. That being said, inline styles are useful for prototyping, setting CSS variables using runtime values, and overriding classes. Use what makes sense for you and don’t be afraid to experiment. 🙂

If you are just getting started, you can colocate `<style>` tags to help you prototype classes side-by-side with your HTML. There are several reasons you wouldn’t want to do this for production, but for prototyping it’s a useful way to get started. Note that the community provides packages such as [solid-styled](https://github.com/lxsmnsyc/solid-styled) which scopes CSS much the same way as scoped styles in Vue, Svelte, and Astro.

Here’s an example of a simple card component:

```jsx
// Card.jsx
function Card() {
  return (
    <>
      <style>{`
      .grid { display: grid; }
      .grid.grid-center { place-items: center; }
      .screen { min-height: 100vh; }

      .card {
        height: 160px;
        aspect-ratio: 2;
        border-radius: 16px;
        background-color: white;
        box-shadow: 0 0 0 4px hsl(0 0% 0% / 15%);
      }
    `}</style>

      <div class="grid grid-center screen">
        <div class="card">Hello, world!</div>
      </div>
    </>
  );
}
```

Let’s say we’re ready to extract our CSS to a separate file. We can copy-paste the contents of our `<style>` tag and extract them to `card.css`. Now our code looks like this:

```css
/* Card.css */
.grid {
  display: grid;
}
.grid.grid-center {
  place-items: center;
}
.screen {
  min-height: 100vh;
}

.card {
  height: 160px;
  aspect-ratio: 2;
  border-radius: 16px;
  background-color: white;
  box-shadow: 0 0 0 4px hsl(0 0% 0% / 15%);
}
```

```jsx
// Card.jsx
import "./Card.css";

function Card() {
  return (
    <>
      <div class="grid grid-center screen">
        <div class="card">Hello, world!</div>
      </div>
    </>
  );
}
```

Provided that `card.css` and `Card.jsx` are in the same folder, `Card.jsx` can import `card.css` using relative syntax. Relative syntax is when a file starts with `./`. Colocating CSS files next to component files is generally a good strategy for organization. 👍

By importing `"./card.css"`, we’ve imported classes `.grid`, `.grid.center`, `.screen`, `.card` as globally scoped CSS. This means any elements that use one or more of these class names will automatically inherit these styles. There are pros and cons to this approach, but if you are interested in automatically scoping classes to the importer, you can use [CSS Modules](css-modules).

For global CSS not specific to components, we recommend creating and maintaining `src/index.css`, imported by `src/index.jsx`. For component CSS, we recommend creating and maintaining `src/components/<component>.css`, imported by `src/components/<Component>.jsx`.

For example:

```
src/
  index.css
  index.jsx (imports index.css)
  components/
    nav.css
    Nav.jsx (imports nav.css)
```
