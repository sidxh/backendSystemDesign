# Notes For Advanced Backend & System Design⚡

Welcome to my GitHub repository dedicated to backend engineering and system design! Here, you'll find organized notes covering a range of topics from fundamental principles to complex system architecture. I've created this space to document my daily learnings, insights and challenges while providing a valuable resource for learners and enthusiasts interested in building robust and scalable systems. Happy coding! 🚀

---

# Table of Contents

1. [Introduction](#introduction)
2. [Backend Fundamentals](#backend-fundamentals)
3. [Resources](#resources)

---

# 1] Understanding Request-Response Communication in Backend Systems

## Introduction:

In this section, we delve into the foundational concept of request and response, `a fundamental communication design pattern` prevalent in backend engineering. This pattern is elegant, classic, and ubiquitous across various domains. Our exploration will unravel the intricacies of this communication model, shedding light on the critical components of requests and responses.

## Defining a Request:

A request, in the context of a network environment, is the initiation of communication from a client to a server. The definition and understanding of a request become crucial, considering it's not a singular piece of data akin to traditional mail `but rather a continuous stream`, especially in TCP. Parsing a request involves identifying its start and end, a task demanding attention due to its computational cost.

## Processing Requests on the Server:

Upon receiving a request, the server must parse it, comprehending the distinction between multiple requests within a stream. This parsing process is not trivial, as `it involves understanding the boundaries and structure of the incoming data`. The expense of parsing requests underscores the significance of backend engineers comprehending this intricacy.

## Executing Requests:

Executing a request involves more than parsing; it requires the server to process the request based on its type. Whether it's a GET request or involves querying a database, understanding and executing the request's purpose is a distinct phase in the request-response lifecycle.

## Response Handling:

Once the server processes the request, it formulates a response, which follows a similar trajectory. The client must parse the response, and here, the concept of `serialization and deserialization` becomes pivotal. Choosing between JSON, XML, or protocol buffers for payload affects not only human readability but also impacts parsing efficiency and size.

## Where Request-Response is Utilized:

The request-response pattern finds extensive application across various domains, including web technologies `(HTTP)`, DNS resolution, Remote Procedure Calls `(RPC)`, SQL queries, and APIs like `REST` and `GraphQL`. Understanding where and how this pattern is applied provides a foundational grasp of backend communication.

## Challenges and Alternatives:

While request-response is pervasive, certain scenarios pose challenges. Long-running requests and scenarios where the client lacks knowledge of ongoing events require alternative approaches. `Polling` and asynchronous processing become essential tools in these cases, offering solutions to issues like client disconnection and latency in handling extensive requests.

> Quote

## Visualizing the Request-Response Lifecycle:

A visual representation illustrates the timeline of a request-response interaction, emphasizing the time spent in various stages, including request transmission, server processing, and response reception. This visualization aids in comprehending the sequential flow of actions in this communication model.

## Practical Demonstration with cURL:

To solidify our understanding, a practical demonstration using cURL showcases the intricacies of a simple HTTP request-response interaction. Examining the TCP connection establishment, DNS resolution, request headers, and response handling provides a hands-on perspective on the concepts discussed.

## Conclusion:

This section serves as a foundational exploration into the intricate world of request and response in backend systems. As we navigate through the complexities of parsing, executing, and handling responses, a deeper comprehension of the request-response paradigm will pave the way for more advanced discussions in subsequent sections.

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real


# 2] Synchronous vs Asynchronous Workloads

## Introduction:
- Synchronous vs asynchronous workloads are fundamental concepts in programming, critical for both backend and client-side development.
- The essence is understanding whether you can perform other tasks while waiting for a process to complete.

## Evolution of Execution:
- Initially, computing was synchronous; everything operated in perfect rhythm.
- Synchronous execution is likened to a sine wave, where client and server are in sync, moving together.
- Asynchronous execution, on the other hand, is when processes are not in sync, allowing parallelism.
- Asynchronous execution became ubiquitous in programming, reflecting real-world scenarios like asynchronous motors.

## Synchronous IO:
- In synchronous IO, the caller sends a request and gets blocked until a response is received.
- This blocking affects overall performance, especially in scenarios like file reading or network requests.
- Context switching occurs in the CPU, kicking out the blocked process, causing inefficiency.

## Asynchronous IO:
- Asynchronous IO allows the caller to continue work until a response is received.
- Two ways to check if the response is ready: polling (check if ready) and callback (receiver calls back when done).
- Node.js uses event loops and asynchronous callbacks to handle asynchronous IO efficiently.

### Example: Asynchronous Call in Node.js
```javascript
// Program spins up a secondary thread
// Secondary thread reads from disk
// Main program remains unblocked
// When thread finishes reading, it calls back the main thread
```

### Async/Await in Node.js:
- Async/Await is a syntax sugar that makes asynchronous code look synchronous.
- It allows blocking in the vicinity of the asynchronous operation without actually blocking the main execution.
- Async/Await is widely used in Node.js for asynchronous operations.

## Request-Response and Synchronicity:
- Synchronicity is often a client property in request-response scenarios.
- Most modern client libraries use asynchronous communication, allowing them to continue working while waiting for a response.

### Real-life Examples:
- Synchronous communication: Asking a question in a meeting where an immediate response is expected.
- Asynchronous communication: Sending an email and continuing other work while waiting for a response.
- Even chat messages in teams or Slack can sometimes be asynchronous.

## Asynchronous Backend Processing:
- In backend processing, the client is asynchronous, but the system is synchronous.
- Requests are queued, and a job ID is provided immediately to unblock the client.
- The backend processes requests in its own time, and clients can check the status asynchronously.

### Example: Using Queues for Asynchronous Backend Processing
```javascript
// Client sends a request
// Immediately gets a response with a job ID
// Backend processes the request in its own time
// Client can check the status asynchronously using the job ID
```

## Asynchronous Commits in Postgres:
- Postgres has asynchronous commits where the commit operation unblocks the client.
- This is achieved by flushing changes to a write-ahead log (WAL) asynchronously.

### Example: Asynchronous Commits in Postgres
```sql
-- Client initiates a commit
-- Postgres asynchronously flushes changes to the write-ahead log
-- The client is unblocked immediately
```

## Asynchronous vs. Synchronous Execution in Various Contexts
**1. Database Operations:**
- `Synchronous`:
In databases like PostgreSQL, synchronous commits block until the write operation is confirmed on disk.
Guarantees data durability but can be costly for large transactions.
Example:
```sql
BEGIN;
-- Perform operations
COMMIT;
```

- `Asynchronous:`
Asynchronous commits return success to the client before waiting for the write operation to disk.
Riskier, as failures might occur after returning success, leading to dirty reads.
Example:

```sql
BEGIN;
-- Perform operations
COMMIT ASYNC;
```

**2. I/O Operations:**
- `Synchronous (Blocking):`
Reading from a file synchronously waits until the operation is complete.
Example (Node.js):

```javascript
const fs = require('fs');
const data = fs.readFileSync('file.txt', 'utf-8');
```

- `Asynchronous (Non-blocking):`
Non-blocking reads allow the program to continue while waiting for the I/O operation to complete.
Example (Node.js):

```javascript
const fs = require('fs');
fs.readFile('file.txt', 'utf-8', (err, data) => {
  console.log(data);
});
```

**3. Operating System I/O Handling:**
- `Asynchronous I/O (epoll):`
Utilizes readiness notification rather than completion.
Checks if a file descriptor is ready for reading without blocking.
Example (Linux epoll):

```c
epoll_wait(epollfd, events, maxevents, timeout);
```

**4. File System Cache Bypass (fsync):**
`- Synchronous (fsync):`
Database systems may use fsync to bypass OS caches for data durability.
Linux drivers may dislike this due to potential fragmentation.
Example (Linux command):

```bash
echo 1 > /proc/sys/vm/drop_caches
```

In summary, asynchronous execution is prevalent across various domains, from databases to I/O operations and file system management. Understanding the implications and use cases for synchronous and asynchronous approaches is crucial for designing efficient and responsive systems.

## Conclusion:
Understanding the nuances of synchronous and asynchronous workloads is crucial for optimizing performance in both client-side and backend development. These concepts are foundational for building efficient and responsive systems.

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real


# 3] Push Model in Backend Execution

## Introduction to Push Model:
- Push is a design pattern for achieving real-time responses in the client.
- Ideal for scenarios where immediate results are crucial, like real-time notifications.
- Has both advantages and disadvantages, which will be discussed in detail.

## Request-Response Limitations:
- Not always suitable for certain workloads, such as real-time notifications.
- Examples: User logins, YouTube video uploads, Twitter tweets.
- In these cases, the server has knowledge of events, not the client.

## Understanding Push Model:
- Push model involves the server sending data to the client without a specific request from the client.
- Requires a bidirectional protocol for effective communication.
- Example: RabbitMQ's push approach in streaming messages to clients.

## Push Model Workflow:
- A client establishes a connection to the server.
- The server can send data to the client without the client making specific requests.
- Utilizes a bidirectional protocol, often implemented with web sockets for real-time communication.

## Advantages of Push Model:
- Real-time response: Pushes data to clients immediately upon events.
- Suitable for use cases like chat applications.

## Disadvantages of Push Model:
- Clients must be online to receive pushed data.
- Handling the load: Clients need to be capable of managing the load pushed by the server.
- Requires a bidirectional protocol.

## Web Socket Example:
- Demonstrated with a simple Node.js application using the WebSocket library.
- Establishes a bidirectional connection for real-time communication.
- Shows how messages sent by one client are pushed to all connected clients.

```javascript
// Sample WebSocket Server Implementation
const WebSocket = require('ws');
const server = http.createServer();

const connections = [];

const wss = new WebSocket.Server({ server });

wss.on('connection', (ws) => {
  // New connection established
  connections.push(ws);

  ws.on('message', (message) => {
    // Broadcast the message to all connected clients
    connections.forEach((connection) => {
      if (connection.readyState === WebSocket.OPEN) {
        connection.send(message);
      }
    });
  });

  ws.on('close', () => {
    // Handle disconnection, remove from connections array
    connections.splice(connections.indexOf(ws), 1);
  });
});

server.listen(8080, () => {
  console.log('WebSocket server is listening on port 8080');
});
```