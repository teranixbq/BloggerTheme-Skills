# Data Tags and Conditional Logic

Blogger provides powerful control structures natively inside its XML using the `b:` and `data:` namespaces. This allows templates to function dynamically without excessive JavaScript.

## 1. Conditional Logic `<b:if>`, `<b:elseif>`, `<b:else/>`

You can conditionally render HTML based on the current page type, URL, or data state.

```xml
<!-- Example: Showing different layouts based on the view type -->
<b:if cond='data:view.isHomepage'>
    <!-- Shown ONLY on the home page (e.g., Hero section, Grid of posts) -->
    <h1 class='text-4xl'>Welcome Home</h1>
<b:elseif cond='data:view.isPost'/>
    <!-- Shown ONLY on a single article/post page -->
    <h1 class='text-2xl'><data:post.title/></h1>
<b:elseif cond='data:view.isLabelSearch'/>
    <!-- Shown when user clicks a specific category/tag -->
    <h2>Viewing Category: <data:view.search.label/></h2>
<b:else/>
    <!-- Fallback for other pages (archives, 404, etc) -->
    <p>General View</p>
</b:if>
```

**Common View Conditions:**
- `data:view.isHomepage`: True on index page.
- `data:view.isPost`: True on an individual blog post.
- `data:view.isPage`: True on a static page.
- `data:view.isMultipleItems`: True on index, search, label, and archive pages.
- `data:view.isSingleItem`: True on post and static pages.

## 2. Looping Arrays `<b:loop>`

Used to iterate over items like posts, comments, or labels.

```xml
<b:loop values='data:posts' var='post'>
    <article class='mb-8 border-b-2 border-gray-200 pb-4'>
        <!-- Accessing loop variable -->
        <h2 class='text-xl font-bold'><data:post.title/></h2>
        <p><data:post.snippet/></p>
        
        <!-- Nested loop for labels -->
        <div class='flex gap-2 mt-2'>
            <b:loop values='data:post.labels' var='label'>
                <span class='bg-gray-100 px-2 py-1 text-xs'><data:label.name/></span>
            </b:loop>
        </div>
    </article>
</b:loop>
```

## 3. Using Data in HTML Attributes using `expr:`

If you need to use a Blogger variable inside a standard HTML attribute (like `src`, `href`, `class`), you MUST prefix the attribute with `expr:`.

```xml
<!-- WRONG: <a href="data:post.url">...</a> -->

<!-- CORRECT: -->
<a expr:href='data:post.url' class='text-blue-500 hover:underline'>
    Read More
</a>

<!-- Example with images -->
<img expr:alt='data:post.title' expr:src='data:post.featuredImage' class='w-full object-cover'/>
```

## 4. Useful Data Tags
- `<data:blog.title/>`: The name of the blog.
- `<data:blog.pageTitle/>`: SEO friendly title for the current page.
- `<data:blog.homepageUrl/>`: URL of the home page.
- `<data:post.date/>`: Publish date of a post.
- `<data:post.author.name/>`: Name of the post author.
- `<data:post.body/>`: The full HTML content of a post.

## 5. Built-in Evaluation `<b:eval>`
Used to evaluate mathematical expressions or functions, most commonly for resizing images securely on Blogger's CDN.

```xml
<!-- Automatically resizes the featured image to 600px width, 1:1 aspect ratio -->
<img expr:src='resizeImage(data:post.featuredImage, 600, "1:1")' />
```
