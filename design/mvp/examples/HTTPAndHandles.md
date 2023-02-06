# HTTP And Handles Example

This example demonstrates the different types of handles as they would show up
in an HTTP interface.

TODO:
* incomplete
* not using `future`/`stream` or manually async
* See wasi-http for real proposal.

```wit
default world proxy {
  import default-outgoing-HTTP: pkg.handler
  import HTTP-backends: pkg.backends
  export HTTP: pkg.handler
}
```
TODO:
* explain basic structure

```wit
interface handler {
  use pkg.types.{request, response}
  handle: func(request) -> result<response>
}
```
TODO:
* explain `own`

```wit
interface backends {
  use pkg.types.{request, response}
  resource backend {
    handle: func(request) -> result<response>
  }
  get: func(name: string) -> use<backend>
}
```
TODO:
* motivate dynamic backend
* explain `use`

```wit
interface types {
  resource fields {
    get: func(key: string) -> list<string>
    ...
  }
  type headers = fields
  type trailers = fields

  resource body-and-trailers {
    constructor(byte-stream, option<trailers>)
    body: func() -> child use<byte-stream>
    finish: destructor func() -> option<trailers>
  }

  resource request {
    use io.streams.{byte-stream}
    constructor(headers, body-and-trailers, ...)
    headers: func() -> child use<headers>
    consume: func() -> child body-and-trailers
    ...
  }

  resource response { ... }
}
```
TODO:
* explain `child use` vs. own and why each
* link to wasi-http


[WASI HTTP]: https://github.com/webAssembly/wasi-http
