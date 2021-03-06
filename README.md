# SwiftWebSocket

<a href="https://tidwall.github.io/SwiftWebSocket/results/"><img src="https://tidwall.github.io/SwiftWebSocket/build.png" alt="" width="93" height="20" border="0" /></a>

Conforming WebSocket ([RFC 6455](https://tools.ietf.org/html/rfc6455)) client library implemented in pure Swift.

[Test results for SwiftWebSocket](https://tidwall.github.io/SwiftWebSocket/results/). You can compare to the popular [Objective-C Library](http://square.github.io/SocketRocket/results/)

SwiftWebSocket currently passes all 521 of the Autobahn's fuzzing tests, including strict UTF-8, and message compression.

## Features

- Pure Swift solution. No need for Objective-C Bridging.
- Reads compressed messages (`permessage-deflate`). [IETF Draft](https://tools.ietf.org/html/draft-ietf-hybi-permessage-compression-21)
- Strict UTF-8 processing. 
- The API is modeled after the [Javascript API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket).
- TLS / WSS support.
- `binaryType` property to choose between `[UInt8]` or `NSData` messages.
- Zero asserts. All networking, stream, and protocol errors are routed through the `error` event.
- High performance. 

##Install (iOS and OS X)

###CocoaPods

You can use [CocoaPods](http://cocoapods.org/?q=SwiftWebSocket) to install the `SwiftWebSocket` framework.

Add the following lines to your `Podfile`.

```ruby
use_frameworks!
pod 'SwiftWebSocket'
```

The `import SwiftWebSocket` directive is required in order to access SwiftWebSocket features.

###Drop-in

Just drop the `WebSocket.swift` file into your project.  
You must also add the `libz.dylib` library. `Project -> Target -> Build Phases -> Link Binary With Libraries`

There is no need for `import SwiftWebSocket` when manually installing.


###Example


```swift
func echoTest(){
    var messageNum = 1
    var ws = WebSocket(url: "wss://echo.websocket.org")
    var send : ()->() = {
        var msg = "#\(messageNum++): \(NSDate().description)"
        println("send: \(msg)")
        ws.send(msg)
    }
    ws.event.open = {
        println("opened")
        send()
    }
    ws.event.close = { (code, reason, clean) in
        println("close")
    }
    ws.event.error = { (error) in
        println("error \(error.localizedDescription)")
    }
    ws.event.message = { (message) in
        if let text = message as? String {
            println("recv: \(text)")
            send()
        }
    }
}
```

## Contact
Josh Baker [@tidwall](http://twitter.com/tidwall)

## License

The SwiftWebSocket source code is available under the MIT License.
