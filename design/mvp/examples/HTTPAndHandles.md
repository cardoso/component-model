# HTTP And Handles Example

This example demonstrates the different types of handles as they would show up
in an HTTP interface.

TODO:
* incomplete
* not using `future`/`stream` or manually async
* See wasi-http for real proposal.

```wit
package wasi:http

world proxy {
  import handler
  import backends
  export handler
}
```
TODO:
* explain basic structure

```wit
interface handler {
  use types.{request, response}
  handle: func(request) -> result<response>
}
```
TODO:
* explain `unique` as the default

```wit
interface backends {
  use types.{request, response}
  resource backend {
    handle: func(request) -> result<response>
  }
  get: func(name: string) -> rc<backend>
}
```
TODO:
* motivate dynamic backend
* explain how methods like `handle` take `call-scoped use` by default and why not `call-scoped borrow`
  * explain why `borrow` makes sense in async future and `exclusive func`
* explain unscoped `rc` return value

```wit
interface types {
  use wasi:io/streams.{input-stream, output-stream}

  resource fields {
    get: func(key: string) -> list<string>
    ...
  }
  type headers = fields
  type trailers = fields

  resource request {
    constructor(headers, output-stream, ...)
    headers: func() -> child use<headers>
    consume: func() -> child input-stream
    ...
  }

  resource response { ... }
}
```
TODO:
* explain `destructor` is `call-scoped` `unique` 
* explain `child` vs. `child use` vs. `child borrow`

TODO:
* show Wat instance type for all above
* count combinations out of 12 used


[WASI HTTP]: https://github.com/webAssembly/wasi-http
