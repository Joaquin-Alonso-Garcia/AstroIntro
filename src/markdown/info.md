## What you need to run Astro on your system?
Node.js v18.14.1+
You can use something like nvm to work with different node versions, depending on your current setup, and change versions with ease.

Astro routes are created under the pages folder and each route should end in .astro . This is a file-based routing, which means each file in your src/pages/ directory becomes an endpoint on your site.

Unlike many frameworks, Astro uses standard HTML <a> elements to navigate between pages (also called routes), with traditional page refreshes.

The information at the top of the file, inside the code fences, is called frontmatter. This data—including tags and a post image—is information about your post that Astro can use. It does not appear on the page automatically, but you will access it later in the tutorial to enhance your site.

Here you can see a markdown cheat sheet to help you write it: [markdown cheat sheet](https://www.markdownguide.org/cheat-sheet/)

Astro has special HTML attributes that you can use to specify an element or component behavior, here are some common directives:
**Common Directives**

- class:list
  ``class:list={...}`` takes an array of class values and converts them into a class string.
  This can be used to set some dynamic classes when needed into an element or a component.
  Ex: ``const { isRed } = Astro.props;``
  If `isRed` is truthy, class will be "box red".
  If `isRed` is falsy, class will be "box".
  ``<div class:list={['box', { red: isRed }]}><slot /></div>``

- set:htm
  ``set:html={string}`` injects an HTML string into an element, similar to setting el.innerHTML. Commonly used to inject promises or content that comes from an external api.

- set:text
  ``set:text={string}`` injects a text string into an element, just that.

**Client Directives**

These directives control how UI Framework components are hydrated on the page.

Astro Hydration works with client directives: You can use interactive components with different frameworks inside Astro, by default Astro render all your components on the server to generate static HTML without javascript unless you specify it with the client directive *client: only*, which will render your component on the client side along with its own javascript. You can also specify the way a component should hydrate with one of the client directives available:
- client:load
  Priority: High
  Useful for: Immediately-visible UI elements that need to be interactive as soon as possible.

  Load and hydrate the component JavaScript immediately on page load.

- client:idle
  Priority: Medium
  Useful for: Lower-priority UI elements that don’t need to be immediately interactive.

  Load and hydrate the component JavaScript once the page is done with its initial load and the requestIdleCallback event has fired. If you are in a browser that doesn’t support requestIdleCallback, then the document load event is used.

- client:visible
  Priority: Low
  Useful for: Low-priority UI elements that are either far down the page (“below the fold”) or so resource-intensive to load that you would prefer not to load them at all if the user never saw the element.

  Load and hydrate the component JavaScript once the component has entered the user’s viewport. This uses an IntersectionObserver internally to keep track of visibility.

- client:visible={{rootMargin}}

  Optionally, a value for rootMargin can be passed to the underlying IntersectionObserver. When rootMargin is specified, the component JavaScript will hydrate when a specified margin (in pixels) around the component enters the viewport, rather than the component itself.
  Specifying a rootMargin value can reduce layout shifts (CLS), allow more time for a component to hydrate on slower internet connections, and make components interactive sooner, enhancing the stability and responsiveness of the page.

- client:media
  Priority: Low
  Useful for: Sidebar toggles, or other elements that might only be visible on certain screen sizes.

  ``client:media={string}`` loads and hydrates the component JavaScript once a certain CSS media query is met.

- client:only
  ``client:only={string}`` skips HTML server-rendering, and renders only on the client. It acts similar to client:load in that it loads, renders and hydrates the component immediately on page load.

  You must pass the component’s correct framework as a value! Because Astro doesn’t run the component during your build / on the server, Astro doesn’t know what framework your component uses unless you tell it explicitly.

  ``<SomeReactComponent client:only="react" />``
  ``<SomePreactComponent client:only="preact" />``
  ``<SomeSvelteComponent client:only="svelte" />``
  ``<SomeVueComponent client:only="vue" />``
  ``<SomeSolidComponent client:only="solid-js" />``
  ``<SomeLitComponent client:only="lit" />``

**Script & Style Directives**

Astro supports Sass and Less by default.
Scoped styles don't leak to other components so is okay to use low specificity selectors. Scoped styles also takes precedence over global styles, so they will overwrite your global styles if the element has applied any global styles.
Astro also comes with PostCSS that is include as part of Vite so you can configure it via a postcss.config.cjs file in the project root.

These directives can only be used on HTML ``<script>`` and ``<style>`` tags, to control how your client-side JavaScript and CSS are handled on the page.

- is:global
  By default all styles that you include in your components are scoped for that component only, but you can use the ``<style is:global>`` directive to apply on all the site.

- is:inline
  This directive tells Astro to leave the ``<script>`` and ``<style>`` tags as-is, they won't be processed, optimized or bundled.

- define:vars
  ``define:vars={...}`` can pass server-side variables form your component frontmatter into the client ``<script>`` and ``<style>`` tags.
  Ex: {
    <style define:vars={{ textColor: foregroundColor, backgroundColor }}>
      h1 {
        background-color: var(--backgroundColor);
        color: var(--textColor);
      }
    </style>

    <script define:vars={{ message }}>
      alert(message);
    </script>
  }

- is:raw
  ``is:raw`` is an advanced directive that instructs the Astro compiler to treat any children of that element as text. This means that all special Astro templating syntax will be ignored inside of this component.