Put loc in meta :
```bash
'ZZ63qGJSJ5gWc3QUIKbYXng_.mDHJjEJH.8XSygfHM7Bzle2UG9ljmzQpURlrcCuvlvAy1HUyLO3CieY6Aa_BwsNzFLZ5MYAniL6EQD.QUgpIycT7y9pz8rSjsV1GoTRZAX7XS1GNuOEINj6lpWY0f4IFjYaiF9S'
'READ' STORE
'qNBPFXtKLbhNj8a6YDWXzrpoTLnp.Ymq53iSlRdUgJ4ioZNkTDRoEATtpdE4XlaSHX7rUtJR8uYNntzHYhUwJp9K8ilJpFqhWmq1x5Z3EiC9sW5wpcN3.zdVDktTVkK1' 
'WRITE' STORE

$READ AUTHENTICATE 200000000 MAXOPS
   
[ $READ 'data.fuel' { } NOW -1 ] FETCH

<%
  // Remove list index
  DROP
  // Duplicate the GTS
  DUP
  [ SWAP <%
      // Extract lat + lon
      [ 4 5 ] SUBLIST FLATTEN LIST-> DROP
      // Convert to HHCode
      ->HHCODE
      // Set HHCode as attribute 'loc'
      {} SWAP 'loc' PUT SETATTRIBUTES
      // Return NO VALUE
      0 NaN NaN NaN NULL
    %> MACROMAPPER 0 0 0 ] MAP
    // Discard mapped GTS as we do not need it
    DROP
%> LMAP
$WRITE
META
```

Lowest price :
```bash
'ZZ63qGJSJ5gWc3QUIKbYXng_.mDHJjEJH.8XSygfHM7Bzle2UG9ljmzQpURlrcCuvlvAy1HUyLO3CieY6Aa_BwsNzFLZ5MYAniL6EQD.QUgpIycT7y9pz8rSjsV1GoTRZAX7XS1GNuOEINj6lpWY0f4IFjYaiF9S'
'READ' STORE

'POLYGON ((-0.60 47.25, -0.60 47.65, -0.35 47.65, -0.35 47.35, -0.60 47.25))' 0.01 false GEO.WKT
GEO.REGEXP '~(' SWAP + ')' +
'regexp' STORE

[ $READ 'data.fuel' { 'type' 'gazole' 'loc' $regexp } NOW -1 ] FETCH 'data' STORE
$data

NOW ISO8601
'2017-09-01T00:00:00.000000Z'
TIMECLIP
NONEMPTY

[ SWAP 
  <%
    // Extract lat + lon
    [ 3 7 ] SUBLIST FLATTEN DUP
    [ 1 2 ] SUBLIST LIST-> DROP
    47.48 -0.56
    HAVERSINE 'dist' STORE
    <% $dist 10000.0 > %>
    <% NULL 4 SET %>
    IFT
  %> MACROMAPPER 0 0 0 
] MAP NONEMPTY 

[ SWAP 
  bucketizer.min
  0
  31536000000000
  0
] BUCKETIZE

[ SWAP
  []
  reducer.min
] REDUCE
[ SWAP mapper.tostring 0 0 0 ] MAP

// Get station value and its metadata (Name of the station)
LIST-> DROP [ SWAP LOCATIONS ] FLATTEN LIST-> DROP ->HHCODE 'hfilter' STORE
[ $data [ ] { 'loc' $hfilter } filter.byattr ] FILTER
      [ SWAP mapper.tostring 0 0 0 ] MAP
'gtsList' STORE
{ 'gts' $gtsList 'params' [ { 'render' 'marker' 'marker' 'fuel' } ] }
```

Price evolution : 

```bash
'ZZ63qGJSJ5gWc3QUIKbYXng_.mDHJjEJH.8XSygfHM7Bzle2UG9ljmzQpURlrcCuvlvAy1HUyLO3CieY6Aa_BwsNzFLZ5MYAniL6EQD.QUgpIycT7y9pz8rSjsV1GoTRZAX7XS1GNuOEINj6lpWY0f4IFjYaiF9S'
'READ' STORE

[ $READ 'data.fuel' { 'type' 'gazole' 'cp' '29470' } NOW 365 d ] FETCH

'gts' STORE
{
  'gts' $gts
  'globalParams' { 'interpolate' 'linear' }
}
```