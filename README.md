# Notes For Advanced Backend & System Design⚡

Welcome to my GitHub repository dedicated to backend engineering and system design! Here, you'll find organized notes covering a range of topics from fundamental principles to complex system architecture. I've created this space to document my daily learnings, insights and challenges while providing a valuable resource for learners and enthusiasts interested in building robust and scalable systems. Happy coding! 🚀

---

# Table of Contents
<a id='table-of-contents'>

1. [Understanding Request-Response ](#understanding-request-response)
2. [Synchronous vs Asynchronous Workloads](#sync-vs-async-workloads)
3. [Push Model in Backend Execution](#push-model)
4. [Polling Design Pattern Communication](#polling-design)
5. [Long Polling Design Pattern](#long-polling)
6. [Server-Sent Events](#server-sent-events)
7. [Publish-Subscribe Pattern](#publish-subscribe-pattern)
8. [Multiplexing vs Demultiplexing](#mux-demux)
9. [Stateless vs. Stateful Architectures](#stateless-stateful)
10. [Understanding Sidecar Pattern](#sidecar-pattern)
11. [Introduction to Protocols](#intro-to-protocols)
12. [Understanding the OSI Model](#osi-model)

<a id='understanding-request-response'>

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

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='sync-vs-async-workloads'>

---


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

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='push-model'>

---


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

## Issues with the Example:
- The example doesn't handle scenarios where clients disconnect.
- A more robust implementation would include error handling and client disconnection checks.

## Conclusion:
- Push model is powerful for real-time scenarios.
- Understanding its advantages and disadvantages is crucial for effective implementation.
- Future sections will delve deeper into WebSocket implementation and considerations for push models.

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='polling-design'>

---

# 4] Polling Design Pattern Communication

In this section, we delve into the polling design pattern, specifically focusing on short polling. Short polling is a commonly used and straightforward communication style, especially suitable for scenarios where a request takes a long time to process.

## Short Polling Overview:
**Definition:** Short polling involves the client sending a request to the server, which immediately responds with a handle (unique identifier) for the request. The backend processes the request asynchronously, and the client uses the handle to poll for the status.

**Use Cases:** Short polling is beneficial when dealing with long-running requests, such as uploading a YouTube video. Instead of waiting for the entire process to complete, the client receives a handle and can check for the status asynchronously.

## Short Polling Workflow:
1. The client submits a request.
2. The server responds immediately with a unique identifier (handle).
3. The backend processes the request asynchronously.
4. The client polls the server using the handle to check the status.
5. Once the request is complete, the server responds with the result.
   
## Pros and Cons of Short Polling:
**Pros:**
- Simplicity: Short polling is easy to implement on both the client and server sides.
Suitable for Long-Running Requests: Ideal for scenarios where requests take a considerable amount of time to process.

**Cons:**
- Chattiness: Short polling can lead to a high number of requests, causing network congestion and wasting resources.
- Network Bandwidth: The frequent polling may consume significant network bandwidth, impacting performance.
- Resource Utilization: Backend resources are used to handle unnecessary poll requests, affecting overall efficiency.

## Short Polling Example:
Let's consider a Node.js application that simulates short polling for job submission and status checking.

```javascript
const express = require('express');
const app = express();

const jobs = {};

function updateJob(jobId) {
  setTimeout(() => {
    if (jobs[jobId] < 100) {
      jobs[jobId] += 10;
      updateJob(jobId);
    }
  }, 5000);
}

app.post('/submit', (req, res) => {
  const jobId = Date.now().toString();
  jobs[jobId] = 0;
  updateJob(jobId);
  res.json({ jobId });
});

app.get('/check-status', (req, res) => {
  const jobId = req.query.jobId;
  console.log(`Checking status for job ${jobId}`);
  res.json({ progress: jobs[jobId] });
});

app.listen(8080, () => {
  console.log('Server is running on port 8080');
});

```
In this example, a client can submit a job, receive a job ID, and then poll for the status using that ID.

## Issues with Short Polling:
The main drawback of short polling is its chattiness, which can lead to unnecessary network traffic and resource utilization. In the next section, we'll explore a more efficient alternative known as long polling.

Stay tuned for the upcoming section on "Long Polling," where we address the shortcomings of short polling and introduce a more sophisticated approach.

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='long-polling'>

---


# 5] Long Polling Design Pattern

In this section, we'll explore the long polling design pattern, a technique employed to address the chattiness issues associated with short polling. Long polling is particularly prominent in systems like Kafka, providing an alternative approach to communication between clients and servers.

## Introduction to Long Polling:

- **Origin:** Long polling became prominent around 40 years ago, especially in the context of Kafka, a distributed streaming platform. It is a mechanism that deviates from the immediate response of short polling.

- **Purpose:** Long polling is used when a request takes a considerable amount of time, and the client wants to be notified only when the response is ready. It's a way of saying, "Talk to me when it's ready."

## Long Polling Workflow:

1. The client sends a request to the server.
2. The server responds immediately with a handle (unique identifier), similar to short polling.
3. Instead of an immediate response, the server does not write anything to the socket. It effectively waits until the response is ready.
4. The client, being asynchronous, continues its operations.
5. Once the server determines that the response is ready, it sends the response back to the client.

## Long Polling Example:

Let's consider a scenario similar to short polling, but with long polling. A client submits a job, receives a job ID, and then checks the status using long polling.

```javascript
const express = require('express');
const app = express();

const jobs = {};

async function checkJobComplete(jobId) {
  return new Promise((resolve) => {
    setTimeout(() => {
      if (jobs[jobId] < 100) {
        resolve(false);
      } else {
        resolve(true);
      }
    }, 1000);
  });
}

app.post('/submit', async (req, res) => {
  const jobId = Date.now().toString();
  jobs[jobId] = 0;

  // Simulating long polling
  while (!(await checkJobComplete(jobId))) {}

  res.json({ jobId });
});

app.get('/check-status', (req, res) => {
  const jobId = req.query.jobId;
  console.log(`Checking status for job ${jobId}`);
  res.json({ progress: jobs[jobId] });
});

app.listen(8080, () => {
  console.log('Server is running on port 8080');
});
```

This example introduces a `checkJobComplete` function that uses a promise-based approach to simulate long polling. The server effectively waits until the job is complete before responding.

## Pros and Cons of Long Polling:

### Pros:
- **Less Chatty:** Long polling reduces the number of requests, making it more efficient than short polling.
- **Backend-Friendly:** Suitable for scenarios where the backend prefers fewer requests and can handle asynchronous processing.

### Cons:
- **Not Real-Time:** Due to the polling mechanism, long polling may not provide real-time updates. Clients may experience delays in receiving messages or data.
- **Timeouts:** There's a need for careful management of timeouts to avoid indefinite waits.

## Conclusion:

Long polling offers a valuable alternative to short polling, especially in scenarios where reducing chattiness is crucial. While it may not provide real-time updates, it strikes a balance between client responsiveness and backend efficiency.

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='server-sent-events'>

---

# 6] Server-Sent Events (SSE)

Server-Sent Events (SSE) is a powerful design pattern that transforms the traditional request-response model of HTTP into a streaming server model. This approach is particularly elegant, enabling real-time communication between clients and servers without resorting to more complex protocols like WebSockets.

## Understanding Server-Sent Events:

- **Origin:** SSE is a pure HTTP concept that doesn't rely on additional protocols. It was designed to address the limitations of the traditional request-response model.

- **Unending Response:** The key feature of SSE is that it provides an unending response. While the response is being sent, it doesn't have a definitive end; instead, it contains a continuous stream of data.

- **Chunk Streaming:** Data is sent to the client in chunks, and the client is smart enough to interpret and pause these mini-responses, effectively treating them as events.

## How SSE Works:

1. The client sends a special request with a specific content type, indicating it is ready to receive server-sent events.
2. The server, instead of providing a final response, sends a series of events in chunks.
3. Each event starts with the keyword "data" and ends with two new lines. The client parses these events as they arrive.

## Use Cases for SSE:

SSE is particularly useful for scenarios where real-time notifications are required. For example:
- User login events.
- Incoming messages.
- Any scenario where immediate, continuous updates are essential.

## SSE Example:

Let's look at a simple example using Express.js for the server and the EventSource object in the browser for the client.

**Server-side code:**
```javascript
const express = require('express');
const app = express();

let counter = 0;

app.get('/', (req, res) => {
  res.send('Hello from server!');
});

app.get('/stream', (req, res) => {
  res.setHeader('Content-Type', 'text/event-stream');
  res.setHeader('Cache-Control', 'no-cache');
  
  function sendEvent() {
    res.write(`data: Hello from server, event ${counter}\n\n`);
    counter++;
  }

  // Send an event every second
  setInterval(sendEvent, 1000);
});

app.listen(8888, () => {
  console.log('Server is running on port 8888');
});
```

**Client-side code:**
```javascript
const eventSource = new EventSource('http://localhost:8888/stream');

eventSource.onmessage = (event) => {
  console.log(`Received: ${event.data}`);
};
```

In this example, the server sends an event every second, and the client captures and logs these events in the console. The key headers set by the server (`Content-Type` and `Cache-Control`) indicate that this is an SSE endpoint.

## Pros and Cons of SSE:

### Pros:
- **Real-Time Updates:** SSE provides real-time communication between the server and the client, making it suitable for scenarios requiring immediate updates.
- **Compatible with HTTP:** Works within the HTTP framework, making it easy to implement without additional protocols.

### Cons:
- **Client Must Be Online:** Since SSE relies on continuous responses, the client needs to be online to receive updates.
- **Connection Limits:** In some scenarios (e.g., Chrome with HTTP/1.1), there might be connection limits that affect the number of SSE connections a client can establish.

## Conclusion:

Server-Sent Events offer an elegant solution for scenarios where real-time updates are crucial. Despite some limitations, SSE stands out as a straightforward and effective pattern, allowing for continuous communication within the familiar HTTP environment. In the next section, we'll explore more communication patterns, so stay tuned!


> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='publish-subscribe-pattern'>

---

# 7] Publish-Subscribe Pattern with RabbitMQ

The Publish-Subscribe pattern is a powerful design pattern for backend communication, especially in scenarios where multiple services need to communicate without direct connections. This pattern involves publishers, which publish information to a central server or broker, and subscribers, which consume the information they are interested in. In this case, RabbitMQ, a message broker, is used to facilitate communication between publishers and subscribers.

## Understanding the Need for Publish-Subscribe:

### Traditional Request-Response Model Challenges:
- **Scaling Issues:** When multiple servers or services need to communicate, the traditional request-response model becomes challenging to scale.
- **High Coupling:** The services become tightly coupled, making it difficult to manage changes or additions.
- **Blocking Workflow:** In scenarios like video upload processing, the traditional model can lead to blocking workflows, where each step waits for the previous one to complete.

### Introducing Publish-Subscribe:
- **One Publisher, Many Readers:** In the Publish-Subscribe model, a single publisher can broadcast information to multiple subscribers without direct connections.
- **Decoupling Services:** This pattern reduces coupling between services, allowing them to operate independently.
- **Asynchronous Workflow:** Publishers can publish information and move on, allowing subscribers to consume the data at their own pace.

## Example Scenario - Video Processing Workflow:

### Traditional Request-Response Workflow:
1. User uploads a video.
2. Video upload service processes the video.
3. Processed video is sent to compression service.
4. Compression service completes and sends the video to format service.
5. Format service notifies notification service.
6. Notification service notifies users.

### Challenges:
- Blocking at each step.
- Coupling between services.
- User experience may suffer due to waiting.

### Publish-Subscribe Workflow:
1. User uploads a video and receives an ID.
2. Video upload service publishes the raw video to a "Raw MP4 Videos" topic.
3. Compression service subscribes to the "Raw MP4 Videos" topic, processes, and publishes to "Compressed Videos" topic.
4. Format service subscribes to "Compressed Videos" topic, formats, and publishes to respective topics (e.g., 1080p, 720p, 4K).
5. Notification service subscribes to the relevant format topics and notifies users.

### Advantages:
- Asynchronous and non-blocking.
- Reduced coupling between services.
- Scalable and flexible.

## Using RabbitMQ for Publish-Subscribe:

### RabbitMQ Setup:
1. Create a RabbitMQ instance on a cloud service.
2. Retrieve the AMQP URL for connecting to the RabbitMQ server.

### Publisher (Node.js Code Example):
```javascript
const amqp = require('amqplib');

async function publishToQueue(number) {
  const connection = await amqp.connect('AMQP_URL');
  const channel = await connection.createChannel();
  
  await channel.assertQueue('Jobs');
  
  const content = { number };
  channel.sendToQueue('Jobs', Buffer.from(JSON.stringify(content)));
  
  console.log(`Published job with input ${number}`);
  
  await channel.close();
  await connection.close();
}

publishToQueue(107);
```

### Consumer (Node.js Code Example):
```javascript
const amqp = require('amqplib');

async function consumeFromQueue() {
  const connection = await amqp.connect('AMQP_URL');
  const channel = await connection.createChannel();
  
  await channel.assertQueue('Jobs');
  
  channel.consume('Jobs', (message) => {
    const job = JSON.parse(message.content.toString());
    console.log(`Received job with input ${job.number}`);
  }, { noAck: true });
}

consumeFromQueue();
```

### Notes on Acknowledgment:
- In RabbitMQ, acknowledging the receipt of a message is crucial.
- If a consumer fails to acknowledge, the message may be redelivered.

## Conclusion:
The Publish-Subscribe pattern, when implemented with tools like RabbitMQ, provides an elegant solution for decoupling services and facilitating asynchronous communication. It enhances scalability, flexibility, and the overall user experience. By using message brokers, such as RabbitMQ, engineers can implement efficient communication patterns in their systems.

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='mux-demux'>

---

# 8] Multiplexing vs Demultiplexing

## Introduction

In the realm of networking, understanding the concepts of multiplexing and demultiplexing is crucial. These terms find relevance in popular protocols like HTTP and technologies such as Quick and Multipath TCP. In this discussion, we'll delve into the distinctions between multiplexing and demultiplexing, exploring how they operate and their significance in various scenarios.

## Multiplexing

### Definition

Multiplexing involves combining multiple signals or data streams into a single line or channel. It is akin to merging several connections into one, efficiently transmitting various signals through a unified pathway.

### Examples

#### 1. HTTP/1.1 Server

- When a user sends multiple requests to an HTTP/1.1 server, the browser typically opens separate connections for each request.
- These connections are pipelined, meaning the server processes requests one after the other.
- Each request has its own connection, and they are sent independently.

#### 2. Connection Pooling

- In database communication, connection pooling is a technique where multiple connections are pre-established and kept ready.
- When a request comes in, a free connection from the pool is picked to handle the request.
- This approach optimizes resource utilization and enhances response times.

#### 3. Browser Connection Pooling

- Browsers, especially in HTTP/1.1, have limitations on the number of connections they can establish (typically six).
- When these connections are occupied, further requests are blocked until a connection becomes available.
- This limitation leads to potential bottlenecks and delayed responses.

### Advantages

- Efficient utilization of resources.
- Simultaneous handling of multiple tasks over a single connection.
- Improved response times in certain scenarios.

### Limitations

- Can lead to resource contention, especially when there's a high volume of requests.
- May result in blocking when connection limits are reached.

## Demultiplexing

### Definition

Demultiplexing is the process of extracting multiple signals or data streams from a single line or channel. It involves distributing the combined data into their respective destinations.

### Examples

#### 1. HTTP/2

- In contrast to HTTP/1.1, HTTP/2 supports multiplexing and allows multiple streams (requests and responses) to be sent concurrently over a single connection.
- Each stream is assigned a unique identifier, enabling demultiplexing on the receiving end.

#### 2. Connection Pooling (Response)

- In a connection pool scenario, once a response is received, the connection is freed up to handle additional requests.
- This demultiplexing of responses ensures that each request receives the appropriate response.

### Advantages

- Enhanced parallel processing of data streams.
- Efficient utilization of available connections.
- Reduced contention for resources.

### Limitations

- Complexity may increase, especially in scenarios where precise ordering of responses is crucial.
- Requires effective handling of identifiers or tags to route data to the correct destination.

## Conclusion

Understanding the dynamics of multiplexing and demultiplexing is essential for designing efficient communication systems. Whether optimizing resource usage through multiplexing or ensuring orderly distribution with demultiplexing, these concepts play a pivotal role in networking and protocol design.

In the upcoming sections, we'll explore practical examples and delve deeper into specific protocols and technologies that leverage these concepts. Stay tuned for a hands-on exploration of multiplexing and demultiplexing in real-world scenarios.

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='stateless-stateful'>

---


# 9] Stateless vs. Stateful Architectures: A Deep Dive

### Introduction:
- The lecture explores the debate between stateful and stateless architectures in engineering.
- Emphasizes the practical implications over strict definitions.
- Encourages a philosophical understanding of why engineers choose certain approaches.

### Stateless vs. Stateful Definition:
- **Stateless:** No state stored in the server or client; each request contains all necessary information.
  - Ideal for quick restarts, resilience, and simplicity.
- **Stateful:** Server stores client information and relies on it for proper functioning.
  - Prone to issues if the stored state is lost.

### Stateful Backends:
- **Characteristics:**
  - Stores client state in memory.
  - Depends on this information for proper functionality.

- **Example:**
  - Login application storing session locally.
  - Risk: If the backend restarts, the stored session is lost, leading to user authentication issues.

### Stateless Backends:
- **Characteristics:**
  - No reliance on stored state in the server.
  - Client responsible for transferring state with each request.

- **Example:**
  - Logging application storing data in a database.
  - Can restart during idle time, and client workflows remain uninterrupted.

### Testing Statelessness:
- **Criteria:**
  - Can the backend restart during idle time?
  - Can clients finish their workflow seamlessly after a backend restart?

### Stateful Systems vs. Stateless Backends:
- **System Example:**
  - Stateful system: Stores data in a centralized database.
  - Stateless backend: Serves requests without relying on local stored state.

### Protocol Considerations:
- **TCP:** Stateful protocol with connection, sequences, and maintained information.
- **UDP:** Stateless protocol, message-based with no connection setup.
- **HTTP over TCP:** Stateless requests with cookies to maintain state.
- **Quick over UDP:** Stateful protocol built on top of stateless UDP.

### Challenges and Solutions:
- **Stateless Challenges:**
  - Session loss during backend restarts.
  - Sticky sessions might be required in load balancing.

- **Stateful Solutions:**
  - Store sessions persistently in a database.
  - Ensure backend restarts don't disrupt user workflows.

### Examples of Stateless Systems:
- Systems where the input contains all necessary information (e.g., checking if a number is prime).
- JSON Web Tokens (JWTs): Entire state in the token itself, making it stateless.

### Considerations for JWT:
- Stateless nature: All information is within the token.
- Security concerns: Stolen tokens pose a risk.
- Refresh tokens: Mitigate issues by keeping access tokens short and refresh tokens longer.

### Final Thoughts:
- Acknowledges that not everything in backend engineering is perfected.
- Constantly evolving landscape with ongoing challenges.
- Encourages understanding current limitations and working around them.

### Conclusion:
- A comprehensive overview of stateful vs. stateless architectures.
- Pragmatic approach over strict definitions.
- The importance of adapting to current knowledge while being aware of potential flaws in the system.

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='sidecar-pattern'>

---

# 10] Understanding Sidecar Pattern

## Challenges with Protocols:

- Using different protocols requires specific libraries.
- Libraries become complex, especially with sophisticated protocols.
- Upgrading libraries can be challenging, leading to thick clients and backends.

## Introduction to Sidecar Pattern:

- Eliminates the need for clients to directly handle protocol intricacies.
- Clients use a thin library that communicates with a sidecar proxy.
- The sidecar proxy is responsible for protocol translation, upgrading, and communication with the backend.

## Examples:

**HTTP Library Example:**

- If using an HTTP library in Python, it must understand how to communicate using HTTP/1.1.
- Sidecar Pattern allows the proxy to handle the communication intricacies.

**gRPC Example:**

- gRPC requires a client and server library.
- Sidecar Pattern allows the client to remain oblivious to protocol details; the proxy handles the gRPC specifics.

## How Sidecar Pattern Works:

- Each client must have a sidecar proxy.
- The client configures requests to go through the sidecar proxy.
- The proxy intercepts the request, understands the destination, and communicates with the server-side proxy.
- The server-side proxy communicates with the actual server.

## Benefits:

**Language Agnostic:**

- Clients in different languages can communicate seamlessly since the sidecar proxy abstracts protocol complexities.

**Protocol Upgrade:**

- Upgrading protocols becomes easier. Only the sidecar proxy needs updating.

**Security:**

- Enables secure communication without clients needing to handle encryption details directly.

**Tracing and Monitoring:**

- Sidecar proxies facilitate tracing and monitoring by tagging requests and providing a centralized control plane.

**Service Discovery:**

- Simplifies service discovery through a centralized DNS system.

**Caching:**

- Allows for caching at the sidecar level, improving performance.

## Drawbacks:

- Complexity:
  - The system becomes complex due to the introduction of sidecar proxies, making debugging and understanding challenging.

- Latency:
  - Adds latency with the introduction of two additional hops for each communication.

## Examples of Sidecar Proxies:

**Service Mesh Proxies:**

- Examples include Linkerd and Istio, which are based on the sidecar pattern.

**Envoy Proxy:**

- A layer 7 proxy used as a sidecar container. Requires application-level understanding.

## Conclusion:

The Sidecar Pattern is a powerful solution for managing protocol complexities in microservices. While it introduces some complexity and latency, its benefits in terms of language agnosticism, protocol upgrades, and enhanced monitoring make it a valuable architectural choice, especially in the context of service meshes.

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='intro-to-protocols'>

---

# 11] Introduction to Protocols

In this section, we delve into the fascinating realm of protocols, exploring their properties, the considerations when designing one, and an overview of popular protocols.

## Designing Your Own Protocol:

- When designing a protocol, consider the purpose: What problem are you solving? Each protocol is crafted to address specific needs.
- Key consideration: Performance and usability for the intended use case.

## Popular Protocols Covered:

- The section focuses on fundamental and widely used protocols:
  - TCP
  - UDP
  - HTTP
  - gRPC
  - HDB (HDB1, HDB2, HDB3)
  - WebSockets
  - WebRTC
  - Apache FTP
  - SMTP

## Protocol Properties:

### 1. Data Format:

- Crucial aspect: How data is formatted during communication.
- Two types:
  - **Text-based:** Readable if unencrypted. Examples: JSON, XML, plaintext HTTP.
  - **Binary:** Efficient for machine reading. Examples: Protocol Buffers, MessagePack.

### 2. Transfer Mode:

- Determines how data is transferred.
- **Message-based:** Each message has a start and end (e.g., HTTP, UDP).
- **Stream:** Continuous flow of bytes without distinct start or end (e.g., TCP).

### 3. Addressing System:

- Specifies where the data is going.
- Examples: DNS, IP addresses, MAC addresses (for layer 2 protocols).
- Destination and source addressing crucial for routing.

### 4. Directionality:

- Defines if the protocol is bidirectional, unidirectional, full duplex, or half duplex.
- Example: Wi-Fi operates in half duplex, while many wired connections are full duplex.

### 5. State:

- Defines if the protocol is stateful or stateless.
- **Stateful:** Remembers previous interactions (e.g., gRPC, TCP).
- **Stateless:** Each interaction is independent (e.g., UDP).

### 6. Routing:

- Considers how the protocol interacts with gateways and proxies.
- Addresses immediate and final destinations, especially in the presence of proxies.

### 7. Flow and Congestion Control:

- Deals with managing data flow, congestion, and reliability.
- Example: TCP has flow and congestion control, while UDP does not.

### 8. Error Management:

- Includes handling error codes, timeouts, and retries.
- Example: HTTP has specific error messages and defines actions in case of timeouts or errors.

## Conclusion:

Understanding these properties provides a foundation for comprehending the nuances of protocols. As we explore each protocol in-depth, keep in mind how these properties influence their design and usage. Stay tuned for the upcoming sections, where we'll dive into the specifics of each protocol. Happy learning!

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
<a id='osi-model'>

---

# 12] Understanding the OSI Model

### Introduction:
- The OSI model (Open Systems Interconnection) is crucial for anyone dealing with networking and software engineering.
- The speaker reflects on a past experience where the OSI model seemed unimportant, emphasizing the importance of understanding it.

### Key Takeaways:
1. **Importance of Communication Model:**
   - Building agnostic applications requires a standard for communication.
   - Without a standard, applications would need to be tailored for each network medium, leading to chaos.

2. **Network Equipment Management:**
   - A standard model allows for easier upgrading of network equipment.
   - Decoupling innovation: Each layer can be improved independently without affecting others.

3. **The Seven Layers of the OSI Model:**
   - **Layer 1 (Physical):** Deals with the bare metal and the physical transmission medium (e.g., electric signals, light, radio waves).
   - **Layer 2 (Data Link):** Concerned with MAC addresses and frames within frames. Involves Ethernet and Wi-Fi.
   - **Layer 3 (Network):** Involves IP addresses and routing. Decides how to route packets between devices.
   - **Layer 4 (Transport):** Focuses on segments (TCP) and datagrams (UDP). Protocols like TCP and UDP reside here.
   - **Layer 5 (Session):** Manages connection establishment, TLS, and states in communication.
   - **Layer 6 (Presentation):** Deals with encoding and serialization of data. Converts application data to byte strings.
   - **Layer 7 (Application):** The topmost layer, where actual applications reside. Deals with user interfaces and application-level protocols.

4. **Example: Sending a POST Request:**
   - Describes the process of sending a POST request from an application through the OSI layers.
   - Each layer, from physical to application, plays a role in transmitting and receiving the data.

### Practical Implications:
- As a software engineer, understanding the OSI model helps determine where your application fits and how it interacts with the network.
- Networking components like CDNs, reverse proxies, and load balancers reside in specific layers, impacting how they operate.

### Key Concepts:
- **Decoupling:** The ability to upgrade network equipment without worrying about the underlying medium.
- **Protocol Ossification:** The challenge of changing protocols due to existing routers' expectations.

### Conclusion:
- Understanding the OSI model is essential for anyone interacting with networking.
- It provides a standardized framework for communication, allowing for interoperability and innovation in each layer.

*Note: Consider adding practical examples and code snippets for further clarification, especially in the context of encoding, serialization, and protocol handling.*

> [Siddhant](https://siddhantxh.vercel.app) is learning markdown, it seems pretty cool ngl frfr no cap imho real [⬆️Back to top](#table-of-contents)
---

