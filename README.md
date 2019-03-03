In this tutorial we're going to build a simple decentralized chat application which runs entirely in the browser.

All chat messages will be sent peer-to-peer from one browser to the other without going through a central server.

We're going to use the [Bugout](https://chr15m/bugout) JavaScript library to do this.

 * If you just want the end result of the tutorial you can download the [index.html]() file from this repository.
 * If you're actually looking for a proper decentralized browser chat application that is a bit more fleshed out check out [dirc](https://github.com/chr15m/dirc).

Ok, let's get started!

### Get started

To keep this tutorial simple we're going to do everything in a single `.html` file using pure Javascript. All you need is a text editor and a browser. We're not going to use any `node` based front-end development tools, minifiers, transpilers, etc.

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
