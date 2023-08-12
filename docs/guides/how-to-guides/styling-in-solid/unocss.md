<Title>UnoCSS</Title>

UnoCSS is an on demand utility CSS library. UnoCSS integrates with Solid as a Vite plugin.

## Install Vite Plugin

```sh
npm i --save-dev unocss
pnpm i --dev unocss   # Using pnpm
yarn add --dev unocss # Using yarn
```

## Import Vite Plugin

Once installed, open `vite.config.js` or `vite.config.ts`. The starter Solid Vite configuration looks like this:

````js
import { defineConfig } from "vite"

import solidPlugin from "vite-plugin-solid"

export default defineConfig({
	plugins: [
		solidPlugin(),
	],
	server: {
		port: 3000,
	},
	build: {
		target: "esnext",
	},
})
````

Now, `import unocssPlugin from "unocss/vite"` and invoke it as a function inside of plugins:

```diff
import { defineConfig } from "vite"

+ import unocssPlugin from "unocss/vite"
import solidPlugin from "vite-plugin-solid"

export default defineConfig({
	plugins: [
+		unocssPlugin(),
		solidPlugin(),
	],
	server: {
		port: 3000,
	},
	build: {
		target: "esnext",
	},
})
```

Note that `unocssPlugin` should be ordered before `solidPlugin`. This prevents some edge cases such as `&` from being escaped as `&amp;`.

## Import UnoCSS

Add `import "uno.css"` to `index.jsx` or `index.tsx`:

```diff
/* @refresh reload */
+ import "uno.css"

import { render } from 'solid-js/web';

import './index.css';
import App from './App';

render(() => <App />, document.getElementById('root') as HTMLElement);
```

You can also use the alias `import "virtual:uno.css"`; this is equivalent to `import "uno.css"`. `virtual` simply communicates that there is `uno.css` on the filesystem.

```diff
/* @refresh reload */
+ import "virtual:uno.css"

import { render } from 'solid-js/web';

import './index.css';
import App from './App';

render(() => <App />, document.getElementById('root') as HTMLElement);
```

{/* TODO: Add tip on dev tools and screenshot example */}

## Support

For more support, see the [UnoCSS/Vite integration guide](https://github.com/unocss/unocss/tree/main/packages/vite) or join the [offical UnoCSS Github discussion](https://github.com/unocss/unocss/discussions) and [Solid JS](https://discord.com/invite/solidjs) Discord channel. 👋
