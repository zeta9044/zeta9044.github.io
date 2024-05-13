---
layout: post
title: "Sticky Header Code"
date: 2024-01-18
tags: [Header, Code, Sticky]
categories: blog
---

## 사용자 입력:
> Show me a code snippet of a website's sticky header in CSS and JavaScript.

## ChatGPT 응답:
> Certainly! Here's a simple code snippet for creating a sticky header using HTML, CSS, and JavaScript:

HTML:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Sticky Header Example</title>
</head>
<body>
    <header id="sticky-header">
        <!-- Your header content goes here -->
        <h1>Sticky Header Example</h1>
    </header>

    <!-- Rest of your website content -->

    <script src="script.js"></script>
</body>
</html>
```

CSS (styles.css):
```css
body {
    margin: 0;
    font-family: 'Arial', sans-serif;
}

#sticky-header {
    background-color: #333;
    color: #fff;
    padding: 15px;
    text-align: center;
    position: fixed;
    width: 100%;
    top: 0;
    z-index: 1000;
}

/* Adjust the styling for the rest of your content as needed */
```

JavaScript (script.js):
```javascript
window.onscroll = function() {
    stickyHeader();
};

const header = document.getElementById("sticky-header");
const sticky = header.offsetTop;

function stickyHeader() {
    if (window.pageYOffset > sticky) {
        header.classList.add("sticky");
    } else {
        header.classList.remove("sticky");
    }
}
```

This code creates a sticky header that becomes fixed at the top of the page when scrolling down. Customize the styles in the CSS section according to your design preferences.

