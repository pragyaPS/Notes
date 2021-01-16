A selector does what it says on the tin, it selects some part of your document in order that you can apply CSS rules to it.

Some selectors act as if you had applied a class to something in the document. For example p:first-child behaves as if you added a class to the first p element, these are known as **pseudo-class** selectors. 

The **pseudo-element** selectors act as if an element was dynamically inserted, for example ::first-line acts in a similar way to you wrapping a span around the first line of text. However it will reapply if the length of that line changes, which wouldn’t be the case if you inserted the element.

If you are struggling with getting some CSS to apply to an element then your browser DevTools are the best place to start, take a look at the example below in which I have an h1 element targeted by the element selector h1 and making the heading orange. I am also using a class, which sets the h1 to rebeccapurple. **The class is more specific and so the h1 is purple.**


### BEING IN OR OUT OF FLOW

Elements in CSS are described as being ‘in flow’ or ‘out of flow.’ Elements in flow are given space and that space is respected by the other elements in flow. If you take an element out of flow, by floating or positioning it, then the space for that element will no longer be respected by other in flow items.

This is most noticeable with absolutely positioned items. If you give an item `position: absolute` it is removed from flow, then you will need to ensure that you do not have a situation in which the out of flow element overlaps and makes unreadable some other part of your layout.

However, floated items also are removed from flow, and while following content will wrap around the shortened line boxes of a floated element, you can see by placing a background color on the box of the following elements that they have risen up and are ignoring the space used by the floated item.

