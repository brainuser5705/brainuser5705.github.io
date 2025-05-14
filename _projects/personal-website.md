---
name: Personal Website
description: Development timeline of my personal website
time: 2021-05
tags: html/css php jekyll
permalink: /personal-website
---  
  
Over the past year, I have been developing my personal website and gone through several iterations while I experimented with different web development technologies. Here is a visual timeline of the landing page of my website. I will also include some notes about the features, the languages/technologies I used, and the general development process for each version.

---

## (V1) [Github Pages Static HTML and CSS](https://brainuser5705.github.io/website/)

![](https://i.imgur.com/QCN9UMQ.png)

**Languages/Technologies/Tools**
- HTML/CSS
- Github Pages

**Features**
- A static site with very crude HTML and CSS
- Features a blog page, but I never updated it (at this time, I wasn't aware of Jekyll)
- Largely inspired by [bgreco](https://bgreco.net)'s page

**Development Process**  
Nothing special, it's front end HTML and CSS.

---

## (V2) [Django Web Application](https://brainuser5705-website.herokuapp.com/)

![](https://i.imgur.com/OugkFVs.png)

**Languages/Technologies**
- HTML/CSS, Python
- Django framework
- Heroku (for hosting) with PostgreSQL addon

**Features**
- Status updates and updatable personal stats
- Includes a database where I can store all the data
- Admin page for easy data insertion (posting status updates, links, etc.)
- More refined frontend design

**Development Process**  
I had experience with the Django framework for a now defuncted project and wanted to use a database to store my site's content. I worked on this for about a month or so.

---

## (V3) [LAMP stack](https://ashleyliew-prod.herokuapp.com/)

![](https://i.imgur.com/dxjEZNf.png)

**Languages/Technologies**
- HTML/CSS, PHP
- Heroku with PostgreSQL

**Features**
- Formatted status updates and can support links, images
- Automated processes for data insertion (had to remove this after hosting with Heroku)
- Own domain (removed for new version)

**Development Process**  
My intentions of creating this version was to create a website "from scratch", using pure HTML/CSS and PHP. I came across the LAMP technology stack and started researching. Before starting, I had to learn PHP and re-learn SQL. I used XAMPP as a development environment with its built-in Apache and MariaDB. After that, I tried hosting it on my own Linux machine, but switched to Heroku. With Heroku hosting, I had to substantially change the backend design of my website including reworking file upload processes intended for my projects and blogs. Here is the [dev log](https://docs.google.com/document/d/1SA3ubiG70d9M1Hqk-DVgXof6wn9dMNGebQ_Oc5kBN2c/edit?usp=sharing) that I kept.

---

## (V4) Jekyll Site

![](https://i.imgur.com/hF062eS.png)

**Languages/Technologies**
- Jekyll
- Github Pages

**Features**
- Using Jekyll templates instead of creating from scratch
- Jekyll blogging system
- Large of use of Markdown for writing (as of now)
- No backend or database

**Development Process**  
The backend of the previous version was very insecure and inefficient. After multiple tweaks and fixes, it ended up like a static site that didn't need a database at all. I wanted to use Jekyll's "blog awareness" feature, clean file structure and easy workflow (at least compared to my LAMP stack), so I switched over. After a few hours of research and a full night of rewriting content, the site is up and hosting with Github Pages. Majority of the activity of my website lies in [status updates](https://status.ashleyliew.com), so to keep it I modified the LAMP stack to only be for status updates. As for blogging, it will be done on this site with Jekyll. 

---

## (V5, *Current Version*) Jekyll Revamp

![](https://i.imgur.com/HT5oMs4.png)

**Languages/Technologies**
- Jekyll
- Github Pages

**Features**
- Frontend redesign
- Cleaner project structure (projects collection)
- Minimalistic, project focused

**Development Process**  
The old version was very messy; I made it without any knowledge on how static site generators like Jekyll work. So I finally read up on the documentation and followed the step-by-step tutorial. I was unhappy with how messy the previous site design was as well, so I stripped it down to the main focus of my site which is to be hub for my projects. It now only has a list of the projects on the landing page and a simple about page.