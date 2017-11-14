# Quantumviz Practice

## Requisits

At least you need [Python](https://doc.ubuntu-fr.org/python) runtime or [NodeJS](https://nodejs.org/en/download/package-manager/) (either using polymerCLI, see [CONTRIBUTE.md](CONTRIBUTE.md), either using a [simple HTTP Server](https://raw.githubusercontent.com/Giwi/angular2-beer/master/scripts/web-server.js)). Well, you need a HTTP server
## First step

Normaly, you have some WarpScripts. 

First, [download](https://github.com/Giwi/quantumviz-practice/archive/master.zip) this projet an try to run it :
```bash
$ python -m SimpleHTTPServer
```

or

```bash
$ node web-server.js
```

## Analyze project

`index.html` is the entrypoint of our app. It refers (and load) the [Polymer](https://www.polymer-project.org/) framework.

```html
<script src="./bower_components/webcomponentsjs/webcomponents-loader.js"></script>
```

And then import a Webcomponent called `quantumviz-practice-app` with

```html
 <link rel="import" href="./src/quantumviz-practice-app/quantumviz-practice-app.html">
```

And finally call it : 

```html
<quantumviz-practice-app></quantumviz-practice-app>
```