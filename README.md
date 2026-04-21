## Commit 1 Reflection Notes

I created a simple TCP server using Rust. The server listens on localhost (127.0.0.1:7878) using TcpListener.

When a browser sends a request, the server receives it as a stream and prints "Connection established!" in the terminal. However, the browser does not display anything because the server does not send any response back.

This shows that a web server works by listening to incoming connections and handling requests through streams. At this stage, the server only accepts connections but does not process or respond to them.

## Commit 2 Reflection Notes

I improved the server so it can return an actual HTTP response. The server now reads the request from the browser using BufReader and processes it line by line.

I learned that a web server must send a properly formatted HTTP response, including a status line (e.g., HTTP/1.1 200 OK), headers such as Content-Length, and the body (HTML content).

The server reads an HTML file (hello.html) and sends it back to the browser. This makes the browser able to render the page correctly. This helped me understand the basic structure of HTTP communication between client and server.s