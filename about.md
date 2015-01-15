---
layout: page
title: About
permalink: /about/
---

This is the base Jekyll theme. You can find out more info about customizing your Jekyll theme, as well as basic Jekyll usage documentation at [jekyllrb.com](http://jekyllrb.com/)

You can find the source code for the Jekyll new theme at: [github.com/jglovier/jekyll-new](https://github.com/jglovier/jekyll-new)



``` python
def hello():
    print "howdy howdy howdy"

```


```
section	.text
    global _start   ;must be declared for linker (ld)
    _start:	            ;tells linker entry point
    mov	edx,len     ;message length
    mov	ecx,msg     ;message to write
    mov	ebx,1       ;file descriptor (stdout)
    mov	eax,4       ;system call number (sys_write)
    int	0x80        ;call kernel

    mov	eax,1       ;system call number (sys_exit)
    int	0x80        ;call kernel

```


You can find the source code for Jekyll at [github.com/jekyll/jekyll](https://github.com/jekyll/jekyll)

Here is an image:

![Another Picture of the Internals]({{ "/images/IMG_0013.JPG" | prepend: site.baseurl }})
