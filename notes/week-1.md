# Week 1 Notes

Great work this week! We covered a lot this week and it's only going to pick up from here. We want to reiterate a few things:

1. This class is all about you. We don't want this to turn into a "Branick and Jimmy talk for 2 hours" thing.
2. You'll get out of this what you put in. We're totally cool with it if this isn't a priority for you - we're all busy people. But if you really want to master the topics we're teaching then it will require some hard work on your part, especially if you're new to the programming world.
3. That being said, we are here to help *you*. You can always reach out to us outside of class with questions as the arise. Like we mentioned in class, between the two of us we've seen a ton of different obscure errors and corner cases so chances are we'll be able to help you out. And if we can't, we will find someone who can.

Below is a recap of all of the topics that we touched on in the first class. Each class is going to build on the previous class and we won't necessarily revisit these topics in-depth again, but each new topic will refer back to these basics.

# Next Week Prep

1. A text editor of your choice, preferably VSCode ([download here](https://code.visualstudio.com/)).
2. Inspect 1 website using Chrome Dev Tools and explore the HTML - [Tutorial Here](https://medium.com/@branick/introduction-to-chrome-developer-tools-a67f225793b2)
3. Setup Github if you don't already have an account ([registration link](https://github.com/))

## HTTP Verbs

Before covering requests, let's talk about the different types of requests. These are called HTTP verbs and the ones that you'll mainly be dealing with are `GET` `POST`  `PUT` and `DELETE`. There's a link in the section at the bottom to the full list of verbs if you're interested. These four verbs are the basics of Restful API's and allow you to Create, Read, Update, and Delete objects. 

- GET = Read = used for getting things
- POST = Create = used to post data and create and object
- PUT = Update = used to post data to update and existing object 
- DELETE = Delete used to delete an object

When you request a page from a website, you are `GET`ting that site. Notice that the verb fits nicely in the context - it makes sense. When you submit your login information to the same site and ask to be authenticated, you are `POST`ing your data to the server so that it can do something with that data. You wouldn't send data like that with a `GET` request for security reasons we'll cover in the future, but also because it logically doesn't make sense. You wouldn't `GET` a website by giving it specific data. You would `POST` that data.

If you are trying to delete your account from a website, you will be sending a `DELETE` request to the server, along with the required feilds.

## Requests

When you refresh a page in Chrome, type something into the address bar, or pull up a mobile app, you requesting data from the server. Requests are just text. They all follow a similar format. Here's an example request:

```
GET https://google.com HTTP/1.1
Host: google.com
Connection: close
User-Agent: Paw/3.1.5 (Macintosh; OS X/10.14.0) GCDHTTPRequest
```

There are a few important things to remember about this request format. First, `GET` specifies the verb that you are performing. Remember - the protocol was written by humans like you and me so a lot of this is more straightforward than it looks.

Then, note the headers. Headers are a way to send arbitrary data with each request. The example we used in class was authentication. A website needs to know which user to return data for, so you might program your app to include a header like this:

```
Authentication: jimmy@email.com
```

We'll talk in a future class about why that specific format is super insecure, but for now just know that headers can essentially be used for any extra pieces of data that you want to communicate to the host.

Every request, regardless of the response type, will return text. It's up to the client (the person or piece of software receiving the response) to parse and understand the text it sends back. For example, Chrome acts as a web-client, meaning that it can receive and interpret HTML documents and convert it into something that is nice to look at. The entire internet is just text - nothing more, nothing less.

## Responses

Responses are similar to requests in the sense that they are only - you guessed it - text. Here's an example response we used in class:

```
HTTP/1.1 200 OK
Server: Cowboy
Connection: close
Content-Type: application/json; charset=utf-8
Content-Length: 1829
Date: Wed, 06 Feb 2019 18:54:16 GMT
Via: 1.1 vegur

{"myData": "Data"}
```

Note the similarities between this format and the request format - this is what the browser gets back when you ask it for data. It has to parse through this information, find the response body (in this case `{"myData": "Data"}`) and figure out how it wants to display that information.

The big point we're trying to get across with requests and responses is that everything is up to interpretation. The server's job is **not** to tell the end user how to render, view, or understand the data. The server's job is to send the data back and move on to the next task. It's up to the client (machine, web browser, app, etc) to get that data from the response and display it in a way that is meaningful for its specific context. This is a really important topic because when that starts to click you'll also start to understand how the same server can be used to build multiple types of clients - web applications, mobile apps, etc.

## Status Codes

A critical part of all responses is the status code that is returned. There are a few main status codes that you'll be dealing with, although the list of full status codes is much, much longer (see the links section for reading on that). Here are the most common ones:

- `200`: This means "Ok, I did what you asked and everything went fine, here's your data"
- `201`: This means "I successfully created the data that you sent to me."
- `400`: This means "Hey, you gave me something that was incorrect and I need you to fix it before I give you the data that you want."
- `404`: This means "I have no idea what you're looking for, reformulate your request with the correct path and try again."
- `500`: This means "Everything has broken." (That may be a little dramatic, but `500` codes are not good.)

Notice the pattern with the codes here - codes that start with `2` are good, codes that start with `4` indicate a fixable problem, and codes that start with `5` mean that something needs to be fixed on the server's end.

We're going to be talking a lot about the correct codes to send in the correct situations. Similar to other protocols that exist, there is no strict definition of when to use each code for each purpose in the sense that your application will not break if you use `400` to indicate success. However, that's the wrong way of doing things, as the client will probably interpret that incorrectly.

## Routes

When you ask a server to return a response, you need to direct your request to a specific URL. Typically, people think about URLs as `www.google.com`. Let's break that down:

- `www` is the subdomain. Like we talked about in class, these are often used for vanity purposes rather than technical reasons, although we'll cover this more when we get to the deployment section.
- `google` is the root domain.
- `.com` is the TLD, or Top Level Domain. These are regulated by ICANN and some of them are restricted - you can't go out and buy a `.gov` TLD, because that would be insanity.

Now, take the following domain: `www.mydomain.com/login`. This looks a little different than `www.google.com`, mostly because of the `/login` part. This is known as the route, or the path. Once you own the domain `www.mydomain.com`, you can have an infinite number of routes on that domain. You are not limited by any concrete number - you can parse and interpret the routes in whatever way you please. We'll dive much deeper into that when we jump into the backend (remember - all of this is still mostly on the frontend side of things).

## Query Strings

Query strings, similar to headers, are a way to add extra context to a request. For example, Google uses query strings to encode your search query: `www.google.com/?q=Boston%20College` (We'll cover `%20` later, but for now just know that it means "Space" to Google.) Unlike routes, extraneous or wrong query parameters do not automatically throw 404 errors. If you went to `www.google.com/adfkajsdflkasjfsdkfj`, you'll get a 404 error. But if you go to `www.google.com/?the_best=boston-college` you'll just get Google's homepage. They don't care about that extra data because they're simply not looking for it.

## HTML

HTML stands for Hyper Text Markup Language. This is the language that is used to create elements for websites. We're going to dive into the specific format of HTML next week during our class, but for now just know that every website in the world uses HTML - it's the primary format that web browsers like Chrome can understand and turn into visual sites. Here's an example of HTML:

```html
<html>
    <head>
        <title>My Really Good Site</title>
    </head>
    <body>
        <h1>Hello, World!</h1>
    </body>
</html>
```

Without us teaching you much about HTML, see if you can identify the common patterns in the code snippet. Just like almost everything else we're going to learn in this course, HTML is just a text standard developed by humans, so it makes sense.

## Chrome Dev Tools

The frontend code for every website in the world is completely publicly accessible. It's important to note though that this does **not** mean that the backend code is available as well. You will not be able to see how a website connects to the database, handles login, creates new data, etc. But you *can* see how they construct the page and how they style the site. There's a link to instructions on Chrome Dev Tools in the bottom section - we're going to use it a lot, so you should start getting familiar with it. Here's a brief guide to get you started: https://medium.com/@branick/introduction-to-chrome-developer-tools-a67f225793b2

# Links to topics discussed

- <https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods>

- <https://en.wikipedia.org/wiki/List_of_HTTP_status_codes>

- [https://aryeo.com](https://aryeo.com/)

- [https://gotranseo.com](https://gotranseo.com/)

- <https://developers.google.com/web/tools/chrome-devtools/>

- https://medium.com/@branick/introduction-to-chrome-developer-tools-a67f225793b2

  
