<Title>on:* and oncapture:*</Title>

For events with capital letters, or if you need to attach event handlers directly to a DOM element instead of optimized delegating via the document, use `on:*` in place of `on*`.

```tsx
<div on:DOMContentLoaded={(e) => console.log('Welcome!')} />
```

This directly attaches an event handler (via [`addEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)) to the `div`.

Another special attribute is `oncapture:*`, which attaches the event handler with the option [`capture: true`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#capture), meaning that the event will get captured on the way down the DOM tree, instead of waiting for it to bubble up from its descendants. This results in calling this event handler before descendant handlers (in particular, before those descendants could potentially cancel bubbling via [`stopPropagation`](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation)), and enables the handler to stop the event from descending via `stopPropagation`.
