+++
title = "HUGO date problems. TL;DR: update your HUGO binary"
author = ["myself"]
date = 2018-05-23T20:53:00-03:00
categories = ["emacs"]
draft = false
+++

## Introduction {#introduction}

I was having tons of trouble exporting my blog. The date format used by ox-hugo was not recognized by the TOML parser. Who would have guessed that using ~~other tool~~ **an up do date HUGO binary** would do the trick.


## Procedure {#procedure}

Fedora -- which is my distro at the time of writing this post -- makes available through the stable repo an older HUGO binary (v37) which apparently carries along [this issue](https://github.com/gohugoio/hugo/issues/2100). Go ahead and grab the proper executable from [the HUGO releases page](https://github.com/gohugoio/hugo/releases) and place it within your path after removing the one you got through dnf/yum.
Previously I could avoid the TOML date parsing issue by choosing YAML explicitly by placing **`#+hugo_front_matter_format: yaml`** at the beginning of the file.


## Conclusion {#conclusion}

Keep your stuff up to date specially if somebody had the same problem a long time ago. It may have already been properly addressed.


## Credits {#credits}

A **HUGE** shoutout to [kaushalmodi](https://github.com/kaushalmodi) for taking the time to assist me on this matter by [reaching out to me](https://github.com/leetdatageek/leetdatageek/issues/1). I really appreciate it!

-   [ox-hugo post-yaml test site](https://ox-hugo.scripter.co/test/singles/post-yaml/)
