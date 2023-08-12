<Title>WindiCSS</Title>

WindiCSS is an on demand utility CSS library. WindiCSS integrates with Solid as a Vite plugin.

## Install Vite Plugin

```sh
# Install (choose one)
npm i --save-dev vite-plugin-windicss windicss
pnpm i --dev vite-plugin-windicss windicss   # Using pnpm
yarn add --dev vite-plugin-windicss windicss # Using yarn
```

## Create a Configuration

WindiCSS is a configuration-based tool. Create `.windi.config.ts` at the root of your project directory. It should look something like this:

```ts
// windi.config.ts
import { defineConfig } from 'windicss/helpers'
import formsPlugin from 'windicss/plugin/forms'

export default defineConfig({
  darkMode: 'class',
  safelist: 'p-3 p-4 p-5',
  theme: {
    extend: {
      colors: {
        teal: {
          100: '#096',
        },
      },
    },
  },
  plugins: [formsPlugin],
})
```

## Import Vite Plugin

Once installed, open `vite.config.js` or `vite.config.ts`. The starter Solid Vite configuration looks like this:

```js
import { defineConfig } from "vite";

import solidPlugin from "vite-plugin-solid";

export default defineConfig({
  plugins: [solidPlugin()],
  server: {
    port: 3000,
  },
  build: {
    target: "esnext",
  },
});
```

Now, `import WindiCSS from "vite-plugin-windicss"` and invoke it as a function inside of plugins:

```diff
import { defineConfig } from "vite"

+ import WindiCSS from "vite-plugin-windicss"
import solidPlugin from "vite-plugin-solid"

export default defineConfig({
	plugins: [
+		WindiCSS(),
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

Note that `WindiCSS` should be ordered before `solidPlugin`. This prevents some edge cases such as `&` from being escaped as `&amp;`.

## Import WindiCSS

Add `import "virtual:windi.css"` to `index.jsx` or `index.tsx`. `virtual` simply communicates that there is `windi.css` on the filesystem.

```diff
/* @refresh reload */
+ import "virtual:windi.css"

import { render } from 'solid-js/web';

import './index.css';
import App from './App';

render(() => <App />, document.getElementById('root') as HTMLElement);
```

## Support

For more support, see the [WindiCSS/Vite integration guide](https://windicss.org/integrations/vite.html) or join the [offical WindiCSS](https://discord.com/invite/GTKYWq9zgA) and [Solid JS](https://discord.com/invite/solidjs) Discord channels. 👋
