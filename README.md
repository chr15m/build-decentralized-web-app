In this 15 minute tutorial we're going to build a simple decentralized chat application which runs entirely in a web browser.

All you will need is a text editor, a web browser, and a basic knowledge of how to save HTML files and open them in the browser.

We're going to use [Bugout](https://chr15m/bugout), a JavaScript library that takes care of the peer-to-peer networking and cryptography.

 * If you just want the files, download [index.html](./index.html) in this repo.

Ok, let's get started!

### Start with the HTML boilerplate

To keep this tutorial simple we're going to do everything in one `.html` file using pure Javascript. We're not going to use any build tools, minifiers, language transpilers, etc. You'll probably need those things when you build something more complicated but for the purposes of this tutorial we'll stick with good old fashioned HTML and JavaScript.

The first thing we need is a basic boilerplate web page into which we can start building our application. We also need a simple function to output text on the screen. Here's a snippet you can use:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta content="width=device-width, initial-scale=1" name="viewport">
  <title>Bugout chat tutorial</title>
  <style>
    body { background-color: #333; font-size: 1.5em; padding: 0em 0.25em; }
    pre { color: #fff; white-space: pre-wrap; word-wrap: break-word; text-shadow: 0 0 10px #ccc; }
  </style>
  <script>
    function log(message) {
      document.getElementById("log").textContent += message + "\n";
    }
  </script>
</head>
<body><pre id="log"></pre></body>
<script>
  log("Hello world!");

  /***** Your code goes here! *****/
  
</script>
</html>
```

Go ahead and save the snippet above into a file called `index.html` and then open that file in your web browser.

You should see the words "Hello world!" in white text at the top of the screen.

Great, we are up and running with a basic web page and a `log()` function which will print text on the screen.

### Import Bugout

Now let's get the [Bugout](https://chr15m/bugout) library imported so we can use it to connect peoples' browsers together in a peer-to-peer style. We'll load the library directly from its GitHub page.

Add this `<script>` tag into the `<head>` section of the HTML just before the closing tag:

```html
<script src="https://chr15m.github.io/bugout/bugout.min.js" type="application/javascript"></script>
```

Save your `index.html` file again and hit refresh in the browser. If you know [how to use the developer console](https://www.digitalocean.com/community/tutorials/how-to-use-the-javascript-developer-console) you can check the network tab to verify the `bugout.min.js` file getting loaded in. If you don't, don't worry just skip this step and move on.

### Make a Bugout object

Let's make a Bugout object that we can use to talk to other browsers. Add the following code at the end of the file in the script tag after it says "Your code goes here!":

```javascript
var b = Bugout();
log(b.address());
```

Now when you hit reload you should see "Hello world!" like before and on the next line you should see the address of this Bugout instance. It will look something like this: `bKpdPiLJjPmwrYWoZYXVWbJFcEMUpfh6BN`.

You might notice this address looks a bit like a Bitcoin address. That's because Bugout uses a similar type of cryptographic technique to create its address from an internal cryptographic keypair. Cryptography is how Bugout nodes can be sure they are receiving information from the node they think they are receiving it from. On the network Bugout nodes can find and identify eachother using these addresses.

### Connect your Bugout instance

Now that we have a Bugout instance running in our web page, how do we connect it to other Bugout instances running in pages on other people's computers?

In real life when you want to meet up with somebody you share the address of the place to meet. Computers are the same. Any time you want to connect two computer programs together over a network you need some type of address. For example to get to this web page you followed a link to it's URL, and your computer loaded this page from that address.

Bugout instances connect to addresses called "identifiers" which you can think of as room names. The first argument passed to the `Bugout()` instance is the identifier or room name that you want it to connect to.

If you don't supply a room name argument the Bugout instance will connect to it's own `.address()` by default. That means it will listen out for other Bugout instances connecting back to it. Other instances can connect by passing your Bugout instance's `.address()` in as their first argument.

For our chat room we want to connect all the Bugout instances together in one room. We do that by using the same room name as the first argument.

Update the code to pass an argument `"bugout-chat-tutorial"` as the room name. We'll also install an event handler which will fire every time we see another Bugout instance connecting to the same room using `b.on("seen")`.
```javascript
var b = Bugout("bugout-chat-tutorial");
b.on("seen", function(pk) { log(pk + " [ seen ]"); });
```

When you refresh the page now you may see other Bugout instances connecting - those are other people doing this same tutorial! You can open the index.html in another tab or browser and after a few seconds in both windows you should see the Bugout instances discover eachother and output `...address... [ seen ]`.

