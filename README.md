# Hextris


## Install Server

Bash: \
`npm install web-dev-server`

Create file run.js.\
in run.js:

```javascript
var WebDevServer = require("web-dev-server");

// Create web server instance.
WebDevServer.Server.CreateNew()
   // Required.
   .SetDocumentRoot(__dirname)
   // Optional, 8000 by default.
   .SetPort(8000)
   // Optional, '127.0.0.1' by default.
   //.SetHostname('127.0.0.1')
   // Optional, `true` by default to display Errors and directories.
   //.SetDevelopment(false)
   // Optional, `null` by default, useful for Apache `mod_proxy` usage.
   //.SetBasePath('/node')
   // Optional, custom place to log any unhandled errors.
   //.SetErrorHandler(async function (err,code,req,res) {})
   // Optional, to prepend any execution before `web-dev-server` module execution.
   .AddPreHandler(async function (req, res, event) {
      if (req.GetPath() == '/health') {
       res.SetCode(200).SetBody('1').Send();
        // Do not anything else in `web-dev-server` module for this request:
         event.PreventDefault();
      }
      /*setTimeout(() => {
         throw new RangeError("Uncatched test error.");
      }, 1000);*/
   })
   // optional, callback called after server has been started or after error ocured
   .Start(function (success, err) {
      if (!success) console.error(err);
      console.log("Server is running.");
   });
```

Run server:\
`node run.js`

Create empty folder ./app, next to ./run.js with new empty file ./app/index.js
inside, executed as default directory content later.
Initialize web application instance in ./app/index.js:
```javascript
var WebDevServer = require("web-dev-server");

/**
 * @summary 
 * Exported class to handle directory requests.
 * 
 * When there is first request to directory with default 
 * `index.js` script inside, this class is automatically 
 * created and method `Start()` is executed.
 * All request are normally handled by method `HttpHandle()`.
 * If there is detected any file change inside this file 
 * or inside file included in this file (on development server 
 * instance), the module `web-dev-server` automaticly reloads 
 * all necesssary dependent source codes, stops previous instance 
 * by method `Stop`() and recreates this application instance again
 * by `Start()` method. The same realoding procedure is executed, 
 * if there is any unhandled error inside method `HttpHandle()` 
 * (to develop more comfortably).
 */
class App {

   constructor () {
      /**
       * @summary WebDevServer server instance.
       * @var {WebDevServer.Server}
       */
      this.server = null;
      /**
       * @summary Requests counter. 
       * @var {number}
       */
      this.counter = 0;
   }

   /** 
    * @summary Application start point.
    * @public
    * @param {WebDevServer.Server}   server
    * @param {WebDevServer.Request}  firstRequest
    * @param {WebDevServer.Response} firstResponse
    * @return {Promise<void>}
    */
   async Start (server, firstRequest, firstResponse) {
      this.server = server;
      // Any initializations:
      console.log("App start.");
   }

   /** 
    * @summary Application end point, called on unhandled error 
    * (on development server instance) or on server stop event.
    * @public
    * @param {WebDevServer.Server} server
    * @return {Promise<void>}
    */
   async Stop (server) {
      // Any destructions:
      console.log("App stop.");
   }
   
   /**
    * @summary 
    * This method is executed each request to directory with 
    * `index.js` script inside or into any non-existing directory,
    * inside directory with this script.
    * @public
    * @param {WebDevServer.Request}  request
    * @param {WebDevServer.Response} response
    * @return {Promise<void>}
    */
   async HttpHandle (request, response) {
      console.log("App http handle.");

      // increase request counter:
      this.counter++;
      
      // try to uncomment line bellow to see rendered error in browser:
      //throw new Error("Uncatched test error 1.");

      response
         .SetHeader('content-Type', 'text/javascript')
         .SetBody(
            JSON.stringify({
               basePath: request.GetBasePath(),
               path: request.GetPath(),
               domainUrl: request.GetDomainUrl(),
               baseUrl: request.GetBaseUrl(),
               requestUrl: request.GetRequestUrl(),
               fullUrl: request.GetFullUrl(),
               params: request.GetParams(false, false),
               appRequests: this.counter
            }, null, "\t")
         )
         .Send();
   }
};
module.exports = App;
```




Hextris
==========

<img src="images/twitter-opengraph.png" width="100px"><br>

An addictive puzzle game inspired by Tetris. Play it at [hextris.love2dev.com](hextris.love2dev.com).
[Read the Blog Post on How Hextris was made a Progressive Web App](https://love2dev.com/blog/https://love2dev.com/blog/pwa-hextris/)
[Watch the Video on How Hextris was made a Progressive Web App](https://www.youtube.com/watch?v=HkgjfDdTBYU)

Updated PWA Code By:
- Chris Love ([@ChrisLove](https://love2dev.com/))

Original Code By:
 - Logan Engstrom ([@lengstrom](http://loganengstrom.com/))
 - Garrett Finucane ([@garrettdreyfus](http://github.com/garrettdreyfus))
 - Noah Moroze ([@nmoroze](http://github.com/nmoroze))
 - Michael Yang ([@themichaelyang](http://github.com/themichaelyang))

# About
Hextris was originally created by a couple high school friends (who are now in college!) who unfortunately don't have as much time to update the game. If you'd like to support the open-source development of Hextris, please consider donating at:

**ETH:** `0xbf5414129552D37B4Fb12D058Cf1596B960d25b2`

The code was forked and updated to a Progressive Web App by [Chris Love](https://twitter.com/chrislove/). The purpose to show how easy it is to make a progressive web app and of course to have some fun!

##Read More

Read the PWA [Hextris Blog Announcement](https://love2dev.com/blog/pwa-hextris/) to learn more about making Progressive Web Apps.

##Progressive Web Apps

If you want to know more about progressive web apps visit my [long article](https://love2dev.com/blog/what-is-a-pwa/).

If you want Progressive Web Applicatition training sign up for my [PWA course](http://pwacourse.com).

## License
Copyright (C) 2018 Chris Love

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.


# Progressive Web Application Development by Example

<a href="https://www.packtpub.com/application-development/progressive-web-application-development-example?utm_source=github&utm_medium=repository&utm_campaign=9781787125421"><img src="https://www.packtpub.com/sites/default/files/B06922_MockupCoverNew.png" alt="Progressive Web Application Development by Example" height="256px" align="right"></a>

This is the code repository for [Progressive Web Application Development by Example](https://www.packtpub.com/application-development/progressive-web-application-development-example?utm_source=github&utm_medium=repository&utm_campaign=9781787125421), published by Packt.

**Develop fast, reliable, and engaging user experiences for the web**

## What is this book about?
Are you a developer that wants to create truly cross-platform user experiences with a minimal footprint, free of store restrictions and features customers want? Then you need to get to grips with Progressive Web Applications (PWAs), a perfect amalgamation of web and mobile applications with a blazing-fast response time.

This book covers the following exciting features:
* Explore the core principles of PWAs 
* Study the three main technical requirements of PWAs 
* Discover enhancing requirements to make PWAs transcend native apps and traditional websites 
* Create and install PWAs on common websites with a given HTTPS as the core requirement
* Get acquainted with the service worker life cycle 

If you feel this book is for you, get your [copy](https://amzn.to/2uTCevY) today!

<a href="https://www.packtpub.com/?utm_source=github&utm_medium=banner&utm_campaign=GitHubBanner"><img src="https://raw.githubusercontent.com/PacktPublishing/GitHub/master/GitHub.png" 
alt="https://www.packtpub.com/" border="5" /></a>


## Instructions and Navigations
All of the code is organized into folders. For example, Chapter02.

The code will look like the following:
```
 function renderResults(results) {

        var template = document.getElementById("search-results-
        template"),
            searchResults = document.querySelector('.search-results');
```

**Following is what you need for this book:**
Progressive Web Application Development by Example is for you if you’re a web developer or front-end designer who wants to ensure improved user experiences. If you are an application developer with knowledge of HTML, CSS, and JavaScript, this book will help you enhance your skills in order to develop progressive web applications, the future of app development.

With the following software and hardware list you can run all code files present in the book (Chapter 1-10).

### Software and Hardware List

| Chapter  | Software required                   | OS required                                                  |
| -------- | ------------------------------------| -------------------------------------------------------------|
| 1-10     | Nodejs 6.1 or above                 |Windows, MacOS or Linux supported by NodeJs and has a browser |
|          |  Chrome, Microsoft Edge,            |                                                              |
|          |   FireFox, Opera or Safari          |                                                              |
|          |  Visual Studio Code or Sublime      |                                                              |


### Related products <Paste books from the Other books you may enjoy section>
* Progressive Web Apps with React [[Packt]](https://www.packtpub.com/web-development/progressive-web-apps-react?utm_source=github&utm_medium=repository&utm_campaign=9781788297554) [[Amazon]](https://amzn.to/2Dvobn7)

* Hands-on Full Stack Development with Angular 5 and Firebase [[Packt]](https://www.packtpub.com/application-development/hands-full-stack-development-angular-5-and-firebase?utm_source=github&utm_medium=repository&utm_campaign=9781788298735) [[Amazon]](https://amzn.to/2DunAlA)

## Get to Know the Author
**Chris Love**
Chris Love is a frontend developer with 25 years of professional experience. He has won the Microsoft MVP award for 12 years and has authored multiple books. He has helped over 1,000 businesses of all sizes and from various industries.

Chris regularly speaks at user groups, code camps, and developer conferences, and also writes articles and videos to help fellow developers.

When he's not working on frontend development, you can find him spending time with his step-kids, doing karate, and taking part in Spartan races.
