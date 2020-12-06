### web workers
A javascript file that works on a seperate thread(access seperate web program) than the webpage thread.
one use case if I have a heavy lifting work to do and we want webpage to be non responsive. run the program offline.
All web browsers guarentee that if we spin up a different thread than a main page
if we spin up multiple web worker, in general they have seperate thread but there is no actual guarntee that every siggle thread we make has a seperate thread, coz if that is guaranteed it would be easy for denial of service of somebody's browsers
- Dedicated web worker: one webworker dedicated to the page and if there are multiple pages open they all will have dedicated web worker which gets killed on closing the tab.
- Shared web worker: another way but not all browser or mobile device handle this properly if you d multiple tabs open for shopping site and we can communicate using shared worker running once. It lives as long as there are one open tab(out of 5 tabs 4 closed still web worker running and we open new tab then it will be able to communicate with the new tab)
There are other ways to achieve this using worker.postMessage
one thing theat is different/unique in service worker is that it survive and run in the background beyond tab is going away or closed.


