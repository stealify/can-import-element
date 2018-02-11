# can-import-element
A WebComponent Version of can-view-import supporting styles default behavior like can-view-import


## Idea
HTML now supports
<link rel="import"> that returns a promise that should get integrated so that
<can-import from="*.html">Works.</can-import>
https://github.com/canjs/can-util/blob/master/js/import/import.js

## The questions here 
what should a html import return as that will lead to a lot of edge cases but i think this will get one of the main default loading algos on the web its the nativ replacement for <can-import> and gets used in combination with a script that extracts parts of the result or alternativ the element registers it self 

if we would for example have a can-component.html that executes a javascript that registers a can-component this would be enough to render component tag

## Feature Detection
```js
function supportsImports() {
  return 'import' in document.createElement('link');
}

if (supportsImports()) {
  // Good to go!
} else {
  // Use other libraries/require systems to load files. StealJS
}
```


HTML
```html
<script>
  function handleLoad(e) {
    console.log('Loaded import: ' + e.target.href);
  }
  function handleError(e) {
    console.log('Error loading import: ' + e.target.href);
  }
</script>

<link rel="import" href="file.html"
      onload="handleLoad(event)" onerror="handleError(event)">

<script>
// Usage
var content = document.querySelector('link[rel="import"]').import; // => document.fragment
</script>
```


or

```js
var link = document.createElement('link');
link.rel = 'import';
link.href = 'file.html';
//link.setAttribute('async', ''); // make it async!
link.onload = function(e) {...};
link.onerror = function(e) {...};
document.head.appendChild(link);

// Usage
var content = document.querySelector(link).import; // => document.fragment
```

