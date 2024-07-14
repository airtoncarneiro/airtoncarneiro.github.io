---
title: Testing advanced markdown
published: false
description: teste
tags: sql
id: 1922342
---

# Testing advanced markdown

<details>

<summary>Tips for collapsed sections</summary>

### You can add a header

You can add text within a collapsed section. 

You can add an image or a code block, too.

```ruby
   puts "Hello World"
```

</details>

<br>
code:
```python
def teste():
    print(1)
```

<br>
create diagram:
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```

<br>
autolink:
visit: https://github.com

<br>
calls outs:
> [!NOTE]  
> Highlights information that users should take into account, even when skimming.

> [!TIP]
> Optional information to help a user be more successful.

> [!IMPORTANT]  
> Crucial information necessary for users to succeed.

> [!WARNING]  
> Critical content demanding immediate user attention due to potential risks.

> [!CAUTION]
> Negative potential consequences of an action.

<br>
additional link:
[Website](http://www.serverless.com) • [Email Updates](http://eepurl.com/b8dv4P) • [Gitter](https://gitter.im/serverless/serverless) • [Forum](http://forum.serverless.com) • [Meetups](https://github.com/serverless-meetups/main) • [Twitter](https://twitter.com/goserverless) • [Facebook](https://www.facebook.com/serverless) • [Contact Us](mailto:hello@serverless.com)

<br>
Badges:
<div align="center">

[![CI](https://github.com/fastify/fastify/workflows/ci/badge.svg)](https://github.com/fastify/fastify/actions/workflows/ci.yml)
[![Package Manager CI](https://github.com/fastify/fastify/workflows/package-manager-ci/badge.svg)](https://github.com/fastify/fastify/actions/workflows/package-manager-ci.yml)
[![Web SIte](https://github.com/fastify/fastify/workflows/website/badge.svg)](https://github.com/fastify/fastify/actions/workflows/website.yml)
[![Known Vulnerabilities](https://snyk.io/test/github/fastify/fastify/badge.svg)](https://snyk.io/test/github/fastify/fastify)
[![Coverage Status](https://coveralls.io/repos/github/fastify/fastify/badge.svg?branch=main)](https://coveralls.io/github/fastify/fastify?branch=main)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](https://standardjs.com/)

</div>

<br>
Nice looking file tree:
# Code & components for pages
./src/* 
  ├─ src/assets - # Minified images, fonts, icon files
  ├─ src/components - # Individual smaller components
  ├─ src/fragments - # Larger chunks of a page composed of multiple components
  ├─ src/layouts - # Page layouts used for different types of pages composed of components and fragments
  ├─ src/page - # Custom pages or pages composed of layouts with hardcoded data components, fragments, & layouts
  ├─ src/pages/* - # Next.js file based routing
  │  ├─ _app.js - # next.js app entry point
  │  ├─ _document.js - # next.js document wrapper
  │  ├─ global.css - #  Global CSS styles
  │  └─ Everything else... - # File based routing
  └─ src/utils - # Utility functions used in various places

<br>
Using CSS in Readme:
<div align="center">
  <br/>
  <a href="https://github.com/DavidWells/advanced-markdown/blame/master/svg-with-css.svg">
    <img alt="Click to see the source" height="400" src="svg-with-css.svg" width="800" />
  </a>
  <br/>
</div>

<br>
Highlight:
I need to highlight these ==very important words==
