# Interface to Google Drive in Racket

## Fred Martin
### April 22, 2017

# Overview
This set of code provides an interface to searching through one's Google Drive account. 
Its most important feature is that it provides a *folder-delimited search*.

The essential model of files in Google Drive is that they are in one big “pile.” So you can't directly find a file in a
given folder.

This code recursively collects all folders found within a given folder, and then 
construct a search query that includes a list of all the subfolders (flattened into a single list).

This then allows you to perform a folder-delimited search.

**Authorship note:** All of the code described here was written by myself.

# Libraries Used
The code uses four libraries:

```
(require net/url)
(require (planet ryanc/webapi:1:=1/oauth2))
(require json)
(require net/uri-codec)
```

* The ```net/url``` library provides the ability to make REST-style https queries to the Google Drive API.
* Ryan Culpepper's ```webapi``` library is used to provide the ```oauth2``` interface required for authentication.
* The ```json``` library is used to parse the replies from the Google Drive API.
* The ```net/uri-codec``` library is used to format parameters provided in API calls into an ASCII encoding used by Google Drive.

# Key Code Excerpts

Here is a discussion of the most essential procedures, including a description of how they embody ideas from 
UMass Lowell's COMP.3010 Organization of Programming languages course.

## Initialization using a Global Object

The following code creates a global object, ```drive-client``` that is used in each of the subsequent API calls:

```
(define drive-client
  (oauth2-client
   #:id "548798434144-6s8abp8aiqh99bthfptv1cc4qotlllj6.apps.googleusercontent.com"
   #:secret "<email me for secret if you want to use my API>"))
 ```
 
 While using global objects is not a central theme in the course, it's necessary to show this code to understand
 the later examples.
