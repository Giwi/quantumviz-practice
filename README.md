# Quantumviz Practice

## Requisites

At least you need [Python](https://doc.ubuntu-fr.org/python) runtime or 
[NodeJS](https://nodejs.org/en/download/package-manager/) (either using polymerCLI, see [CONTRIBUTE.md](CONTRIBUTE.md), 
either using a [simple HTTP Server](https://raw.githubusercontent.com/Giwi/angular2-beer/master/scripts/web-server.js)). 
Well, you need a HTTP server
## First step

Normally, you have some WarpScripts. 

First, [download](https://github.com/Giwi/quantumviz-practice/archive/master.zip) or
 `git clone git@github.com:Giwi/quantumviz-practice.git` this project an try to run it :

```bash
$ python -m SimpleHTTPServer
```

or

```bash
$ node web-server.js
```

## Analyze project

The file `index.html` is the entrypoint of our app. It refers (and load) the [Polymer](https://www.polymer-project.org/)
framework.

```html
<script src="./bower_components/webcomponentsjs/webcomponents-loader.js"></script>
```

And then import a Web-Component called `quantumviz-practice-app` with

```html
 <link rel="import" href="./src/quantumviz-practice-app/quantumviz-practice-app.html">
```

And finally call it : 

```html
<quantumviz-practice-app></quantumviz-practice-app>
```

In th file `src/quantumviz-practice-app/quantumviz-practice-app.html` we import a Quantum-Viz element dependencies : 

```html
<link rel="import" href="../../bower_components/warp10-quantumviz/warp10-geoquantumviz.html">
```

And the we use it : 

```html
<warp10-geoquantumviz 
    map-zoom="14"
    host="https://127.0.0.1/dist/api/v0/exec">
</warp10-geoquantumviz>
```

+ **map-zoom** : the initial zoom value (default : 10)
+ **host** : set the Warp10 url

There is some other options, see 
[bower_components/warp10-quantumviz/warp10-geoquantumviz.html](bower_components/warp10-quantumviz/warp10-geoquantumviz.html).

Now, as you can see, it displays nothing.

## Add a WarpScript

Don't forget to add a custom JSON structure at the end of your WarpScript

```bash
'gtsList' STORE
{ 
    'gts' $gtsList 
    'params' [ 
        { 
            'render' 'marker' 
            'marker' 'fuel' 
        } 
    ]
}
```

```html
<warp10-geoquantumviz host="https://127.0.0.1/dist/api/v0/exec">
 <!-- ADD YOUR WARP SCRIPT SHOWING CHEAPEST THE GAS STATION LOCATION HERE -->
</warp10-geoquantumviz>
```

Reload your browser, and it's magic!!

## Next

Any question?

Let's go further at [step 2](./src/step02/README.md)