# usestate lazy initialize function
# useffect to be called based on state change
# Create reusable custome hooks 
# React hook flow
# console.dir


### Notes:
* Error boundaries have to be class components

* A class component becomes an error boundary if it defines either (or both) of the lifecycle methods static getDerivedStateFromError() or componentDidCatch(). Use static getDerivedStateFromError() to render a fallback UI after an error has been thrown. Use componentDidCatch() to log error information.

* In practice, most of the time you’ll want to declare an error boundary component once and use it throughout your application.

* An error boundary can’t catch an error within itself. If an error boundary fails trying to render the error message, the error will propagate to the closest error boundary above it. This, too, is similar to how catch {} block works in JavaScript.




 
