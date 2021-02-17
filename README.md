# Hummingbird Lambda

Run Hummingbird inside an AWS Lambda

## Usage

Create struct conforming to `HBLambda`. Set the `In` and `Out` types. If you are using an `APIGateway` REST interface to invoke your Lambda then set these to `APIGateway.Request` and `APIGateway.Response` respectively. If you are using an `APIGateway` HTML interface then set these to `APIGateway.V2.Request` and `APIGateway.V2.Response`. If you are using any other `In`/`Out` types you will need to implement the `request(context:application:from:)` and `output(from:)` methods.

The `init` function is where you setup your application. Add your middleware, add route handlers etc

```swift
struct MyHandler: HBLambda {
    // define input and output
    typealias In = APIGateway.Request
    typealias Out = APIGateway.Response
    
    init(_ app: HBApplication) {
        app.middleware.add(HBLogRequestsMiddleware(.debug))
        app.router.get("hello") { _ in
            return "Hello"
        }
    }
}

Lambda.run { context in
    return HBLambdaHandler<MyHandler>(context: context)
}
```
