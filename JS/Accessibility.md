Navigation

- correct contrast
- dont use all uppercase, (reader reads us as US)
- use nav nav-> ul-> li
- if have multiple navigation then give aria-label
- letters should not be too close
- for active navigation better way is aria-current= "Page"
  and use the style like below
  nav ul li a[aria-current="page"] {
  font-weight: bold;
  // .. rest style
  }

- Don't use title for anchors coz they are inaccessible they don't zoom and screen readers call the title twice if you have the same text
- Give outlines when focused/hover
  outline: 4px solid green

- outline dosen't move the content around, whereas border does

### Hamburger navigation

- Use aria-expanded as true/false
- add aria-label
- Always use title case or sentence case
