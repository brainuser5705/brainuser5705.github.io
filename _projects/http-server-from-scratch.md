---
name: http server from scratch
description: logging my process
time: 2024-03
tags: c
---

# Log

Each log links to a page for each "subproject" building up to the end. They include links to documentation and general things learned during that time.

<ul>
{% for page in current %}
    <li>
    {{ page.time }} - <a href="{{ page.url }}">{{ page.name }}</a>
    </li>
{% endfor %}
</ul>

## Targets

- [X] Bind server to IP address (*achieved 3/6/24*)
- [ ] Implement HTTP Protocol
- [ ] Implement concurrency to allow multiple connections
- [ ] Research security risks and implement safeguards

# About

My current project is to build a HTTP server from scratch (read more below about what I deemed as "from stratch").

I'm not entirely sure how the end "product" will look. Right now I believe I should be aiming for a "localhost dev environment", sort of like when you would build and serve a website in development on your computer before you host it on an actual server. After that, the server would be able to connect with clients across the Internet i.e. another computer.

# What do I mean "from scratch"?

As in I cannot simply search "how to make http server in C" cause that would be too easy. So no tutorials. Most of my research should be sourced from documentation (e.g. RFCs, `man` pages). 

There is a grey area when it comes to [textbooks](https://beej.us/guide/bgnet/html/split/index.html) or [indirect documentation](https://www.ibm.com/docs/en/i/7.5?topic=programming-how-sockets-work). These sources generally contain consolidated information about a concept, followed by examples to put those concepts into practice. I will be avoiding any code samples from those sources (at least for now).

Debugging should mainly come from reading documentation to figure out flaws in logic/usage. But I will allow myself to use StackOverflow.

So it's not an exact defintion of what it means to build from scratch, but the baseline should be **no tutorials**.