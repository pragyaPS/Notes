Available on chrome, Firefox, chromium(Edge)
## Performance improvements


### Component Filters
- you can hide non relevent components
- Filter preferences are remembered between sessions

### Inline Props are no longer shownAll the props, state, and hooks wiil be displayed on the right hand side of the console

```
import XYZ from 'xyz';

class Main extends Component {
  constructor(props) {
    super(props);

    this.state = { name : "John" }
 }
 render() {
    const { name } = this.state;
      return (
        <ABC>
          <XYZ name={name} />
        </ABC>
      )
  }
}
```

Here `Main` is the owner component. and only owners can send down props.
We can quickly debug an unexpected prop value by skipping over the parents. DevTools V4 has a new `rendered by` list in 
the right hand pane that allow quickly step through the list of owners to speed up the debugging.


### Inline Props are no longer shown

## Visual improvements

###Indented component View


In the previous versions, deeply nested  components require both vertical and horizontal scrolling to see, Which makes tracking large component 
trees difficult.

Devtools now Dynamically adjust nesting indentation to eliminate horizontal scrolling

###Improved searching
In previous versions, when searching in devtools the result is often a filtered components tree showing matching nodes as roots i.e 
other components are hidden and the search match now display as root elements. This made the overall structure of the application harder to reason about 
because it displayed ancestors as siblings.

Now we can easily search through components with results shown inline similar to the browser's find in page search
!(../images/improvedsearching.gif)

## Functional improvements










