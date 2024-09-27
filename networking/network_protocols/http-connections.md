### Differences Between Persistent and Non-Persistent HTTP Connections

**HTTP connections** define how a client (typically a web browser) and a server communicate over the network to exchange data, such as web pages. The primary difference between **persistent** and **non-persistent** HTTP connections lies in how they handle multiple requests and responses.

### 1. **Non-Persistent HTTP Connections (HTTP/1.0)**

In **non-persistent** HTTP connections (also known as **short-lived connections**), each request/response pair is handled by a separate TCP connection. After the server sends the response to the client, the connection is immediately closed.

#### Key Characteristics of Non-Persistent HTTP Connections:

- **Separate TCP Connection for Each Request**:
  - Every request (such as requesting an HTML file, image, or script) requires a new TCP connection to be established.
  - Once the server sends the requested data, the connection is closed.
  
- **Connection Overhead**:
  - Establishing a new TCP connection requires a **three-way handshake**, which adds overhead (in terms of time and resources).
  - For websites with multiple resources (HTML, CSS, JavaScript, images), this can lead to delays, as the client must establish and close a connection for each file.

- **More TCP Handshakes**:
  - Since a new connection is established for each resource, each transaction requires a separate **TCP handshake** (setup and teardown of the connection), increasing the total latency.

- **Example**:
  - If a web page has multiple components (e.g., HTML, images, CSS files), each component is requested in its own connection, leading to multiple TCP handshakes and connection closures.

#### Example Flow of Non-Persistent HTTP Connection:

1. **Client** requests `index.html`.
2. The server sends `index.html` and **closes** the connection.
3. The client requests an image (`logo.png`) through a **new connection**.
4. The server sends the image and **closes** the connection.

### 2. **Persistent HTTP Connections (HTTP/1.1 and Later)**

In **persistent** HTTP connections (also called **long-lived connections**), the same TCP connection remains open for multiple requests and responses. This allows the client to reuse the connection to fetch additional resources without needing to establish a new connection each time.

#### Key Characteristics of Persistent HTTP Connections:

- **Single TCP Connection for Multiple Requests**:
  - A single TCP connection is established and used for multiple HTTP requests and responses.
  - The connection remains open until it is explicitly closed by either the client or the server, typically after a period of inactivity.

- **Reduced Latency and Overhead**:
  - The overhead of opening and closing connections is reduced since the connection is reused for multiple requests.
  - This reduces the **TCP handshake** overhead and improves performance, particularly for web pages that require multiple resources to load (HTML, images, CSS, JavaScript, etc.).

- **Pipelining** (HTTP/1.1 Feature):
  - **HTTP pipelining** allows a client to send multiple requests **without waiting for the corresponding responses**, further improving performance.
  - However, pipelining is not widely used or supported in modern browsers due to its complexity and potential issues. Most modern browsers and HTTP/2 implement **multiplexing**, which achieves better results.

- **Keep-Alive Mechanism**:
  - Persistent connections use the **Connection: keep-alive** header to inform the server that the connection should remain open.
  - If the client and server do not communicate for a period of time, the connection may eventually time out and close.

#### Example Flow of Persistent HTTP Connection:

1. **Client** requests `index.html` through a **single connection**.
2. The server sends `index.html` but **keeps the connection open**.
3. The client uses the **same connection** to request additional resources, such as `logo.png` and `style.css`.
4. Once all requests are completed, the connection can remain open for future requests or be closed after a timeout.

### Key Differences Between Persistent and Non-Persistent HTTP Connections:

| **Feature**               | **Non-Persistent HTTP**                          | **Persistent HTTP**                          |
|---------------------------|--------------------------------------------------|----------------------------------------------|
| **Connection Reuse**       | A new TCP connection is created for each request | A single TCP connection is reused for multiple requests |
| **Connection Overhead**    | High overhead due to multiple TCP handshakes     | Lower overhead since connections are reused  |
| **Performance**            | Slower for web pages with many components        | Faster, with reduced latency and improved performance |
| **Connection Closure**     | Connection closes after each response            | Connection remains open for multiple requests until explicitly closed |
| **Keep-Alive Support**     | Not supported in HTTP/1.0 (non-persistent mode)  | Supported in HTTP/1.1 and later with the "Connection: keep-alive" header |
| **TCP Handshakes**         | Multiple handshakes for multiple requests        | Single handshake for multiple requests       |
| **HTTP Pipelining**        | Not supported                                    | Supported in HTTP/1.1 (though rarely used)   |

### Example Use Cases:

- **Non-Persistent HTTP**:
  - Simple or single-request applications where only one resource needs to be transferred, such as small APIs or services that don’t require loading multiple resources.
  
- **Persistent HTTP**:
  - Modern web applications and websites that need to load multiple resources (images, CSS, JavaScript files) benefit from persistent connections.
  - Ideal for improving performance, reducing latency, and saving resources, especially on web pages with multiple components.

### Summary:

- **Non-persistent HTTP** opens and closes a separate TCP connection for each request, resulting in higher overhead due to multiple TCP handshakes.
- **Persistent HTTP** keeps the connection open across multiple requests, reducing latency, improving performance, and minimizing the overhead of repeated handshakes.
- **Persistent connections** are the standard in modern web protocols (HTTP/1.1 and HTTP/2), offering significant performance advantages, especially for websites that require the transfer of multiple resources.