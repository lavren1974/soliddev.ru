---
description: Solid Router - это простой и удобный в использовании универсальный маршрутизатор для приложений Solid. Он работает как на клиенте, так и на сервере
---

# Solid Router

В современной веб-разработке маршрутизатор - это программный компонент, отвечающий за обработку клиентских запросов и определяющий, какой компонент следует отобразить, с помощью маршрутизации на стороне сервера или на стороне клиента. При поступлении запроса от клиента маршрутизатор оценивает URL и решает, какой контроллер или компонент на стороне сервера должен обработать этот запрос или быть отображен.

**Solid Router** - это простой и удобный в использовании универсальный маршрутизатор для приложений Solid. Он работает как на клиенте, так и на сервере. Маршруты задаются с помощью простого синтаксиса JSX и могут быть вложенными. Однако маршруты можно передавать и в виде объекта конфигурации.

Solid Router обладает множеством замечательных возможностей, одной из которых является поддержка вложенной маршрутизации, позволяющей изменять определенную часть компонента, а не заменять его полностью.

Он поддерживает все методы Solid SSR и имеет встроенные переходы Solid, поэтому его можно свободно использовать с суспензиями, ресурсами и "ленивыми" компонентами. Solid Router также позволяет определить функцию данных, которая загружается параллельно с маршрутами ([render-as-you-fetch](https://epicreact.dev/render-as-you-fetch/)).

!!!note ""

    Дополнительную информацию о Solid Router можно найти на [Solid Router GitHub page](https://github.com/solidjs/solid-router#getting-started).

## Начало работы

Для того чтобы начать работу с Solid Router, необходимо установить его в проект, поскольку по умолчанию он не установлен. После этого необходимо настроить маршрутизатор и определить некоторые маршруты.

### Установка

Давайте перейдем к установке маршрутизатора. Для этого нам нужно установить маршрутизатор с помощью NPM, Yarn или вашего любимого менеджера пакетов.

```bash
npm install @solidjs/router
# or
yarn add @solidjs/router
# or
pnpm i @solidjs/router
```

### Настройка

Теперь, когда маршрутизатор установлен, мы можем его настроить и определить некоторые маршруты. Начнем с импорта маршрутизатора в наш корневой файл `Index`.

```js
import { render } from 'solid-js/web';
import App from './App';
import { Router } from '@solidjs/router'; // 👈 Import the router

render(
    () => (
        <Router>
            {' '}
            {/* 👈 Wrap the router around the app */}
            <App />
        </Router>
    ),
    document.getElementById('app')
);
```

Теперь, когда маршрутизатор настроен, мы можем определить некоторые маршруты. Начнем с того, что сделаем компонент `App` нашей домашней страницей или маршрутом `/`.

```js
import { render } from 'solid-js/web';
import App from './App';
import { Router, Route, Routes } from '@solidjs/router';

render(
    () => (
        <Router>
            <Routes>
                <Route path="/" component={App} />{' '}
                {/* 👈 Define the home page route */}
            </Routes>
        </Router>
    ),
    document.getElementById('app')
);
```

В приведенном выше коде мы импортировали компоненты `Route` и `Routes` из Solid Router и определили маршрут для главной страницы. Компонент `Route` принимает свойство `path`, которое представляет собой путь URL, которому будет соответствовать маршрут. Свойство `component` - это компонент, который будет отображаться при совпадении с маршрутом. Компонент `Routes` используется для группировки маршрутов и необходим для вложенных маршрутов. Этот компонент используется для того, чтобы показать, где должны быть отображены маршруты.

Вы можете добавить несколько маршрутов в компонент `Routes`, и они будут отображаться каждый раз, когда URL-адрес будет соответствовать пути маршрута. Добавим маршрут для страниц `/about` и `/contact`.

```js
import { render } from 'solid-js/web';
import App from './App';
import About from './About';
import Contact from './Contact';
import { Router, Route, Routes } from '@solidjs/router';

render(
    () => (
        <Router>
            <Routes>
                <Route path="/" component={App} />
                <Route
                    path="/about"
                    component={About}
                /> {/* 👈 Define the about page route */}
                <Route
                    path="/contact"
                    component={Contact}
                />{' '}
                {/* 👈 Define the contact page route */}
            </Routes>
        </Router>
    ),
    document.getElementById('app')
);
```

!!!note "Примечание"

    Если вы хотите лениво загружать свои компоненты, чтобы они загружались только при совпадении маршрута, вы можете использовать функцию `lazy` из `solid-js` для ленивой загрузки своих компонентов.

```js
import { render } from 'solid-js/web';
import App from './App';
import { Router, Route, Routes } from '@solidjs/router';
import { lazy } from 'solid-js';

const About = lazy(() => import('./About'));
const Contact = lazy(() => import('./Contact'));

render(
    () => (
        <Router>
            <Routes>
                <Route path="/" component={App} />
                <Route
                    path="/about"
                    component={About}
                /> {/* 👈 Define the about page route */}
                <Route
                    path="/contact"
                    component={Contact}
                />{' '}
                {/* 👈 Define the contact page route */}
            </Routes>
        </Router>
    ),
    document.getElementById('app')
);
```

Теперь, когда определены маршруты, мы можем переходить на страницы `/about` и `/contact`. Давайте добавим несколько ссылок в компонент `App`, чтобы можно было переходить на другие страницы. Для добавления ссылок в маршруты можно использовать компонент `A` из Solid Router. Добавим ссылки на страницы `/about` и `/contact`.

```js
import styles from './App.module.css';
import { A } from '@solidjs/router'; // 👈 Import the A component

const App = () => {
    return (
        <div class={styles.App}>
            <header class={styles.header}>
                <p>
                    Edit <code>src/App.tsx</code> and save
                    to reload.
                </p>
                <a
                    class={styles.link}
                    href="https://github.com/solidjs/solid"
                    target="_blank"
                    rel="noopener noreferrer"
                >
                    Learn Solid
                </a>
                <A href="/about">About</A>{' '}
                {/* 👈 Add a link to the about page */}
                <A href="/contact">Contact</A> {/* 👈 Add a link to the contact page */}
            </header>
        </div>
    );
};

export default App;
```

## Создание ссылок на другие маршруты

### Компонент `A`

Компонент `A` используется для создания ссылок на другие маршруты в вашем приложении. Компонент `A` также принимает свойство `href`, которое представляет собой URL-путь, по которому будет осуществляться переход по ссылке. Компонент `A` также принимает свойство `activeClass`, которое представляет собой класс, который будет применен к ссылке, когда текущий путь маршрута совпадет с путем ссылки.

Вот краткий пример того, как может выглядеть базовое приложение Solid с Solid Router и как использовать компонент `A`.

```js
import { render } from 'solid-js/web';
import App from './App';
import About from './About';
import Contact from './Contact';
import { Router, Route, Routes, A } from '@solidjs/router';

render(
    () => (
        <Router>
            <Routes>
                <Route path="/" component={App} />
                <Route path="/about" component={About} />
                <Route
                    path="/contact"
                    component={Contact}
                />
            </Routes>
        </Router>
    ),
    document.getElementById('app')
);
```

Приведем пример использования компонента `A` и его свойства `activeClass`.

```js
import styles from './App.css';
import { A } from '@solidjs/router';

const App = () => {
    return (
        <div class={styles.App}>
            <header class={styles.header}>
                <p>
                    Edit <code>src/App.tsx</code> and save
                    to reload.
                </p>
                <A
                    class={styles.link}
                    href="/about"
                    activeClass="underlined" // 👈 Add the active class
                >
                    About
                </A>
                <A
                    class={styles.link}
                    href="/contact"
                    activeClass="underlined" // 👈 Add the active class
                >
                    Contact
                </A>
            </header>
        </div>
    );
};

export default App;
```

!!!note "Примечание"

    Будьте внимательны при использовании свойства **`activeClass`**, поскольку по умолчанию в соответствие включаются маршруты, являющиеся потомками или иным образом вложенными в совпадающий путь (например, `/about` будет соответствовать `/about/me` и `/about/me/you`). Однако можно использовать булево свойство **``end`** для точного соответствия пути (например, `/about` будет соответствовать только `/about`). Свойство **`end`** особенно полезно для ссылок на корневой маршрут `/`, который будет соответствовать всем путям.

Вот список всех свойств, которые принимает компонент `A`.

| Свойства        | Тип       | Описание                                                                                                                                                                                                |
| --------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `href`          | `string`  | Путь маршрута, на который нужно перейти. Он будет разрешен относительно маршрута, в котором находится ссылка, но перед ним можно поставить `/`, чтобы вернуться к корню.                                |
| `activeClass`   | `string`  | Класс, который будет применяться к ссылке, когда текущий путь маршрута совпадает с путем ссылки.                                                                                                        |
| `end`           | `boolean` | Если `true`, то ссылка считается активной только тогда, когда текущее местоположение точно совпадает с `href`; если `false`, то проверяется, начинается ли текущее местоположение с `href`              |
| `noScroll`      | `boolean` | Если `true`, то отключается стандартное поведение прокрутки к верху новой страницы                                                                                                                      |
| `replace`       | `boolean` | Если `true`, то не добавлять новую запись в историю браузера. (По умолчанию новая страница будет добавлена в историю браузера, поэтому нажатие кнопки назад приведет к переходу на предыдущий маршрут). |
| `state`         | `unknown` | [Вставить это значение](https://developer.mozilla.org/ru/docs/Web/API/History/pushState) в стек истории при навигации                                                                                   |
| `inactiveClass` | `string`  | Класс, который должен отображаться, когда ссылка неактивна (когда текущее местоположение не соответствует ссылке)                                                                                       |

### Компонент `Navigate`

Solid Router предоставляет компонент `Navigate`, который работает аналогично компоненту `A`, но сразу после отображения компонента осуществляет переход по указанному пути. Он также использует свойство `href`, но у вас есть дополнительная возможность передать в `href` функцию, возвращающую путь для перехода:

```js
function getPath({ navigate, location }) {
    //navigate is the result of calling useNavigate(); location is the result of calling useLocation().
    //You can use those to dynamically determine a path to navigate to
    return '/some-path';
}

//Navigating to /redirect will redirect you to the result of getPath
<Route
    path="/redirect"
    element={<Navigate href={getPath} />}
/>;
```

## Динамические маршруты

Если путь не известен заранее, можно рассматривать часть пути как гибкий параметр, который передается компоненту. Например, если необходимо перейти к записи блога с определенным идентификатором, можно использовать динамический маршрут.

Вот краткий пример создания динамического маршрута.

```js
import { render } from 'solid-js/web';
import App from './App';
import About from './About';
import User from './User';
import Contact from './Contact';
import { Router, Route, Routes, A } from '@solidjs/router';

render(
    () => (
        <Router>
            <Routes>
                <Route path="/" component={App} />
                <Route path="/about" component={About} />
                <Route
                    path="/user/:id"
                    component={User}
                />{' '}
                {/* 👈 Add a dynamic route */}
                <Route
                    path="/contact"
                    component={Contact}
                />
            </Routes>
        </Router>
    ),
    document.getElementById('app')
);
```

В приведенном выше фрагменте кода мы добавили динамический маршрут с именем `user`, который принимает параметр `:id`. Параметр `:id` будет передаваться компоненту `User` через примитив `useParams`. Подробнее о примитивах Solid Router мы поговорим позже.

Приведем пример использования динамического маршрута в компоненте `User`.

```js
import styles from './User.module.css';
import { useParams } from '@solidjs/router';

const User = () => {
    const params = useParams(); // 👈 Get the dynamic route parameters

    return (
        <div class={styles.User}>
            <header class={styles.header}>
                <p>
                    Edit <code>src/User.tsx</code> and save
                    to reload.
                </p>
                <A class={styles.link} href="/">
                    Home
                </A>
                <A class={styles.link} href="/about">
                    About
                </A>
                <A class={styles.link} href="/contact">
                    Contact
                </A>
                <p>
                    This is the user with the id of{' '}
                    <code>{params.id}</code>{' '}
                    {/* 👈 Access the dynamic route parameter */}
                </p>
            </header>
        </div>
    );
};

export default User;
```

Для доступа к динамическим параметрам маршрута можно использовать объект `params`. В приведенном примере мы получили доступ к параметру `id`. Параметры можно использовать даже в таких примитивах, как `createResource` и `createSignal`:

Вот пример того, как это может выглядеть в примитиве `createResource`:

```js
import { createResource, createSignal } from 'solid-js';
import { useParams } from '@solidjs/router';

// 👇 This is an asynchronous function that fetches a user from the jsonplaceholder API
async function fetchUser(id) {
    const response = await fetch(
        `https://jsonplaceholder.typicode.com/users/${id}`
    );
    return response.json();
}

const User = () => {
    const params = useParams();
    const [data] = createResource(params.id, fetchUser); // 👈 Pass the dynamic route parameter to the createResource primitive

    return (
        <div>
            {data.loading ? ( // 👈 Use the loading property to show a loading indicator
                <p>Loading...</p>
            ) : (
                <div>
                    <p>Name: {data().name}</p>{' '}
                    {/* 👈 Access the data returned from the fetchUser function */}
                    <p>Email: {data().email}</p>
                    <p>Phone: {data().phone}</p>
                </div>
            )}
        </div>
    );
};

export default User;
```

В приведенном выше фрагменте кода мы передали параметр `id` примитиву `createResource`. Это означает, что функция `fetchUser` будет вызываться каждый раз при изменении параметра `id`. Это удобно, если необходимо получать новые данные о пользователе при каждом изменении параметра `id`.

### Необязательные параметры

Вы также можете сделать параметр необязательным, добавив символ `?` после имени параметра. Например, если вы хотите сделать параметр `id` необязательным, то сделайте следующее:

```jsx
import { render } from 'solid-js/web';
import App from './App';
import About from './About';
import User from './User';
import Contact from './Contact';
import { Router, Route, Routes, A } from '@solidjs/router';

render(
    () => (
        <Router>
            <Routes>
                <Route path="/" component={App} />
                <Route path="/about" component={About} />
                <Route
                    path="/user/:id?"
                    component={User}
                />{' '}
                {/* 👈 Make the id parameter optional */}
                <Route
                    path="/contact"
                    component={Contact}
                />
            </Routes>
        </Router>
    ),
    document.getElementById('app')
);
```

Необязательный маршрут `/user/:id?` будет соответствовать `/user` и `/user/123`, но не `/user/123/comment`.

### Маршруты/параметры диких символов

Маршруты с подстановочными знаками - это маршруты, которые соответствуют любому пути-потомку в пределах заданного пути.

Параметр также можно сделать подстановочным, добавив `*` после имени параметра. Например, если необходимо сделать параметр `id` подстановочным, то можно сделать следующее:

```js
import { render } from 'solid-js/web';
import App from './App';
import About from './About';
import User from './User';
import Contact from './Contact';
import { Router, Route, Routes, A } from '@solidjs/router';

render(
    () => (
        <Router>
            <Routes>
                <Route path="/" component={App} />
                <Route path="/about" component={About} />
                <Route
                    path="/user/*"
                    component={User}
                />{' '}
                {/* 👈 Make the id parameter a wildcard */}
                <Route
                    path="/contact"
                    component={Contact}
                />
            </Routes>
        </Router>
    ),
    document.getElementById('app')
);
```

Маршрут с подстановочным символом `/user/*` будет соответствовать маршрутам `/user`, `/user/123`, `/user/123/comment`, `/user/123/comment/456` и т.д.

Если вы хотите получить доступ к параметру wildcard, вы можете назвать его:

```js
import { render } from 'solid-js/web';
import App from './App';
import About from './About';
import User from './User';
import Contact from './Contact';
import { Router, Route, Routes, A } from '@solidjs/router';

render(
    () => (
        <Router>
            <Routes>
                <Route path="/" component={App} />
                <Route path="/about" component={About} />
                <Route
                    path="/user/*id"
                    component={User}
                />{' '}
                {/* 👈 Name the wildcard parameter */}
                <Route
                    path="/contact"
                    component={Contact}
                />
            </Routes>
        </Router>
    ),
    document.getElementById('app')
);
```

Доступ к параметру `wildcard` осуществляется так же, как и к параметру динамического маршрута:

```jsx
import styles from './User.module.css';
import { useParams } from '@solidjs/router';

const User = () => {
    const params = useParams(); // 👈 Get the wildcard route parameters

    return (
        <div class={styles.User}>
            <header class={styles.header}>
                <p>
                    Edit <code>src/User.tsx</code> and save
                    to reload.
                </p>
                <A class={styles.link} href="/">
                    Home
                </A>
                <A class={styles.link} href="/about">
                    About
                </A>
                <A class={styles.link} href="/contact">
                    Contact
                </A>
                <p>
                    This is the wildcard parameter{' '}
                    <code>{params.id}</code>{' '}
                    {/* 👈 Access the wildcard parameter */}
                </p>
            </header>
        </div>
    );
};

export default User;
```

!!!note "Примечание"

    Токен `*` должен быть последним в пути маршрута. Например, `/user/*id` является допустимым, а `/user/id*` и `/user/*id/foo` - нет.

### Множественные пути

Маршруты также поддерживают определение нескольких путей с помощью массива. Это позволяет маршруту оставаться смонтированным и не перерисовываться при переключении между двумя или более местами, которым он соответствует:

```js
//Navigating from login to register does not cause the Login component to re-render
<Route path={['login', 'register']} component={Login} />
```

## Функции данных {#data-functions}

В предыдущих примерах компонент User загружается в ленивом режиме, а затем происходит выборка данных. С помощью функций данных маршрута мы можем начать получать данные параллельно с загрузкой маршрута, чтобы использовать их как можно быстрее.

Для этого создайте функцию, которая получает и возвращает данные с помощью `createResource`. Затем передайте эту функцию в свойство `data` компонента `Route`.

=== "example.jsx"

    ```js
    import { lazy } from 'solid-js';
    import { Route } from '@solidjs/router';
    import User from './pages/users/[id].js';
    import { fetchUser } from './fetchUser'; // Import the your fetchUser function

    const User = lazy(() => import('./pages/users/[id].js'));

    //Data function
    function UserData({ params, location, navigate, data }) {
    	const [user] = createResource(
    		() => params.id,
    		fetchUser
    	); // 👈 Pass the id parameter to the fetchUser function
    	return user;
    }

    //Pass it in the route definition
    <Route
    	path="/users/:id"
    	component={User}
    	data={UserData}
    />;
    ```

=== "example.tsx"

    ```ts
    import { lazy } from 'solid-js';
    import { Route } from '@solidjs/router';
    import User from './pages/users/[id].ts';
    import { fetchUser } from './fetchUser'; // Import the your fetchUser function

    const User = lazy(() => import('./pages/users/[id].ts'));

    //Data function
    function UserData({
    	params,
    	location,
    	navigate,
    	data,
    }: RouteDataFuncArgs) {
    	const [user] = createResource(
    		() => params.id,
    		fetchUser
    	); // 👈 Pass the id parameter to the fetchUser function
    	return user;
    }

    //Pass it in the route definition
    <Route
    	path="/users/:id"
    	component={User}
    	data={UserData}
    />;
    ```

Функция `data` будет вызвана после загрузки маршрута, а доступ к результату можно получить, вызвав `useRouteData` в компоненте маршрута.

=== "/pages/users/[id].jsx"

    ```js
    import { useRouteData } from '@solidjs/router';

    const user = useRouteData();

    return <h1>{user().name}</h1>;
    ```

=== "/pages/users/[id].tsx"

    ```ts
    import { useRouteData } from '@solidjs/router';

    const user = useRouteData<RouteDataFunctionType>();

    return <h1>{user().name}</h1>;
    ```

Поскольку функция data имеет только один аргумент, которым является объект, содержащий информацию о маршруте, например

| Ключ       | Тип                                               | Описание                                                                                                                             |
| ---------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `params`   | object                                            | Параметры маршрута (то же значение, что и при вызове функции useParams() внутри компонента маршрута)                                 |
| `location` | `{ pathname, search, hash, query, state, key}`    | Объект, который можно использовать для получения дополнительной информации о пути (соответствует [useLocation()](#uselocation))      |
| `navigate` | `(to: string, options?: NavigateOptions) => void` | Функция, которую можно вызвать для перехода к другому маршруту (соответствует [useNavigate()](#usenavigation))                       |
| `data`     | unknown                                           | Данные, возвращаемые родительской функцией data, если таковая имеется. (Данные будут проходить через все промежуточные вложенности.) |

Общим шаблоном является экспорт функции данных, соответствующей маршруту, в специальный файл `route.data.js|ts`. Таким образом, функция данных может быть импортирована без загрузки чего-либо еще.

```diff
  import { lazy } from "solid-js";
  import { Route } from "@solidjs/router";
- import { fetchUser } ...
+ import UserData from "./pages/users/[id].data.js";
  const User = lazy(() => import("/pages/users/[id].js"));

  // In the Route definition
  <Route path="/users/:id" component={User} data={UserData} />;
```

Импортировать функцию fetchUser больше не нужно, поскольку она используется не здесь, а в файле `[id].data.js`.

## Вложенные маршруты

Вложенные маршруты - это еще один способ определения маршрутов. Они полезны, когда у вас много маршрутов и вы хотите сгруппировать их вместе. Они также позволяют определить компонент макета, который будет использоваться для всех вложенных маршрутов.

Приведем небольшой пример кода, показывающий разницу между обычными и вложенными определениями маршрутов:

```js
<Route path="/users/:id" component={User} />  {/*// Normal route definition */}
```

---

```js
<Routes path="/users">
  <Route path="/:id" component={User} /> {/*// Nested route definition */}
</Route>
```

`/users/:id` отображает компонент `<User/>`, а `/users/` - пустой маршрут.

Маршрут задается только для листовых узлов Route (внутренних компонентов `Route`). Если вы хотите сделать родительским свой собственный маршрут, то его необходимо указать отдельно:

```js
//This won't work the way you'd expect
<Route path="/users" component={Users}>
  <Route path="/:id" component={User} />
</Route>

//This works
<Route path="/users" component={Users} />
<Route path="/users/:id" component={User} />

//This also works
<Route path="/users">
  <Route path="/" component={Users} />
  <Route path="/:id" component={User} />
</Route>
```

Первое определение Nested Route не будет работать так, как вы ожидаете, поскольку оно работает совершенно по-другому, используя компонент `<Outlet/>`. Компонент `<Outlet/>` используется для отображения дочерних маршрутов родительского маршрута. Компонент `<Outlet/>` отображается только тогда, когда активен родительский маршрут. Это означает, что компонент `<User/>` будет отображаться только при активном маршруте `/users/:id`. Вот как это выглядит:

```js
import { Outlet } from '@solidjs/router';

function PageWrapper() {
    return (
        <div>
            <h1> We love our users! </h1>
            <Outlet />
            <A href="/">Back Home</A>
        </div>
    );
}

<Route path="/users" component={PageWrapper}>
    <Route path="/" component={Users} />
    <Route path="/:id" component={User} />
</Route>;
```

Конфигурация маршрутов остается прежней, но теперь элементы маршрута будут появляться внутри родительского элемента, в котором был объявлен `<Outlet/>`.

Вкладывать элементы можно до бесконечности - только помните, что только листовые узлы станут собственными маршрутами. В данном примере создан только один маршрут `/layer1/layer2`, и он отображается в виде трех вложенных div'ов.

```js
<Route
    path="/"
    element={
        <div>
            Onion starts here <Outlet />
        </div>
    }
>
    <Route
        path="layer1"
        element={
            <div>
                Another layer <Outlet />
            </div>
        }
    >
        <Route
            path="layer2"
            element={<div>Innermost layer</div>}
        ></Route>
    </Route>
</Route>
```

Если объявить функцию данных на родительском и дочернем маршрутах, то результат работы родительской функции данных будет передан в дочернюю функцию данных в качестве свойства `data` аргумента, как это было описано в предыдущем разделе. Это работает, даже если маршрут не является прямым дочерним, поскольку по умолчанию каждый маршрут пересылает данные своего родителя.

## Маршрутизатор с хэш-режимом

По умолчанию Solid Router использует `location.pathname` в качестве пути маршрута. Вы можете просто переключиться в хэш-режим через свойство `source` компонента `<Router>`.

```js
import { Router, hashIntegration } from '@solidjs/router';

<Router source={hashIntegration()}>
    <App />
</Router>;
```

## Маршрутизация на основе конфигурации

Для определения маршрутов необязательно использовать JSX. Для определения маршрутов можно также использовать объект config. Это удобно, когда вы хотите определить маршруты в отдельном файле и импортировать их в приложение, а затем передать их в `useRoutes`.

```js
import { lazy } from 'solid-js';
import { render } from 'solid-js/web';
import { Router, useRoutes, A } from '@solidjs/router';

// Define your routes
const routes = [
    {
        path: '/users',
        component: lazy(() => import('/pages/users.js')),
    },
    {
        path: '/users/:id',
        component: lazy(() =>
            import('/pages/users/[id].js')
        ),
        children: [
            {
                path: '/',
                component: lazy(() =>
                    import('/pages/users/[id]/index.js')
                ),
            },
            {
                path: '/settings',
                component: lazy(() =>
                    import('/pages/users/[id]/settings.js')
                ),
            },
            {
                path: '/*all',
                component: lazy(() =>
                    import('/pages/users/[id]/[...all].js')
                ),
            },
        ],
    },
    {
        path: '/',
        component: lazy(() => import('/pages/index.js')),
    },
    {
        path: '/*all',
        component: lazy(() => import('/pages/[...all].js')),
    },
];

function App() {
    const Routes = useRoutes(routes); // 👈 useRoutes takes in the routes config
    return (
        <>
            <h1>Awesome Site</h1>
            <A class="nav" href="/">
                Home
            </A>
            <A class="nav" href="/users">
                Users
            </A>
            <Routes />
        </>
    );
}

render(
    () => (
        <Router>
            <App />
        </Router>
    ),
    document.getElementById('app')
);
```

## Примитивы маршрутизатора

Solid Router предоставляет ряд примитивов, которые считывают контекст Router и Route.

### `useParams`

Получает реактивный объект типа store, содержащий текущие параметры маршрута, определенные в Route.

```js
import { useParams } from '@solidjs/router';

const params = useParams();

// fetch user based on the id path parameter
const [user] = createResource(() => params.id, fetchUser);
```

### `useNavigation`

Этот метод извлекает метод для императивной навигации. Он возвращает функцию `navigate`, которая принимает путь и необязательный объект options. Объект options может содержать свойство `replace`, чтобы заменить текущую запись истории вместо того, чтобы проталкивать новую.

```js
const navigate = useNavigate();

if (unauthorized) {
    navigate('/login', { replace: true });
}
```

Вот список доступных опций:

-   resolve (_boolean_, по умолчанию `true`): преобразовать путь к текущему маршруту
-   replace (_boolean_, по умолчанию `false`): заменить запись в истории
-   scroll (_boolean_, по умолчанию `true`): прокрутка к верху после навигации
-   state (_any_, по умолчанию `undefined`): передавать пользовательское состояние в location.state

!!!note ""

    Примечание: Сериализация состояния осуществляется с использованием [structured clone algorithm](https://developer.mozilla.org/ru/docs/Web/API/Web_Workers_API/Structured_clone_algorithm), который поддерживает не все типы объектов.

### `useLocation`

Извлекает реактивный объект `location`, полезный для получения таких вещей, как `pathname`, `search`, `hash`, `state` и т. д.

```js
import { useLocation } from '@solidjs/router';

const location = useLocation();

const pathname = createMemo(() => location.pathname);
```

### `useSearchParams`

Получает кортеж, содержащий реактивный объект для чтения параметров запроса текущего местоположения и метод для их обновления. Объект является прокси, поэтому для подписки на реактивные обновления необходимо получить доступ к свойствам. Обратите внимание, что значения будут строками, а имена свойств сохранят свой регистр.

Метод setter принимает объект, записи которого будут объединены в текущую строку запроса. Значения типа `''`, `undefined` и `null` будут удалять ключ из результирующей строки запроса. Обновления будут вести себя так же, как навигация, и сеттер принимает тот же необязательный второй параметр, что и `navigate`, а автопрокрутка по умолчанию отключена.

```js
const [searchParams, setSearchParams] = useSearchParams();

return (
    <div>
        <span>Page: {searchParams.page}</span>
        <button
            onClick={() =>
                setSearchParams({
                    page: searchParams.page + 1,
                })
            }
        >
            Next Page
        </button>
    </div>
);
```

### `useIsRouting`

Получает сигнал, указывающий, находится ли маршрут в данный момент в состоянии перехода. Полезно для отображения состояния "застоялся/отложен", когда разрешение маршрута приостановлено во время параллельного рендеринга.

```js
const isRouting = useIsRouting();

return (
    <div classList={{ 'grey-out': isRouting() }}>
        <MyAwesomeContent />
    </div>
);
```

### `useRouteData`

Получает возвращаемое значение из функции [data function](#data-functions).

=== "/pages/users/[id].jsx"

    ```js
    import { useRouteData } from '@solidjs/router';

    const user = useRouteData();

    return <h1>{user().name}</h1>;
    ```

=== "/pages/users/[id].tsx"

    ```ts
    import { useRouteData } from '@solidjs/router';

    const user = useRouteData<RouteDataFunctionType>();

    return <h1>{user().name}</h1>;
    ```

### `useMatch`

Функция `useMatch` принимает аксессор, возвращающий путь, и создает Memo, возвращающий информацию о совпадении, если текущий путь совпадает с предоставленным путем. Полезно для определения соответствия заданного пути текущему маршруту.

```js
const match = useMatch(() => props.href);

return <div classList={{ active: Boolean(match()) }} />;
```

### `useRoutes`

Используется для определения маршрутов через объект конфигурации вместо JSX. См. раздел [Маршрутизация на основе конфигурации](#config-based-routing).

### `useBeforeLeave`

Функция `useBeforeLeave` принимает функцию, которая будет вызвана перед выходом из маршрута. Функция будет вызвана с:

-   `from` (`Location`): текущее местоположение (до изменения).
-   `to` (`string | number`): путь, переданный для навигации.
-   `options` (`NavigateOptions`): опции, передаваемые навигатору.
-   `preventDefault` (void function): вызов блокировки изменения маршрута.
-   `defaultPrevented` (readonly boolean): true, если все ранее вызванные обработчики оставления вызывали preventDefault().
-   `retry` (void function, force?: boolean ): вызов для повторного выполнения той же навигации, возможно, после согласования с пользователем. Передайте true, чтобы пропустить повторный запуск обработчиков ухода (т.е. принудительный переход без подтверждения).

Пример использования:

```js
useBeforeLeave((e: BeforeLeaveEventArgs) => {
    if (form.isDirty && !e.defaultPrevented) {
        // preventDefault to block immediately and prompt user async
        e.preventDefault();
        setTimeout(() => {
            if (
                window.confirm(
                    'Discard unsaved changes - are you sure?'
                )
            ) {
                // user wants to proceed anyway so retry with force=true
                e.retry(true);
            }
        }, 100);
    }
});
```

!!!note ""

    Более подробную информацию о Solid Router можно найти в репозитории Github [здесь](https://github.com/solidjs/solid-router).

## Ссылки

-   [Solid Router](https://docs.solidjs.com/guides/how-to-guides/routing-in-solid/solid-router)
