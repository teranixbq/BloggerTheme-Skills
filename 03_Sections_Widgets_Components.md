# Sections, Widgets, and Components

Blogger constructs layouts through a hierarchy of `Sections` containing `Widgets`, which in turn contain `Includables` (components).

## 1. Sections `<b:section>`
A section is an area on the page where users can add, move, or arrange widgets from the Blogger Dashboard Layout tab.

### Key Rules for Sections:
1. **Required ID:** Every `<b:section>` MUST have a unique `id` (only letters and numbers, no spaces).
2. **Standard IDs:** For template portability (so users don't lose their widgets when changing themes), use these standard IDs: `id='header'`, `id='main'`, `id='sidebar'`, `id='footer'`.
3. **Show Add Element:** The `showaddelement='yes'` attribute enables the "Add a Gadget" button in the Dashboard for that section. Use `showaddelement='no'` for static/fixed areas.

```xml
<b:section class='header-area' id='header' showaddelement='no'>
    <!-- Widgets go here -->
</b:section>
```

## 2. Widgets `<b:widget>`
Widgets (or Gadgets) are functional blocks that provide content (e.g., Blog Posts, HTML/JS, Links).

### Key Rules for Widgets:
1. **Unique ID:** Every widget must have a globally unique ID (e.g., `Header1`, `Blog1`, `HTML100`). **WARNING:** Changing the ID of an existing widget will delete it and create a new one, causing data loss!
2. **Types:** `type='Header'`, `type='Blog'`, `type='HTML'`, `type='Text'`, `type='LinkList'`.
3. **Locked:** `locked='yes'` prevents the user from accidentally deleting the widget or moving it out of the section. Use this for critical structural widgets like the Header or main Blog feed.

## 3. Includables and Main Entry `<b:includable>`
Every widget MUST contain an `<b:includable id='main'>`. This is the entry point (the "render function") for that widget.

You can create modular components by defining custom includables and calling them using `<b:include>`.

```xml
<b:widget id='Blog1' locked='yes' title='Blog Posts' type='Blog'>
    <!-- Main execution point -->
    <b:includable id='main'>
        <!-- Call a custom includable -->
        <b:include name='post-loop'/>
    </b:includable>

    <!-- Custom Includable -->
    <b:includable id='post-loop'>
        <div class="grid grid-cols-1 md:grid-cols-2">
            <b:loop values='data:posts' var='post'>
                <article>
                    <h2><data:post.title/></h2>
                </article>
            </b:loop>
        </div>
    </b:includable>
</b:widget>
```

## 4. Building with Tailwind
You can seamlessly use Tailwind utility classes directly in the standard HTML tags nested inside the `<b:includable>` logic.

```xml
<b:widget id='Text1' locked='yes' title='Hero Title' type='Text'>
    <b:includable id='main'>
        <h1 class='text-5xl font-black uppercase tracking-tighter text-gray-900 dark:text-white'>
            <data:content/>
        </h1>
    </b:includable>
</b:widget>
```
