## Commit 1 Reflection Notes

I created a simple TCP server using Rust. The server listens on localhost (127.0.0.1:7878) using TcpListener.

When a browser sends a request, the server receives it as a stream and prints "Connection established!" in the terminal. However, the browser does not display anything because the server does not send any response back.

This shows that a web server works by listening to incoming connections and handling requests through streams. At this stage, the server only accepts connections but does not process or respond to them.

## Commit 2 Reflection Notes

I improved the server so it can return an actual HTTP response. The server now reads the request from the browser using BufReader and processes it line by line.

I learned that a web server must send a properly formatted HTTP response, including a status line (e.g., HTTP/1.1 200 OK), headers such as Content-Length, and the body (HTML content).

The server reads an HTML file (hello.html) and sends it back to the browser. This makes the browser able to render the page correctly. This helped me understand the basic structure of HTTP communication between client and server.s

## Commit 3 Reflection Notes

I implemented request validation and selective response handling. The server now reads the first line of the HTTP request and matches it against known routes.

If the request is "GET / HTTP/1.1", the server returns the main HTML page. Otherwise, it returns a 404 page. This simulates basic routing behavior found in real web frameworks.

I learned that the first line of an HTTP request contains important information such as method and path. By matching this line, we can control how the server responds to different URLs.

## Commit 4 Reflection Notes

Here, i made a slow request by adding a sleep delay of 10 seconds for a specific route (/sleep). This shows the limitation of a single-threaded server.

When I opened two browser tabs, one accessing /sleep and another accessing /, I see that the second request was delayed until the first one finished. This shows that the server processes requests sequentially, not concurrently.

If multiple users send slow requests, the server becomes blocked and cannot handle new requests efficiently.

## Commit 5 Reflection Notes

I improved the web server by introducing concurrency using a ThreadPool. Previously, the server handled requests sequentially, where each request had to wait for the previous one to finish. This caused performance issues, especially when handling slow requests such as the `/sleep` endpoint.

To solve this, I implemented a ThreadPool abstraction. Instead of spawning a new thread for every incoming request, the server now creates a fixed number of worker threads at startup. These workers continuously wait for tasks and execute them as they arrive.

When a new request is received, the server sends the task (a closure containing the request handler) to the ThreadPool using the `execute` method. The ThreadPool then assigns the task to one of the available worker threads. This allows multiple requests to be processed concurrently.

One important concept in this implementation is the use of the `move` keyword in closures. This ensures that ownership of the `TcpStream` is transferred into the worker thread, preventing lifetime and borrowing issues when the thread outlives the main function scope.  

After implementing the ThreadPool, I tested the server by accessing `/sleep` and `/` simultaneously in different browser tabs. Unlike the previous single-threaded version, the fast request (`/`) was no longer blocked by the slow request (`/sleep`), this means that concurrency is working correctly.