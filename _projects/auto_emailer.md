---
name: auto emailer
description: Python script to automate mass emailing tasks for my supplemental instructor job
time: 2021-09
tag: python
---

# **Backstory**

In the fall 2021 semester, I worked as an Supplemental Instructor for RIT's Academic Success Center. I supported GCIS-123, or Software Development and Problem Solving I, and my job was to hold study sessions for the students so they could review and engaged with the material outside of class. It's different from recitation in that I'm not lecturing, rather I'm creating [interactive activites](https://github.com/brainuser5705/gcis-123-si-sessions) for the students. 


A big part of the job is also **<u>marketing</u>** my study sessions. I won't lie and say that I did a whole lot of marketing, but I did periodcally send out weekly emails to remind the students that I was holding a study session. 

![](https://i.imgur.com/RnwHYID.png)
*Here is how a typical email would look like*

<br>

#### **Quick Summary On How It Works**

Authorization to use my <code>@rit.edu</code> domain email is done with the Gmail API. The email is a MIMEMultipart object from the <code>email.mime</code> module and is structured with HTML templates. The program inputs the user for the email details and formats the values into the templates with Python's built-in string formatting functions. Properties of the email, such as the subject and receiver emails, are set through "dictionary keys" of the MIME object. The emailing list is stored in a CSV file, along with the names of the students, and added to the <code>'bcc'</code> property. The email is then sent with the API.

<br>

#### **Development Process**

I originally used the <code>SMTP</code> module to send out the MIME objects, however it turned out that authorization with Gmail OAuth 2.0 is necessary because of <code>@rit.edu</code> emails requires a log-in to the <code>rit.edu</code> domain. Eventually I scrapped the SMTP functionality altogether.

![](https://i.imgur.com/AnplDo8.png)
*Here's a picture of a email send out with auto-emailer*

<br>

The way the sending functionality works (aka the <code>send</code> function) is it parses through the CSV file to get the name and email of the students, loops through emails to add to the <code>'bcc'</code> property of the MIME object email, creates a base64 encoding of the message, and executes the Gmail `send(...).execute` function *(honestly I am not very knowledgable on how Gmail API actually sends out the email, you can read more about it on their documentation)*...

<br>

...However a problem arises when I want to create custom emails, let's say adding a greeting to the student *("Hi John Smith")*. In order to create this custom email format, I would have to create a new `send()` function that formats, attach, and encode/sends an MIME object email for each individual entry in the csv file. This is contrary to a mass email where I just need to create one MIME object that is the mass email. I thought about making a "all-in-one" function or some interface, but I haven't worked on it since. To make it more extensible, I created a `Message` interface so I can create different types of `Message`s which all share the same `send()` functionality implemented for mass emails.  

<br>

#### Command line!

The user interface is just a nice colored command line interface using the `colorama` and `termcolor` module which provides functions for printing colored text on the terminal, configured to allow me to backtrack if I ever make a mistake.

<div class="pic">
<img src="https://i.imgur.com/HZu3TTV.png">
The user interface
</div>

---

#### **Further Work?**

This idea was inspired by a colleauge told me about their emailing program that allowed him to send out scheduled mass emails (back when Google didn't have scheduled emails) for work. It works by sending emails with **threads**, setting them to sleep for an alloted time before it is ready to send.

<br>

Ideally for marketing, I would send out the first reminder email a day or two before the study session and another reminder email an hour right before the study session starts. I wanted to implement the same threading concept where once I send out the first email, a shorter reminder email would be created, send with an asleep thread and scheduled to wake up right before the session. Instead of keeping my laptop on for a whole day, I decided to run the program with **PythonAnywhere**, a service that lets you host your Python scripts online. After a really quick refresh up on threading, I was able to implement it, but with the stress of college and other pursuits, I wasn't able to make it function 100% and haven't worked on it since. 
