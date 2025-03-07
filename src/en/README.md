---
home: true
heroImage: /images/logo.svg
heroText: Build fast. Run fast.
tagline: Accelerate your web app development
actionText: Get Started →
actionLink: /en/guide/getting-started.html
features:
- title: Simple and lightweight
  details: Intuitive API with smart defaults and no bloat allows you to get straight to work building your app.
- title: Unopinionated and flexible
  details: Build the way you want to build without letting your tooling constrain you.
- title: Performant and scalable
  details: Built from the ground up with speed and scalability as a main concern. It is ready to power web applications big and small.
- title: Production ready
  details: Out of the box, it comes bundled with a web server ready to power your web applications.
- title: Trusted by millions
  details: Sanic is one of the overall most popular frameworks on PyPI, and the top async enabled framework
- title: Community driven
  details: The project is maintained and run by the community for the community.
pageClass: landing-page
logo: false
---

## The lightning-fast asynchronous Python web framework

**With the features and tools <span class="text-accent">you'd expect</span>.<br />And some <span class="text-accent">you wouldn't believe</span>.**

:::: tabs
::: tab "Production-grade"

After installing, Sanic has all the tools you need for a scalable, production-grade server—out of the box!

Including [full TLS support](#tls-server).

```python
from sanic import Sanic
from sanic.response import text

app = Sanic("MyHelloWorldApp")

@app.get("/")
async def hello_world(request):
    return text("Hello, world.")
```
```
▶ sanic path.to.server:app
[2023-01-31 12:34:56 +0000] [999996] [INFO] Sanic v22.12.0
[2023-01-31 12:34:56 +0000] [999996] [INFO] Goin' Fast @ http://127.0.0.1:8000
[2023-01-31 12:34:56 +0000] [999996] [INFO] mode: production, single worker
[2023-01-31 12:34:56 +0000] [999996] [INFO] server: sanic, HTTP/1.1
[2023-01-31 12:34:56 +0000] [999996] [INFO] python: 3.10.9
[2023-01-31 12:34:56 +0000] [999996] [INFO] platform: SomeOS-9.8.7
[2023-01-31 12:34:56 +0000] [999996] [INFO] packages: sanic-routing==22.8.0, sanic-testing==22.12.0, sanic-ext==22.12.0
[2023-01-31 12:34:56 +0000] [999997] [INFO] Sanic Extensions:
[2023-01-31 12:34:56 +0000] [999997] [INFO]   > injection [12 dependencies; 7 constants]
[2023-01-31 12:34:56 +0000] [999997] [INFO]   > openapi [http://127.0.0.1:8000/docs]
[2023-01-31 12:34:56 +0000] [999997] [INFO]   > http 
[2023-01-31 12:34:56 +0000] [999997] [INFO]   > templating [jinja2==3.1.2]
[2023-01-31 12:34:56 +0000] [999997] [INFO] Starting worker [999997]
```
:::

::: tab "TLS server"

Running Sanic with TLS enabled is as simple as passing it the file paths...
```
▶ sanic path.to.server:app --cert=/path/to/bundle.crt --key=/path/to/privkey.pem
```

... or the a directory containing `fullchain.pem` and `privkey.pem`

```
▶ sanic path.to.server:app --tls=/path/to/certs
```

**Even better**, while you are developing, let Sanic handle setting up local TLS certificates so you can access your site over TLS at [https://localhost:8443](https://localhost:8443)

```
▶ sanic path.to.server:app --dev --auto-tls
```
:::

::: tab Websockets

Up and running with websockets in no time using the [websockets](https://websockets.readthedocs.io) package.
```python
from sanic import Request, Websocket

@app.websocket("/feed")
async def feed(request: Request, ws: Websocket):
    async for msg in ws:
        await ws.send(msg)
```
:::

::: tab "Static files"

Serving static files is of course intuitive and easy. Just name an endpoint and either a file or directory that should be served.

```python
app.static("/", "/path/to/index.html")
app.static("/uploads/", "/path/to/uploads/")
```
:::

::: tab Lifecycle

Beginning or ending a route with functionality is as simple as adding a decorator.

```python
@app.on_request
async def add_key(request):
    request.ctx.foo = "bar"

@app.on_response
async def custom_banner(request, response):
    response.headers["X-Foo"] = request.ctx.foo
```

Same with server events.

```python
@app.before_server_start
async def setup_db(app):
    app.ctx.db_pool = await db_setup()

@app.after_server_stop
async def setup_db(app):
    await app.ctx.db_pool.shutdown()
```

But, Sanic also allows you to tie into a bunch of built-in events (called signals), or create and dispatch your own.

```python
@app.signal("http.lifecycle.complete")  # built-in
async def my_signal_handler(conn_info):
    print("Connection has been closed")

@app.signal("something.happened.ohmy")  # custom
async def my_signal_handler():
    print("something happened")

await app.dispatch("something.happened.ohmy")
```
:::

::: tab "Smart error handling"

Raising errors will intuitively result in proper HTTP errors:

```python
raise sanic.exceptions.NotFound  # Automatically responds with HTTP 404
```

Or, make your own:

```python
from sanic.exceptions import SanicException

class TeapotError(SanicException):
    status_code = 418
    message = "Sorry, I cannot brew coffee"

raise TeapotError
```

Regardless, Sanic comes with an algorithm that attempts to respond with HTML, JSON, or text-based errors as appropriate. Don't worry, it is super easy to setup and customize your error handling to your exact needs.
:::

::: tab "App Inspector"

Check in on your live, running applications (whether local or remote).
```python
▶ sanic inspect      

  ┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
  │                                                        Sanic                                                        │
  │                                          Inspecting @ http://localhost:6457                                         │
  ├───────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────┤
  │                       │     mode: production, single worker                                                         │
  │     ▄███ █████ ██     │   server: unknown                                                                           │
  │    ██                 │   python: 3.10.9                                                                            │
  │     ▀███████ ███▄     │ platform: SomeOS-9.8.7
  │                 ██    │ packages: sanic==22.12.0, sanic-routing==22.8.0, sanic-testing==22.12.0, sanic-ext==22.12.0 │
  │    ████ ████████▀     │                                                                                             │
  │                       │                                                                                             │
  │ Build Fast. Run Fast. │                                                                                             │
  └───────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────┘

  Sanic-Main
  	pid: 999996

  Sanic-Server-0-0
  	server: True
  	state: ACKED
  	pid: 999997
  	start_at: 2023-01-31T12:34:56.00000+00:00
  	starts: 1

  Sanic-Inspector-0
  	server: False
  	state: STARTED
  	pid: 999998
  	start_at: 2023-01-31T12:34:56.00000+00:00
  	starts: 1
```
:::

::: tab Extensible
In addition to the tools that Sanic comes with, the officially supported [Sanic Extensions](./plugins/sanic-ext/getting-started.md) provides lots of extra goodies to make development easier.

- **CORS** protection
- Template rendering with **Jinja**
- **Dependency injection** into route handlers
- OpenAPI documentation with **Redoc** and/or **Swagger**
- Predefined, endpoint-specific response **serializers**
- Request query arguments and body input **validation**
- **Auto create** HEAD, OPTIONS, and TRACE endpoints
- Live **health monitor**
:::

::::
