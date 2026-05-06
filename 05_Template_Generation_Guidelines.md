# Advanced Generation Guidelines for Blogger Templates

When generating or modifying a Blogger template, the following rules **MUST** be strictly enforced to ensure the XML compiles correctly and functions within the Blogger ecosystem.

## 1. Core Development Directives
Always ask the user for their styling preference (Pure CSS vs Tailwind) before generating. If no preference is provided, default to Pure CSS. Follow these core rules before writing any XML code:

- **Rule 1: Strict XML Compliance.** Blogger uses XML, not HTML5. Every tag must be closed (e.g., `<img/>`, `<br/>`, `<hr/>`). Attributes must have quotes. Do not use boolean attributes without values (e.g., use `locked='yes'`, not just `locked`).
- **Rule 2: Proper Escaping.** You CANNOT use raw ampersands `&` in URLs or content. They must be `&amp;`. E.g., `https://fonts.com?family=Inter&display=swap` is **fatal**. It must be `&amp;display=swap`.
- **Rule 3: CDATA for Scripts/Styles.** Any `<script>` or `<style>` tag that contains `<`, `>`, `&`, or `{}` must be wrapped in `//<![CDATA[` and `//]]>`.
- **Rule 4: Prefix Dynamic Attributes.** To use Blogger variables inside standard HTML attributes, prefix with `expr:`. E.g., `<a expr:href='data:post.url'>`.
- **Rule 5: Styling Architecture.** Respect the chosen styling method. If Pure CSS (default), write all styling inside `<b:skin>`. If Tailwind, use utility classes and avoid large CSS blocks. Always ensure `b:css='false'` is on the `<html>` tag regardless of the method.

## 2. Standard Structure Template
When scaffolding a new layout, enforce this exact hierarchy:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE html>
<html b:css='false' b:defaultwidgetversion='2' b:layoutsVersion='3' xmlns:b='http://www.google.com/2005/gml/b' xmlns:data='http://www.google.com/2005/gml/data' xmlns:expr='http://www.google.com/2005/gml/expr'>
<head>
    <!-- 1. Meta & Title -->
    <!-- 2. Optional: Tailwind CDN & CDATA Config (If Requested) -->
    <!-- 3. Mandatory <b:skin><![CDATA[ ... ]]></b:skin> -->
</head>
<body class="...">
    <b:section id='header' showaddelement='no'>
        <b:widget id='Header1' locked='yes' type='Header'>
            <b:includable id='main'>...</b:includable>
        </b:widget>
    </b:section>
    
    <b:section id='main' showaddelement='yes'>
        <b:widget id='Blog1' locked='yes' type='Blog'>
            <b:includable id='main'>
                 <b:if cond='data:view.isHomepage'>
                     <!-- Homepage layout -->
                 <b:else/>
                     <!-- Single Post layout -->
                 </b:if>
            </b:includable>
        </b:widget>
    </b:section>
</body>
</html>
```

## 3. Handling API/Fetch in Blogger
Often, templates need to load specific categories dynamically (like a "Projects" section on a homepage). Use Blogger's native JSON feed.

- **Endpoint:** `/feeds/posts/default/-/CategoryName?alt=json`
- **Rule:** The script doing the fetch MUST be wrapped in CDATA.
- **Rule:** When rendering HTML via Javascript, construct the DOM carefully or use template literals, as the XML parser ignores JS logic inside CDATA.

## 4. Developer Checklist
Before copying code into Blogger, check:
[ ] Did you use `<br>` instead of `<br/>`?
[ ] Did you forget `expr:` on `href` or `src` containing `data:`?
[ ] Are multiple classes separated properly without breaking quotes?
[ ] Are Tailwind classes containing square brackets `[]` causing XML issues? (They usually don't, but quotes must be strictly matched).
[ ] Is the `<b:skin>` tag present, even if empty? (It is required to save).
