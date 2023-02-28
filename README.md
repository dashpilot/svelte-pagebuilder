# Svelte Pagebuilder

The editing has been designed to be completely separate from from the SPA on the page, and should work with any modern javascript framework (Svelte/Vue/React/Alpinejs, etc.). The communication between the editor and the SPA happens in two ways:

1. SPA to Editor (on page load)

On page load, the Editor loads the component configuration from `window.components`.

2. Editor to SPA (whenever the data changes)

Whenever the data changes, the editor calls the `window.relay(data)` function. You spa can hook into this function to update the page data.
