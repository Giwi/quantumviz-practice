# Step 2

Happy to see you here :)

Now you will develop yourself a sparkline widget by using 2 components from 
[warp10-iron](https://github.com/cityzendata/warp10-iron) and 
[warp10-quantumviz](https://github.com/cityzendata/warp10-quantumviz) : 

+ warp10-iron/warp10-warpscript-caller
+ warp10-quantumviz/warp10-display-chart

1. import dependencies
1. place HTML markup
1. add some javascript vars
1. handle the ajax request
1. go drink a coffee, you're a Polymer Guru now

## Import dependencies

```html
<link rel="import" href="../../bower_components/warp10-quantumviz/warp10-display-c3-chart.html">
<link rel="import" href="../../bower_components/warp10-iron/warp10-warpscript-caller.html">
```

## Place HTML markup

We will use Polymer template vars: 

```html
<warp10-display-chart 
    data="{{data}}" 
    tooltip
></warp10-display-chart>

<warp10-warpscript-caller 
    auto 
    url="{{_baseUrl}}"
    warpscript="{{warpscriptScript}}" 
    on-response="_handleResponse"
    on-error="_handleError"
></warp10-warpscript-caller>

<warp10-geoquantumviz 
    map-zoom="14" 
    host="{{_baseUrl}}">
    <!-- your previous WarpScript -->
</warp10-geoquantumviz>
```


Wait.... what is `{{stuff}}` ? This is a template var. But wait, how to set it? Easy, let's start with
`{{_baseUrl}}` : 

```javascript
static get properties() {
    return {};
}
``` 
This is the place for common vars : 

```javascript
static get properties() {
    return {
        _baseUrl: {
            type: String,
            value: 'http://127.0.0.1:8080/api/v0/exec',
        }
    };
}
```
Then let's see `{{warpscriptScript}}` : 

```javascript
ready() {
    super.ready();
}
```

The `ready()` function is called at initialization time. We can now set the `warpscriptScript` var : 

```javascript
ready() {
    super.ready();
    this.warpscriptScript = ``;
}
```

We have no warpscript for now. We will set it later. We must finish the plumbing before.

## warp10-warpscript-caller

This is our ajax requester : 

- **auto** : auto send the request at initialization time (after `ready()`)
- **url** : the warp10 url
- **warpscript** : the WarpScript to send
- **on-response** : the handler of the HTTP response
- **on-error** : the handler of the HTTP error (just in case)

Let's implement the handlers. In the javascript section (just after `ready()`) :

```javascript
_handleResponse(event, response) {
    console.log('_handleResponse', response);
    this.data = response.stack;    // we set the data var used by warp10-display-chart 
    console.log('_handleResponse - parsed - dataChanged', this.data);
}

_handleError(event, error) {
    // ok, a bit more tricky, but taake it as-is
    if (error.request.xhr.responseText.match(/<pre>\s*(.*)\s*<\/prse>/)) {
        this.errorMsg = error.request.xhr.responseText.match(/<pre>\s*(.*)\s*<\/prse>/)[1];
    } else {
        this.errorMsg = error.request.statusText;
    }
    console.log('_handleError', { error: error, errorMsg: this.errorMsg });
}
```

## warpscriptScript 

It's up to you to code the WarpSript. We want to display a sparkline which shows the evolution of the gazole price for 
a given town (let's say Plougastel Daoulas - 29470 or whatever you want in France) during this year.

Don't forget to add this at the end of the WarpScript :

```bash
'gts' STORE
{
    'gts' $gts
    'globalParams' { 'interpolate' 'linear' }
}
```


Hey, you can even change the kind of graph, replace `linear` by `area` or `spline` or `area-spline`

You can also shoose colors : 

```bash
{
  'gts' $gts
  'params' [ { 'color'  '#FF9900' } { 'color' '#039be5' } ]
  'globalParams' { 'interpolate' 'area-spline' }
}
```

## The end

There's all kind of visualization in

- [Quantum-Viz](https://github.com/cityzendata/warp10-quantumviz)
- [Warp10-tiles](https://github.com/cityzendata/warp10-tiles)

So, be curious and browse examples.

Thanks!
