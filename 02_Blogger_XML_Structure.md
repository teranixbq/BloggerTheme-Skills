# Blogger XML Structure

A Blogger template is not standard HTML; it is a strict XML document processed by Google's backend engine before rendering as HTML to the client.

## 1. Required Envelope and Namespaces
Every Blogger template must start with the XML declaration and the `<html>` tag containing specific Google namespaces.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE html>
<html b:css='false' b:defaultwidgetversion='2' b:layoutsVersion='3' 
      xmlns:b='http://www.google.com/2005/gml/b' 
      xmlns:data='http://www.google.com/2005/gml/data' 
      xmlns:expr='http://www.google.com/2005/gml/expr'>
```

### Attributes Explained:
- `b:layoutsVersion='3'` and `b:defaultwidgetversion='2'`: Enables modern, responsive widgets and layout engines.
- `xmlns:b`: Enables layout structure (`<b:section>`, `<b:widget>`) and logic (`<b:if>`, `<b:loop>`).
- `xmlns:data`: Access to the data dictionary (e.g., `<data:blog.title/>`).
- `xmlns:expr`: Evaluates data variables within HTML attributes (e.g., `expr:href='data:post.url'`).
- `b:css='false'`: Prevents Blogger from injecting default, heavy CSS files.

## 2. `<head>` Requirements
Inside the `<head>`, you should include meta tags and the mandatory `<b:skin>` tag.

```xml
<head>
    <meta charset='UTF-8'/>
    <meta content='width=device-width, initial-scale=1' name='viewport'/>
    <title><data:blog.pageTitle/></title>
    
    <!-- Required by Blogger, even if empty -->
    <b:skin><![CDATA[
        /* Minimal CSS fallback / overrides go here */
        body { margin: 0; padding: 0; }
    ]]></b:skin>
</head>
```
*Note: The `<b:skin>` tag is required to save the template, and its contents must be wrapped in `<![CDATA[ ... ]]>`.*

## 3. Strict XML Rules
Since Blogger parses the template as XML:
1. **Closing Tags:** Every tag must be properly closed. `<br>` becomes `<br/>`, `<img src="...">` becomes `<img src="..."/>`.
2. **Attribute Quotes:** Attribute values must be enclosed in quotes (single or double).
3. **Escaping Entities:** Ampersands in URLs must be escaped as `&amp;`. E.g., `https://fonts.googleapis.com/css2?family=Inter&display=swap` will break the template. It must be `&amp;display=swap`.

## 4. `<body>` Structure
The body usually contains a global wrapper and structural tags to divide the page into logical regions (Header, Main, Sidebar, Footer).

```xml
<body class="bg-gray-100 text-gray-900">
    <div class="wrapper">
        <header>
            <!-- header sections -->
        </header>
        <main>
            <!-- main content sections -->
        </main>
        <footer>
            <!-- footer sections -->
        </footer>
    </div>
</body>
</html>
```
