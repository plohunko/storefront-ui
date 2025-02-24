# Customization of Storefron UI components

We put a lot of efforts to let you customize any aspect of the UI. Let's see how to do this from top to bottom.

## Global variables

Most of the styling is made through a SCSS variables in global stylesheets. We use them to setup fonts, HTML elements styling (like h1, p), layout properties and colors. 

You can override any the global variables inside your `sfui.scss` file that should exist in a root of your app.

To override any of the global variables just create one with the same name in `sfui.scss` and your custom value.

````scss
// This will override primary accent color to 'blue'
$c-accent-primary: blue;
````

You can find all available variables and it's defaults [here](https://github.com/DivanteLtd/storefront-ui/tree/master/src/css/variables).

## Component variables

You can override component-specific SCSS variables in the exactly same way. 

````scss
// This will override primary accent color for button component to 'blue'
$c-accent-primary: blue !default;
````

Please note that you should always add a `!default` property when overriding component variables. Otherwise you will also affect scoped modifications you can make for individual components.

## Customization of individual components

If variables are not providing the level of customization you need you can also make a new component that is importing individual partials of the source component from SFUI.

Let's say we want to create `CustomizedButton` component. We should start with creating new Vue component where we can import `Button` partials.
````html 
<script>
import instance from "storefront-ui/SfButton.js";

export default {
  ...instance
};
</script>

<template lang="html" src="storefront-ui/SfButton.html"></template>

<style lang="scss" scoped>
@import "~storefront-ui/SfButton.scss";
</style>
````

Now let's see how we can customize any of it's parts by still making use of the sfui parts we want to remain untouched.

- **Template**:  Here we replaced default HTML with our own. In this example we replaced default <button> with a <div>
````html
<template>
  <div class="sf-button"><slot /></div>
</template>
````

- **Vue instance**: We can make changes directly to imported Vue instance object (like in example we change it's `name` property) and add new properties (like here we added data property `message`)
````html
<script>
import instance from "storefront-ui/SfButton.js";

instance.name = 'CustomizedComponent'

export default {
  ...instance,
  data () {
    return {
      message: "Hello World!"
    }
  }
};
</script>
````
- **Styles**: We can eithe modify SCSS variables only for this specific component or even write completely new stylesheet and get rid of the import.
````html
<style lang="scss" scoped>
$c-accent-primary: blue;
@import "~storefront-ui/SfButton.scss";
</style>
````
**Please note** that `scoped` attribute must be present on `<style>` tag if you're overriding styles. Otherwise your local changes will be overwritten by global overrides from `sfui.scss`
