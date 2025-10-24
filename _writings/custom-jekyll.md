---
base-title: Customizing your Jekyll site
base-description: what to know when creating your Jekyll site
time: Oct 23, 2025

draft: true
---

The Jekyll community already has a gallery of pre-made themes ready to use out-of-the-box. You can also crank out those developer skills and make your own Jekyll site.

I've been using Jekyll for my personal site for a few iterations now. Here are a few things that I've figured out:

---

# Code styling

Jekyll uses Rogue to convert any template text wrapped around code bblocks or {% raw %}`{% highlight %}` {% endraw %} into HTML tags with certain tags. But since you are starting from scratch, there is no styling provided.

Follow this [section](https://jekyllrb.com/docs/liquid/tags/#stylesheets-for-syntax-highlighting) to download the stylesheets and import it into your base SCSS file.
