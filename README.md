In this 15 minute tutorial we're going to build a simple decentralized chat application which runs entirely in the browser. All chat messages will be sent peer-to-peer from one browser to the other without going through a central server.

All you will need is a text editor, a web browser, and a basic knowledge of how to save HTML files and open them in the browser.

We're going to use the [Bugout](https://chr15m/bugout) JavaScript library to handle the peer-to-peer networking.

 * If you just want the files, download [index.html](./index.html) in this repo.

Ok, let's get started!

### HTML Boilerplate

To keep this tutorial simple we're going to do everything in a single `.html` file using pure Javascript. We're not going to use any `node` based build tools, minifiers, language transpilers, etc. You'll probably need those things when you build something more complicated but for the purposes of this tutorial let's stick with good old HTML & JavaScript.

The first thing we need is a basic HTML boilerplate into which we can start building our application. You can start with this:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta content="width=device-width, initial-scale=1" name="viewport">
  <title>Bugout tutorial chat</title>
</head>
<body>Hello world!</body>
</html>
```

Go ahead and save that to a file called `index.html` and then open that file directly in your browser.

Once you have it open in your browser you should see the words "Hello world!" on the screen. Hooray, and we're off!

### Importing Bugout

Let's get the [Bugout](https://chr15m/bugout) library imported so we can use it to connect people's browser's to eachother in a peer-to-peer way.

Add this `<script>` tag into the `<head>` section of the HTML to load the library directly from its GitHub page:

```html
<script src="https://chr15m.github.io/bugout/bugout.min.js" type="application/javascript"></script>
```

Save your `index.html` file again and hit re-load in the browser. If you know [how to use the developer console](https://www.digitalocean.com/community/tutorials/how-to-use-the-javascript-developer-console) in your browser you can check the network tab to verify the `bugout.min.js` file getting loaded in.

Now let's make a Bugout object that we can use to talk to other browsers. Create a new `<script>` tag section as follows and put it near the end of the file right before the final `</html>` tag. Putting it at the bottom means it will get run after everything else.

```html
<script>
var b = Bugout();
document.getElementById("log").innerHTML += "\n" + b.address() + "\n";
</script>
```

Now when you hit reload you should see "Hello world!" like before and on the next line you should see the address of the Bugout node. Something like this: `bKpdPiLJjPmwrYWoZYXVWbJFcEMUpfh6BN`.

You might notice this address looks a bit like a Bitcoin address. That's because Bugout uses a similar type of cryptographic technique to create its address from an internal cryptographic keypair. Cryptography is how Bugout nodes can be sure they are receiving information from the node they think they are receiving it from.

We're going to be doing a lot of printing things on the screen, so let's create a small function which will handle logging. Replace the code inside the `<script>` tag with this:

```javascript
var b = Bugout();
log(b.address());

function log(message) {
    document.getElementById("log").textContent += message + "\n";
}
```

### Aesthetic detour

I think aesthetics are important. Let's take a moment to make this page look at little more leet.

Add the following `<style>` block out of the way at the bottom of the page, below the `<script>` tag you just added and above the final `</html>` tag.

```html
<style>
  body { background-color: #333; }
  pre { color: #def; white-space: pre-wrap; word-wrap: break-word; text-shadow: 0 0 5px #fff; }
</style>
```

### Connecting browsers

Now we have our Bugout instance running in our page, how do we connect it to other Bugout instances running in pages on other people's computers?

In real life when you want to rendezvous with somebody you share the address of the place to meet. Computers are the same. Any time you want to connect two computer programs together over a network you need a way for them to find eachother - some type of address. That goes for programs running on a server somewhere, desktop programs, and also browser tabs. For example your browser had to find the server where you're reading this page and it did that using its domain name and URL ("web address").

Bugout instances running in browser tabs find eachother on the network by using the same address called an "identifier". This can be any string which you pass into Bugout at instantiation time. What string you use depends on the use-case. Sometimes the string will be one of the instance's own addresses and sometimes it will be some other identifier.

Let's have our Bugout instances meet at an address called "sweet decentralized chat tutorial". Note that everybody in the world doing this tutorial will be using the same address, so once you connect to this address you may see other Bugout instances connecting.

Change the code in the `<script>` tag so that instead of `var b = Bugout();` you instead have:

```
var b = Bugout("sweet decentralized chat tutorial");
b.on("seen", function(pk) { console.log("Seen:", pk); })
```

Just below that line add an event handler which will fire whenever we see some other Bugout instance, and print it on the log:

```

