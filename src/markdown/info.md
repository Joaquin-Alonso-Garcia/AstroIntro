## What you need to run Astro on your system?
Node.js v18.14.1+
You can use something like nvm to work with different node versions, depending on your current setup, and change versions with ease.

Astro routes are created under the pages folder and each route should end in .astro . This is a file-based routing, which means each file in your src/pages/ directory becomes an endpoint on your site.

Unlike many frameworks, Astro uses standard HTML <a> elements to navigate between pages (also called routes), with traditional page refreshes.

The information at the top of the file, inside the code fences, is called frontmatter. This data—including tags and a post image—is information about your post that Astro can use. It does not appear on the page automatically, but you will access it later in the tutorial to enhance your site.

Here you can see a markdown cheat sheet to help you write it: [markdown cheat sheet](https://www.markdownguide.org/cheat-sheet/)