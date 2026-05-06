# Styling Architecture: Pure CSS vs Tailwind

When building Blogger templates, there are two primary approaches to styling. **Pure CSS is the default and recommended method** due to its deep integration with Blogger's native tools, while **Tailwind CSS** serves as a modern alternative.

## 1. DEFAULT APPROACH: Pure CSS

By default, templates should rely on custom CSS written inside the mandatory `<b:skin>` tag. This method provides the best compatibility with the Blogger ecosystem.

### Why Pure CSS is the Default:
It allows you to leverage Blogger's built-in **Theme Designer**. By defining `<Variable>` tags, end-users can change the blog's primary colors, fonts, and backgrounds from the dashboard without needing to edit code.

### Structure for Pure CSS:
```xml
<b:skin><![CDATA[
/*
<Group description="Theme Colors" selector="body">
  <Variable name="body.background" description="Background Color" type="color" default="#f9f9f9" value="#f9f9f9"/>
  <Variable name="theme.primary" description="Primary Color" type="color" default="#b9000c" value="#b9000c"/>
</Group>
*/

/* --- RESET & BASIC STYLES --- */
body {
    background-color: $(body.background);
    margin: 0;
    padding: 0;
    font-family: sans-serif;
}

.header {
    background-color: $(theme.primary);
    color: white;
}
]]></b:skin>
```

**Development Rules for Pure CSS:**
- Avoid using inline styles in the HTML body.
- Define all classes properly within the `<b:skin>` section.
- Utilize the `$(variable.name)` syntax for critical colors and fonts so users can modify them.

---

## 2. ALTERNATIVE APPROACH: Tailwind JIT (Zero-CSS)

If the project specifically requires rapid utility-first styling or advanced modular grids, you can opt for the Tailwind CDN. 

### Setting Up Tailwind (When Requested):
Inject the Tailwind CDN script into the `<head>` of your template and use a CDATA block to define your custom design tokens.

```xml
<!-- Include Tailwind CDN -->
<script src='https://cdn.tailwindcss.com?plugins=forms,container-queries'/>

<!-- Configuration Block -->
<script id='tailwind-config'>
//<![CDATA[
  window.tailwind.config = {
    darkMode: "class",
    theme: {
      extend: {
        colors: {
          "primary": "#b9000c",
        }
      },
    },
  }
//]]>
</script>
```

**Development Rules for Tailwind:**
- Do not write massive blocks of CSS in `<b:skin>`. Keep the `<b:skin>` tag almost empty (only basic resets).
- Add classes directly to XML layout tags, being extremely careful not to break XML quotes when using arbitrary values like `w-[500px]`.
