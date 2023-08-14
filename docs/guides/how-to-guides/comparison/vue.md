---
description: Solid не испытывает особого влияния Vue с точки зрения дизайна, но по подходу они сопоставимы. Они оба используют прокси в своей реактивной системе с автоматическим отслеживанием чтения
---

# Сравнение с Vue

Solid не испытывает особого влияния Vue с точки зрения дизайна, но по подходу они сопоставимы. Они оба используют прокси в своей реактивной системе с автоматическим отслеживанием чтения. Но на этом сходство заканчивается. Компактное определение зависимостей в Vue просто вливается в менее компактную систему виртуального DOM и компонентов, в то время как Solid сохраняет детализацию вплоть до прямого обновления DOM.

Vue ценит простоту, а Solid - прозрачность. Хотя направление развития Vue с выходом Vue 3 и введением API композиции больше соответствует подходу Solid. Со временем, в зависимости от дальнейшего развития, эти библиотеки могут еще больше сблизиться. Большинство примеров и ссылок на Vue в этом разделе будут сделаны в отношении API композиции, представленного в Vue 3.

## Шаблонные компоненты против функциональных компонентов

С точки зрения фронтенд-фреймворков компоненты служат для разделения пользовательского интерфейса на различные многократно используемые части кода, большинство приложений и сайтов в настоящее время представляют собой деревья вложенных компонентов.

### Vue

В Vue установка компонентов может осуществляться двумя способами: с помощью шаблонов или с помощью функций рендеринга. Наиболее распространенным на данный момент является использование шаблонов, например, так:

```vue
<script lang="ts" setup>
import { ref } from 'vue';

const count = ref(0);
</script>

<template>
    <button @click="count++">
        Count was pressed {{ count }} times
    </button>
</template>
```

Если вы не знакомы с ним, то это синтаксис шаблонов, используемый с API композиции в Vue. HTML используется в теге `<template></template>`, а Javascript или Typescript - в теге `<script></script>`. Для стилизации используется третий тег, который называется `<style></style>`.

### Solid

Solid использует только один вид компонентов - компонент-функцию. В отличие от Vue, в Solid нет различных синтаксисов, есть только один. Все компоненты и страницы в Solid представлены компонентами функций. Например:

```ts
import { createSignal } from 'solid-js';

const App = () => {
    const [count, setCount] = createSignal(0);
    return (
        <div>
            <button
                onclick={() =>
                    setCount(
                        (previousValue) => previousValue + 1
                    )
                }
            >
                You clicked me {count} times
            </button>
        </div>
    );
};

export default App;
```

Вот как выглядит компонент функции. Обратите внимание на основное различие между ним и синтаксисом шаблона, используемым в Vue. Также есть небольшие отличия в использовании переменных в HTML-части, например, одна фигурная скобка вместо двух. Синтаксис объявления и изменения переменных с состоянием также отличается, подробнее об этом позже.

## Reactivity and Statefulness

В Vue есть две ключевые функции, которые используются для объявления и управления реактивными и государственными значениями, `ref()` и `reactive()`, причем `ref()` может хранить такие примитивы, как булевы числа, строки и объекты, а `reactive()` - только объекты. В Solid имеются аналогичные функции, а именно `createSignal()` и `createStore()` соответственно. В этой части мы рассмотрим их различия в синтаксисе и методах обновления.

Синтаксис функций `ref()` и `createSignal()` или `reactive()` и `createStore()` не сильно отличается, фактически `reactive()` и `createStore()` похожи по синтаксису при обращении к значениям, но отличаются при изменении значений, например

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
    setStore(
        (prev) =>
            (prev = { ...prev, count: prev.count + 1 })
    ); // setting
}
```

С другой стороны, `ref()` и `createSignal()` отличаются как установкой, так и доступом к своим значениям, например:

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

**Ниже более подробно рассмотрено, как может отличаться синтаксис в простом приложении со счетчиком**.

### Vue

В Vue для работы с состоянием компонента Vue можно использовать функции `ref()` и `reactive()`. В некоторых случаях `reactive()` может служить простой альтернативой управлению состоянием вместо использования VueX. Вот краткий фрагмент того, как это может выглядеть:

```js
// store.js
import { reactive } from 'vue';

export const store = reactive({
    count: 0,
});
```

Выше мы создали наш магазин с помощью функции `reactive()`.

```vue
<!-- ComponentA.vue -->
<script setup>
import { store } from './store.js';

const unreactiveLocalValue = store.count;
//Note: this will not behave reactively
</script>

<template>
    <button @click="store.count + 1">
        add +1 to store.count
    </button>
</template>
```

Выше представлен компонент, в который импортирован магазин, чтобы изменить одно из значений в нем.

```vue
<!-- ComponentB.vue -->
<script setup>
import { store } from './store.js';

const unreactiveLocalValue = store.count;
//Note: this will not behave reactively
</script>

<template>From B: {{ store.count }}</template>
```

Это отдельный компонент, который будет отображать значение графа даже при его изменении.

Вы также можете использовать другие реактивные состояния, такие как `ref()` и `computed()`.

```js
// store.js
import { ref } from 'vue';

export const storeUsingRef = ref(0);
```

```vue
<!-- ComponentA.vue -->
<script setup>
import { computed } from 'vue';
import { storeUsingRef } from './store.js';

//Note: to make use of ref() values in the script section you will have to use the .value property like so:
const reactiveLocalValue = computed(
    () => storeUsingRef.value
);
//This will behave reactively
</script>

<template>
    <button @click="storeUsingRef + 1">
        add +1 to store
    </button>
</template>
```

---

```vue
<!-- ComponentB.vue -->
<script setup>
import { computed } from 'vue';
import { storeUsingRef } from './store.js';

//Note: to make use of ref() values in the script section you will have to use the .value property like so:
const reactiveLocalValue = computed(
    () => storeUsingRef.value
);
//This will behave reactively
</script>

<template>
    <h2>From B: {{ storeUsingRef }}</h2>
    <h2>From B: {{ reactiveLocalValue }}</h2>
</template>
```

Обратите внимание на комментарии в коде и на то, как меняется реактивность при использовании `reactive()` или `ref()`.

### Solid

В Solid мы можем сделать нечто подобное, используя примитивы `createStore()` и `createSignal()`. Помните, что `createStore()` принимает в качестве значений только объекты, а `createSignal()` может принимать булевы значения, объекты, строки и числа. Вот краткий пример того, как это может выглядеть, если вы решите использовать `createStore()` в качестве простого решения для управления состоянием

```js
// store.js
import { createStore } from 'solid-js/store';

export const [store, setStore] = createStore({ count: 0 });
```

Выше мы создали магазин с помощью функции `createStore()` в Solid.

```js
// Component A.jsx
import { setStore, store } from './store';

const ComponentA = () => {
    const unreactiveLocalValue = store.count;
    //Note: this will not behave reactively

    return (
        <div>
            <button
                onclick={() =>
                    setStore(
                        (prev) =>
                            (prev = {
                                ...prev,
                                count: prev.count + 1,
                            })
                    )
                }
            >
                add +1 to store
            </button>
        </div>
    );
};

export default ComponentA;
```

Приведенный выше компонент просто импортирует функции `setter` и `getter` магазина, чтобы установить и получить текущее состояние магазина.

```js
// Component B.jsx
import { store } from './store';

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

Это отдельный компонент, который служит для отображения текущего состояния хранилища и значений в нем.

Вы также можете использовать примитив `createSignal()`, например, так:

```js
//store.js
import { createSignal } from 'solid-js';

export const [storeUsingSignal, setStoreUsingSignal] =
    createSignal(0);
```

---

```js
// Component A.jsx
import {
    setStoreUsingSignal,
    storeUsingSignal,
} from './store';

const ComponentA = () => {
    const reactiveLocalValue = storeUsingSignal;
    // This 👆 will behave reactively

    const unreactiveLocalValue = storeUsingSignal();
    // This 👆 will not behave reactively

    return (
        <div>
            <button
                onclick={() =>
                    setStoreUsingSignal((prev) => prev + 1)
                }
            >
                add +1 to store
            </button>
        </div>
    );
};

export default ComponentA;
```

---

```js
// Component B.jsx
import { storeUsingSignal } from './store';

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

Обратите внимание на комментарии в коде и на то, как меняется реактивность при использовании `createStore()` или `createSignal()`.

## Условный рендеринг

В мире фронтенд-фреймворков условный рендеринг - это рендеринг компонента или узла только при выполнении определенного условия.

Например, допустим, вы хотите, чтобы вводимая форма отображалась только в том случае, если пользователь нажимает на кнопку show/hide, и скрывалась, если пользователь нажимает на эту кнопку еще раз. В этом случае можно просто использовать **булевую переменную** под названием `isShowing`, и если ее значение равно true, то форма будет показана, а если false - то скрыта.

Хорошо, теперь, когда мы кратко рассмотрели концепцию условного рендеринга, давайте посмотрим, как это может выглядеть в **Vue**, а затем в **Solid**.

### Vue

Если у вас есть опыт работы с Vue, я уверен, что вы должны быть знакомы с директивами `v-if`, `v-else` и `v-else-if`. Она используется для условного рендеринга самого себя и всего того, что она оборачивает. Ее можно использовать следующим образом:

```html
<h1 v-if="solidVotes < vueVotes">Vue is awesome!</h1>
<h1 v-else-if="solidVotes > vueVotes">
    Solid is more awesome 😁
</h1>
<h1 v-else>Oh no! No votes 😢</h1>
```

В приведенном выше блоке кода `Vue is awesome` выводится, если vueVotes больше, чем solidVotes, а `Solid is more awesome 😁` выводится, если наоборот, и в конце есть `else`, если ни то, ни другое не так.

### Solid

В Solid есть компоненты, подобные этим, которые очень упрощают условный рендеринг и выглядят гораздо чище. Это компоненты `Show`, `Switch` и `Dynamic`. Давайте рассмотрим, как их можно использовать.

#### Show

Этот компонент может использоваться для работы с простыми булевыми значениями и выражениями.

```js
<Show
    when={solidVotes() > vueVotes()}
    fallback={<h1>Vue has more votes</h1>}
>
    <h1>Solid has more votes</h1>
</Show>
```

В приведенном выше коде, если условие `when` не выполнено, то будет отрисован fallback.

#### Switch

Эта функция может быть использована для работы с переключателями и более сложными выражениями, в которых используется более двух взаимоисключающих исходов.

```js
<Switch fallback={<p>There haven't been any votes yet</p>}>
    <Match when={solidVotes() > vueVotes()}>
        <h1>Solid has more votes</h1>
    </Match>
    <Match when={solidVotes() < vueVotes()}>
        <h1>Vue has more votes</h1>
    </Match>
</Switch>
```

В приведенном выше коде, если ни одно из условий `when` не выполнено, будет отрисован fallback.

#### Dynamic

Этот встроенный компонент может быть использован для отрисовки элементов из карты ключей и компонентов/элементов. Он принимает свойство component и динамически отрисовывает любой переданный ему элемент или компонент. Вы можете воспользоваться этим преимуществом, передавая компонент динамически, используя карту и ключ. Приведем небольшой пример, демонстрирующий его использование

```js
import { render, Dynamic } from 'solid-js/web';
import { createSignal, For } from 'solid-js';

const RedThing = () => (
    <strong style="color: red">Red Thing</strong>
);
const GreenThing = () => (
    <strong style="color: green">Green Thing</strong>
);
const BlueThing = () => (
    <strong style="color: blue">Blue Thing</strong>
);

const options = {
    red: RedThing,
    green: GreenThing,
    blue: BlueThing,
};

function App() {
    const [selected, setSelected] = createSignal('red');

    return (
        <>
            <select
                value={selected()}
                onInput={(e) =>
                    setSelected(e.currentTarget.value)
                }
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

В приведенном выше коде выбор компонента для визуализации зависит от выбранного выпадающего варианта, который служит ключом к карте компонентов.

Не стесняйтесь экспериментировать и использовать те встроенные компоненты, которые вам удобны.

## Рендеринг списков

Очень часто разработчики сталкиваются с тем, что чаще всего используют списки. В том случае, если в приложении требуется итерация и рендеринг объектов, относящихся к спискам или массивам, некоторые фреймворки предлагают достаточно простой способ сделать это. Давайте рассмотрим, как это делают **Vue** и **Solid**.

### Vue

В Vue для вывода списка или массива элементов используется директива `for`, например, так:

```html
<li v-for="item in items">{{ item.message }}</li>
```

В приведенном выше коде Vue отображает столько элементов `<li></li>`, сколько элементов содержится в массиве items.

### Solid

В Solid компонент `<For>` является наилучшим способом циклического перемещения по массиву объектов. При изменении массива компонент `<For>` обновляет или перемещает элементы в DOM, а не создает их заново. Рассмотрим пример с использованием компонента `<For>`

```html
<For each="{items()}">
    {(item, i) => (
    <li>{item.message}</li>
    ) }
</For>
```

В компоненте `<For>` есть одно свойство, называемое each, в которое вы передаете массив, по которому хотите выполнить цикл.

## Vue vs. Solid

Приведем таблицу различий между хуками в Vue и хуками в Solid.

| Vue(composition)        | Vue(options)               | Solid            |
| ----------------------- | -------------------------- | ---------------- |
| `onMounted()`           | `mounted`                  | `onMount()`      |
| `onUnmounted()`         | `unmounted`                | `onCleanup()`    |
| `ref()`, `shallowRef()` | `data`                     | `createSignal()` |
| `reactive()`            | `data`                     | `createStore()`  |
| `computed()`            | `computed`                 | `createMemo()`   |
| `watchEffect()`         | `this.$watch()` or `watch` | `createEffect()` |

## Ссылки

-   [Comparison with Vue](https://docs.solidjs.com/guides/how-to-guides/comparison/vue)
