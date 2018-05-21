---
title: "HUGO date problems. TL;DR: switch to YAML"
author: ["myself"]
date: 2018-05-20T22:51:00-03:00
categories: ["emacs"]
draft: false
---

## Introduction {#introduction}

I was having tons of trouble exporting my blog. The date format used by ox-hugo was not recognized by the TOML parser. Who would have guessed that using other tool would do the trick.


## Procedure {#procedure}

It's as easy as adding **`#+hugo_front_matter_format: yaml`** to the beginning of the file.


## Conclusion {#conclusion}

Sometimes the small things matter. Keep this little snippet in high regard.


## Credits {#credits}

-   [ox-hugo post-yaml test site](https://ox-hugo.scripter.co/test/singles/post-yaml/)
