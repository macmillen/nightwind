# Nightwind

A plugin that gives you an out-of-the-box, customisable, overridable dark mode for tailwindcss.

Nightwind uses the Tailwind color palette and your own colors to automatically generate the dark mode version of the color classes you use for text, background, borders and placeholders.

You can check it out on https://nightwindcss.com

## Installation

```sh
npm install nightwind
```

Nightwind uses two experimental features of Tailwind: 
- 'applyComplexClasses' to generate Nightwind classes when using @apply in custom classes
- 'darkModeVariant' to override colors generated by Nightwind (or specify a style only for dark mode).

To enable Nightwind, add in your `tailwind.config.js` file:

```js
// tailwind.config.js
module.exports = {
  experimental: {
    applyComplexClasses: true,
    darkModeVariant: true
  },
  dark: 'class',

  // ...
  
  plugins: [
    require('nightwind')
  ]
}
```

## Usage

Nightwind relies on a fixed 'nightwind' class and a toggled 'dark' class in a top level element in the DOM, typically the <html> element.

You can define your own functions to enable the dark mode, or use the helper functions included in 'helper.js'.

```js
// React Example
import nightwind from 'nightwind/helper'

export default function Layout({children}) {
  
  useEffect(() => {
    nightwind.initNightwind()
  }, []);

  return (
    // ...
  )
}
```

```js
// React Example
import nightwind from 'nightwind/helper'

export default function Navbar() {
  return (
    // ...
    <button onClick={ () => nightwind.toggleNightMode() }></button>
    // ...
  )
}
```

### Some examples

- 'bg-white' in dark mode becomes 'bg-black'
- 'bg-red-100' in dark mode becomes 'bg-red-900'
- 'placeholder-gray-200' in dark mode becomes 'placeholder-gray-800'
- 'hover:text-indigo-300' in dark mode becomes 'hover:text-indigo-700'
- 'sm:border-blue-400' in dark mode becomes 'sm:border-blue-600'
- 'xl:hover:bg-purple-500' in dark mode remains 'xl:hover:bg-purple-500'

## Configuration

If you add custom colors using number notation in your tailwind.config.js file, Nightwind will automatically switch between them when switching from/to dark mode.

> Note: Colors are swapped so that their sum is 1000 (so 100-900, 200-800, etc). There is no limit to the number of color you define, but if you want Nightwind to automatically generate classes make sure to define opposite ones.

Nightwind is responsive by default, supporting both default and custom breakpoints. 

If you need dark colors for one or more variants (such as 'hover'), make sure to specify them as in the example below.

```js
// tailwind.config.js
module.exports = {
  // ...
  theme: {
    extend: {
      colors: {
        // Add your custom colors here, with number notation
        'primary': {
          100: '#caf0f8', // becomes primary-900 in dark mode
          300: '#90e0ef', // becomes primary-700 in dark mode
          500: '#00b4d8', // remains the same in dark mode
          700: '#0077b6', // becomes primary-300 in dark mode
          900: '#03045e', // becomes primary-100 in dark mode
        }
      },
      screens: {
        // Add your custom screens here
        'custom': '800px',
      },
      transitionDuration: {
        // Set the transition duration of color classes when switching to/from dark mode
        'nightwind': '300ms' // default '200ms'
      }
    },
  },
  variants: {
    // Set the variants for which you want Nightwind to automatically generate dark classes
    'nightwind': ['hover', 'focus'],
  },
  // ...
}
```

### Overrides

The 'darkModeVariant' experimental feature allows you to write classes like 'dark:bg-gray-800' (not necessarily related to color classes) that only gets applied when you switch into dark mode.

You can use the 'dark' variant to override the automatic Nightwind classes.

```html
<h2 class="text-gray-900 dark:text-yellow-200">I'm yellow in dark mode</h2>
```

In dark mode, this element would have a 'text-gray-100' color (the opposite of 'text-gray-900'), but the override makes it 'text-yellow-200'.

> Note: The 'dark' variant can also be concatenated with both screens and other variants, so you can write classes like 'sm:dark:hover:text-yellow-200'.

### Custom classes

Thanks to the 'applyComplexClasses' experimental feature, no setup is needed for custom classes. That means you can write things like

```css
.custom {
  @apply text-indigo-700 hover:text-indigo-600;
}
```

and Nightwind will still work as expected.