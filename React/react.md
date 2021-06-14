# usestate lazy initialize function

# useffect to be called based on state change

# Create reusable custome hooks

# React hook flow

# console.dir

### Notes:

- Error boundaries have to be class components

- A class component becomes an error boundary if it defines either (or both) of the lifecycle methods static getDerivedStateFromError() or componentDidCatch(). Use static getDerivedStateFromError() to render a fallback UI after an error has been thrown. Use componentDidCatch() to log error information.

- In practice, most of the time you’ll want to declare an error boundary component once and use it throughout your application.

- An error boundary can’t catch an error within itself. If an error boundary fails trying to render the error message, the error will propagate to the closest error boundary above it. This, too, is similar to how catch {} block works in JavaScript.

### Debouncing pattern for React events

#### typical implementation:

- We will wire an onChange handler to the input text box. As the user types, it will trigger the handler repeatedly with the contents of the search box.
- The handler will call an external API asynchronously with the text as parameter to fetch the results. If the input is `walmart` , the network calls that would be fired in the following sequence. Remember that the results might not be fetched in same order. Network calls are unpredictable, **so it is possible that the second call resolves later than the last one and now you have stale data to process.**

```
https://ticker-2e1ica8b9.now.sh/keyword/w
https://ticker-2e1ica8b9.now.sh/keyword/wa
https://ticker-2e1ica8b9.now.sh/keyword/wal
https://ticker-2e1ica8b9.now.sh/keyword/walm
...
```

```
Debouncing enforces that a function not be called again until a certain amount of time has passed without it being called. As in “execute this function only if 100 milliseconds have passed without it being called”.
```

refer this to understand the difference between debouncing and throttling
http://demo.nimius.net/debounce_throttle/

#### React synthetic events
Your event handlers will be passed instances of `SyntheticEvent`, a cross-browser wrapper around the browser’s native event. It has the same interface as the browser’s native event, including `stopPropagation()` and `preventDefault()`, except the events work identically across all browsers.
