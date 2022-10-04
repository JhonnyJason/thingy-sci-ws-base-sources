# thingy-sci-ws-base 

# Background
This code is primarily based on [thingy-sci-base](https://www.npmjs.com/package/thingy-sci-base) with the added capability of websocket connections.


# Usage

Current Functionality

---------------------
```coffeescript
prepareAndExpose = (middleWare, routes, port = 3333)
onWebsocketConnect = (wsRoute, handlerFunction)
getWSHandle = () -> WebSocketServerObject

```

`routes` is an object which actually is a map of `route -> function`
```coffeescript
routes = {}

routes.requestOne = (res, req) ->
    # req.body is our json
    # handle
    res.send("The Response!")

routes.requestTwo = (res, req) ->
    # req.body is our json
    # handle
    res.send("The other Response!")

```

## middleWare
- Could be a single function
- or an array of functions

These will be mounted before we mount the routes. e.G authorization

## SystemD Sockets
When there is `process.env.SOCKETMODE == true` then we will listen on systemd socket.

## Ports
Otherwise when there is `process.env.PORT != 0` then we will listen on that port.
Ony when we donot specify these environment variables the port we provide would be listened on.
If we provide nothing then the default is port 3333.

## Websockets
Internally we use [express-ws](https://www.npmjs.com/package/express-ws) which directly uses [ws](https://www.npmjs.com/package/ws).

So we have websocket functionailty available on specific routes we set a `onConnect` function on.

This we do via `onWebsocketConnect`
```coffeescript
onConnect = (socket, req) ->
    ## Handle newly connected socket
    socket.on("message", messageHandler)
    return

sciBase.onWebsocketConnect("/", onConnect)

```

For more general use of the websocket functionality, e.g. reaching other clients directly over the WebSocketServer Object, we can get this Object by `getWSHandle()`.

```coffeescript
WSServer = sciBase.getWSHandle()
allClients = WSServer.clients
```


## NO SSL!
Be aware this service is primarily thought to stay behind an nginx who terminates ssl for it - therefore proxypass to socket ;-). 

So we donot use SSL here which might be a security concern to be aware of.

---

All sorts of inputs are welcome, thanks!

---

# License
[Unlicense JhonnyJason style](https://hackmd.io/nCpLO3gxRlSmKVG3Zxy2hA?view)
