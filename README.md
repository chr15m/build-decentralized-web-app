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


