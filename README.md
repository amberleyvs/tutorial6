## Commit 1 Reflection Notes

I created a simple TCP server using Rust. The server listens on localhost (127.0.0.1:7878) using TcpListener.

When a browser sends a request, the server receives it as a stream and prints "Connection established!" in the terminal. However, the browser does not display anything because the server does not send any response back.

This shows that a web server works by listening to incoming connections and handling requests through streams. At this stage, the server only accepts connections but does not process or respond to them.