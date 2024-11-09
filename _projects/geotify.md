---
name: Geotify
description: Geographize your Spotify listening history by mapping out country of origin
time: 2021-11
tags: python spotify-api django
---

**Geotify** is a web application that accesses your Spotify listening history geodata. It uses the Spotify API and Last.fm API (for longer listening history).

<div class="pic">
<img style="border: 1px solid;" src="/assets/images/statify-landing.jpeg"> 
Landing page
</div>

<div class="pic">
<img style="border: 1px solid;" src="/assets/images/statify-results.jpeg">
Results page
</div>

# About

This project was a way for me to applied what I recently learned about APIs and how to use them. I got familiar with using the concept of `GET`/`POST` requests, attaching parameters into the URL, and using Python's `requests` module. I also got insight into how an API is structured with endpoints, how it is basically a webserver, and how it responses to requests with `JSON` or `XML` data.

---

Now I will delve into how it works.

# Technologies

Here is the list of tools and services that Statify uses:

- **Django**: web server to accept Spotify API responses (they require a *redirect URI*)
- **Spotify API**: to get listen history from Spotify
- **Musicbrainz API**: to get the country of origin of the artists
- **Google Charts**: using their Javascript generated geochart to make the heat map

# Making requests to the APIs

All the APIs used were REST web APIs, meaning requests are in the form of urls. I used the Python `requests` library with its `get()` and `post()` functions to make the requests. The basic usage of these functions, is passing in the API endpoint to make request to, along with a Python dictionary for the parameters and header.

# Spotify API Authorization Code Flow

The entire process is detailed [here](https://developer.spotify.com/documentation/general/guides/authorization/code-flow/).

All the authorization requests in Statify are made in `stats.spotify.authorization`. Here is the basic steps to have a user authorize Statify access to their account.

1. Make a Spotify application, taking note of its *client id*.
2. Include the necessary parameters in a request to the https://accounts.spotify.com/authorize endpoint, and gets an user authorization code as a response.
3. Now with the authorization, send a request to https://accounts.spotify.com/api/token to create an access token which is used to make requests to the API (or in other words, get the actual data).

Authorization code is requested with the `index` view, and the redirect URI is the `callback` view which then redirects to the `geotify` view where the data is displayed.

# Musicbrainz API - Getting the Country

Since the Spotify doesn't store the country of origin of the artist, I use MusicBrainz and their API. There is no authorization needed for Musicbrainz.

Using Musicbrain's *search* GET request variation and their `artist` endpoint, I got the artist in the Musicbrainz database. `area` is a response property of the *Artist entity*, representing an *Area* entity and has a property itself `type`. The goal is to get an Area with the `type`  Country.

Some artists have an `area` property but its immediate type is not a Country, so I will have to search through the *relationships*. So I make a request with the `area` id along with an extra `inc` parameter set to `area-rels`. The response is a list of relationships, and I will have to find the first Area with a `direction` property set to *"backwards"*. The search is [recursive](https://community.metabrainz.org/t/python-api-country-of-origin-for-artists/491476) until the Country is found, or there are no relations. 

**ex:**   
-> Alanta: https://musicbrainz.org/ws/2/area/`26e0e534-19ea-4645-bfb3-1aa4e83a4046`?inc=area-rels

-> Georgia: https://musicbrainz.org/ws/2/area/`26e0e534-19ea-4645-bfb3-1aa4e83a4046`?inc=area-rels

-> United States: https://musicbrainz.org/ws/2/area/`489ce91b-6658-3307-9877-795b68554c98`?inc=area-rels

*Only the MBID of the Area is changed in the URL.*

# Google Charts Heat Map

I need to get the [ISO codes](https://www.iban.com/country-codes), so I made a very crude web scraper that allows me to get the country name along with its ISO-3166 codes (Alpha-2).   

To create the matrix with the country's ISO code and its artist count, I first get the artist list from the Spotify API. Then for each artist in the list, I will get their country of origin (name and ISO) with the Musicbrainz API. The count is stored in a Python dictionary with the ISO as the key and the count as the value. So I will access the dictionary with the ISO and increment the count. Finally, I convert the dictionary into a 2D list to pass in as a template tag for Django.

# Sessions

The Django frameworks has a sessions backend readily available for use. All I have to do is set the `sessions` attribute of the current request/response and pass in the desired value, and Django will store the session in its session database. I am storing the token from the APIs.

# AJAX Request

Because of the API request limits by time, I can only ping the APIs so much before I can get banned. That also means that generating the data will take long. As of now, Musicbrainz limits their requests to *1 per second*. So that means if you have 100 unique artists, your generation will take at least more than 100 seconds. So instead of keeping the users on the same screen for 100 seconds, Geotify uses AJAX for asynchronous requests while the screen is updated to a loading page.