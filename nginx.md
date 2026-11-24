# Nginx (Engine-X)

## 1. Definition

**Nginx** (pronounced *Engine-X*) is an open-source, high-performance web server that can also function as a:

- Reverse proxy
- Load balancer
- HTTP cache
- Mail proxy

In simple words, Nginx receives incoming client requests and either serves the requested content directly or forwards the requests to the appropriate backend server. [1][7]

## 2. Why Do We Need Nginx?

We use Nginx to:

- Serve static files such as HTML, CSS, JavaScript, and images
- Act as a reverse proxy between clients and backend applications
- Distribute incoming traffic across multiple servers using load balancing
- Improve performance through caching
- Handle HTTPS connections using SSL/TLS
- Manage incoming HTTP traffic efficiently

Nginx is commonly used as an entry point for web applications because it can route traffic, serve files, and forward API requests to backend services. [1][4][8]

## 3. How Does It Work?

1. A user sends an HTTP or HTTPS request through a browser.
2. The request reaches the Nginx server.
3. Nginx examines the requested URL and configuration rules.
4. It either serves static content directly or forwards the request to a backend application.
5. The backend response is returned to the client.

### Example: MERN Application

```text
Client Browser
      |
      v
    Nginx
      |
      ├── /                → React frontend static files
      └── /api/products    → Node.js backend API
                                 |
                                 v
                           MongoDB Atlas
```

- `/` → Nginx serves the React frontend files.
- `/api/products` → Nginx forwards the request to the Node.js backend.
- Node.js communicates with MongoDB Atlas and returns the response.
- Nginx sends the final response back to the client.

Nginx commonly uses `proxy_pass` to forward requests to a backend application server. [1][4]

## 4. Advantages

- High performance and efficient resource utilization
- Handles many concurrent connections
- Supports reverse proxy and load balancing
- Provides static file serving and caching
- Supports HTTPS through SSL/TLS
- Open-source and lightweight

Nginx supports HTTP, HTTPS, TCP, and UDP load balancing, and can be configured as a reverse proxy for backend services. [2][5][7]

## 5. Disadvantages

- Configuration can be complex for beginners
- Advanced configurations require networking knowledge
- Debugging complex configurations can be challenging
- Some advanced features require additional modules or external tools

## 6. Real-World Example

Suppose we deploy a MERN e-commerce application on AWS EC2.

| Component          | Responsibility                                   |
|--------------------|--------------------------------------------------|
| React              | Frontend user interface                          |
| Node.js + Express  | Backend APIs                                     |
| MongoDB Atlas      | Cloud database                                   |
| Nginx              | Web server, reverse proxy, and traffic management |
| PM2                | Keeps the Node.js application running            |

Nginx acts as the entry point for incoming web traffic and routes requests to the appropriate resources. [1][4]

## Important Note

**Nginx is not a replacement for Node.js or MongoDB.**

It performs a different role in the application architecture:

- **React** builds and displays the frontend.
- **Node.js + Express** handles application logic and APIs.
- **MongoDB** stores application data.
- **Nginx** handles incoming traffic, serves static files, and routes requests.
- **PM2** manages and keeps the Node.js process running.
