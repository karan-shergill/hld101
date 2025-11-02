# Systems that require real-time updates

Real-time Updates address the challenge of delivering immediate notifications and data changes from servers to clients as events occur. From chat applications where messages need instant delivery to live dashboards showing real-time metrics, users expect to be notified the moment something happens.

- [The Problem](#the-problem)
- [The Solution](#the-solution)
- [Client-Server Connection Protocols (First "hop")](#client-server-connection-protocols-first-hop)
    - [Simple Polling: The Baseline](#simple-polling-the-baseline)
        - [Advantages](#advantages)
        - [Disadvantages](#disadvantages)
        - [When to use simple polling](#when-to-use-simple-polling)
        - [Things to Discuss in Your Interview](#things-to-discuss-in-your-interview)
    - [Long Polling: The Easy Solution](#long-polling-the-easy-solution)
        - [Advantages](#advantages)
        - [Disadvantages](#disadvantages)
        - [When to Use Long Polling](#when-to-use-long-polling)
        - [Things to Discuss in Your Interview](#things-to-discuss-in-your-interview)
    - [Server-Sent Events (SSE): The Efficient One-Way Street](#server-sent-events-sse-the-efficient-one-way-street)
        - [How SSE Works](#how-sse-works)
        - [Advantages](#advantages)
        - [Disadvantages](#disadvantages)
        - [When to Use SSE](#when-to-use-sse)
        - [Things to Discuss in Your Interview](#things-to-discuss-in-your-interview)
    - [Websockets: The Full-Duplex Champion](#websockets-the-full-duplex-champion)
        - [How WebSockets Works](#how-websockets-works)
        - [Advantages](#advantages)
        - [Disadvantages](#disadvantages)
        - [When to Use WebSockets](#when-to-use-websockets)
        - [Extra Challenges](#extra-challenges)
        - [Things to Discuss in Your Interview](#things-to-discuss-in-your-interview)
    - [WebRTC: The Peer-to-Peer Solution](#webrtc-the-peer-to-peer-solution)
        - [How WebRTC Works](#how-webrtc-works)
        - [Advantages](#advantages)
        - [Disadvantages](#disadvantages)
        - [When to Use WebRTC](#when-to-use-webrtc)
        - [Things to Discuss in Your Interview](#things-to-discuss-in-your-interview)
- [Which one to use when?](#which-one-to-use-when)
    - [Comparison Table](#comparison-table)
    - [When to Choose Which](#when-to-choose-which)
    - [Scaling Perspective](#scaling-perspective)
    - [Extras: **WebHooks**](#extras-webhooks)
- [Server-Side Push/Pull (Second "hop")](#server-side-pushpull-second-hop)
    - [Pulling with Simple Polling](#pulling-with-simple-polling)
        - [Advantages](#advantages)
        - [Disadvantages](#disadvantages)
        - [When to Use Pull-Based Polling](#when-to-use-pull-based-polling)
        - [Things to Discuss in Your Interview](#things-to-discuss-in-your-interview)
    - [Pushing via Consistent Hashes](#pushing-via-consistent-hashes)
        - [Simple Hashing](#simple-hashing)
        - [Consistent Hashing](#consistent-hashing)
    - [Pushing via Pub/Sub](#pushing-via-pubsub)
        - [Advantages](#advantages)
        - [Disadvantages](#disadvantages)
        - [When to Use Pub/Sub](#when-to-use-pubsub)
        - [Things to Discuss in Your Interview](#things-to-discuss-in-your-interview)
- [When to Use in Interviews](#when-to-use-in-interviews)
    - [Common Interview Scenarios](#common-interview-scenarios)
    - [When NOT to Use](#when-not-to-use)
    - [Comparison Table](#comparison-table)
    - [When to Choose](#when-to-choose)
- [Common Deep Dives](#common-deep-dives)
    - [How do you handle connection failures and reconnection?](#how-do-you-handle-connection-failures-and-reconnection)
    - [What happens when a single user has millions of followers who all need the same update?](#what-happens-when-a-single-user-has-millions-of-followers-who-all-need-the-same-update)
    - [How do you maintain message ordering across distributed servers?](#how-do-you-maintain-message-ordering-across-distributed-servers)
- [Conclusion](#conclusion)
- [Suggestion for Interview Strength](#suggestion-for-interview-strength)

# The Problem

Consider a collaborative document editor like Google Docs. When one user types a character, all other users viewing the document need to see that change within milliseconds. In apps like this you can't have every user constantly polling the server for updates every few milliseconds without crushing your infrastructure.
The core challenge is establishing efficient, persistent communication channels between clients and servers. Standard HTTP follows a request-response model: clients ask for data, servers respond, then the connection closes. This works great for traditional web browsing but breaks down when you need servers to proactively push updates to clients.

Problem with Real-time Updates Pattern

1. Design BookMyShow
2. Design Uber
3. Design WhatsApp
4. Design Stock Brokrage App
5. Design Google Docs
6. Design Strava/Sports Tracker
7. Design Online Auction
8. Design FB Live Comments

# The Solution

When systems require real-time updates, push notifications, etc, the solution requires two distinct pieces:

1. The first "hop": how do we get updates from the server to the client?
2. The second "hop": how do we get updates from the source to the server?

![real_time_updates_1_step_1](/notes/images/real_time_updates_1_step_1.png)

# Client-Server Connection Protocols (First "hop")

The first "hop" is establishing efficient communication channels between clients and servers. While traditional HTTP request-response works for a startling number of use-cases, real-time systems frequently need persistent connections or clever polling strategies to enable servers to **push** updates to clients.

## Simple Polling: The Baseline

The simplest possible approach is to have the client regularly poll the server for updates. This could be done with a simple HTTP request that the client makes at a regular interval. This doesn't technically qualify as real-time, but it's a good starting point and provides a good contrast for our other methods.

![real_time_updates_1.1_short_polling](/notes/images/real_time_updates_1.1_short_polling.png)

How does it work? It's dead simple! The client makes a request to the server at a regular interval and the server responds with the current state of the world. In our chat app, we would just constantly be polling for "what messages have I not received yet?".

```jsx
async function poll() {
  const response = await fetch('/api/updates');
  const data = await response.json();
  processData(data);
}

// Poll every 2 seconds
setInterval(poll, 2000);
```

### Advantages

- Simple to implement.
- Stateless.
- No special infrastructure needed.
- Works with any standard networking infrastructure.
- Doesn't take much time to explain.

### Disadvantages

- Higher latency than other methods. Updates might be delayed as long as the polling interval + the time it takes to process the request.
- Limited update frequency.
- More bandwidth usage.
- Can be resource-intensive with many clients, establishing new connections, etc.

### When to use simple polling

Simple polling is a great baseline and, absent a problem which specifically requires very-low latency, real-time updates, it's a great solution. It's also appropriate when the window where you need updates is short,

### Things to Discuss in Your Interview

You'll want to be clear with your interviewer about the trade-offs you're making with polling vs other methods. A good explanation highlights the simplicity of the approach and gives yourself a backdoor if you discover that you need something more sophisticated. "I'm going to start with a simple polling approach so I can focus on X, but we can switch to something more sophisticated if we need to later."

## Long Polling: The Easy Solution

See [**Long Polling is Underrated - Here's Why It's Brilliant**](https://www.youtube.com/watch?v=zcSVyGHJsJ4)

See [**Short Polling vs Long Polling**](https://www.youtube.com/watch?v=jTU6bUxLGBA)

After a baseline for simple polling, long polling is the easiest approach to achieving near real-time updates. It builds on standard HTTP, making it easy to implement and scale.

The idea is also simple: the client makes a request to the server and the server holds the request open until new data is available. It's as if the server is just taking really long to process the request. The server then responds with the data, finalizes the HTTP requests, and the client immediately makes a **new** HTTP request. This repeats until the server has new data to send. If no data has come through in a long while, we might even return an empty response to the client so that they can make another request.

![real_time_updates_2.1_long_polling](/notes/images/real_time_updates_2.1_long_polling.png)

For our chat app, we would keep making a request to get the *next message*. If there was no message to retrieve, the server would just hold the request open until a new message was sent before responding to us. After we received that message, we'd make a new request for the next message.

1. Client makes HTTP request to server
2. Server holds request open until new data is available
3. Server responds with data
4. Client immediately makes new request
5. Process repeats

```jsx
// Client-side of long polling
async function longPoll() {
  while (true) {
    try {
      const response = await fetch('/api/updates');
      const data = await response.json();
      
      // Handle data
      processData(data);
    } catch (error) {
      // Handle error
      console.error(error);
      
      // Add small delay before retrying on error
      await new Promise(resolve => setTimeout(resolve, 1000));
    }
  }
}
```

The simplicity of the approach hides an important trade-off for high-frequency updates. Since the client needs to "call back" to the server after each receipt, the approach can introduce some extra latency:

![real_time_updates_2.2_long_polling](/notes/images/real_time_updates_2.2_long_polling.png)

### Advantages

- Builds on standard HTTP and works everywhere HTTP works.
- Easy to implement.
- No special infrastructure needed.
- Stateless server-side.

### Disadvantages

- Higher latency than alternatives.
- More HTTP overhead.
- Can be resource-intensive with many clients.
- Not suitable for frequent updates due to the issues mentioned above.
- Makes monitoring more painful since requests can hang around for a long time.
- Browsers limit the number of concurrent connections per domain, meaning you may only be able to have a few long-polling connections per domain.

### When to Use Long Polling

Long polling is a great solution for near-real-time updates with a simple implementation. It's a good choice when updates are infrequent and a simple solution is preferred. If the latency trade-off of a simple polling solution is at all an issue, long-polling is an obvious upgrade with minimal additional complexity.

Long Polling is a great solution for applications where a long async process is running but you want to know when it finishes, as soon as it finishes - like is often the case in payment processing. We'll long-poll for the payment status before showing the user a success page.

### Things to Discuss in Your Interview

Because long-polling utilizes the existing HTTP infrastructure, there's not a bunch of extra infrastructure you're going to need to talk through. Even though the polling is "long", you still do need to be specific about the polling frequency. Keep in mind that each hop in your infrastructure needs to be aware of these lengthy requests: you don't want your load balancer hanging up on the client after 30 seconds when your long-polling server is happy to keep the connection open for 60 (15-30s is a pretty common polling interval that minimizes the fuss here).

## Server-Sent Events (SSE): The Efficient One-Way Street

See [**Networking Essentials for System Design Interviews**](https://youtu.be/SHkbPm1Wrno?t=1603)

See [**Server-sent events are pretty cool**](https://youtu.be/_XQiU3mLNk0)

See [**Server-Sent Events Crash Course**](https://youtu.be/4HlNv1qpZFY)

SSE is an extension on long-polling that allows the server to send a stream of data to the client.

Normally HTTP responses have a header like Content-Length which tells the client how much data to expect. SSE instead uses a special header Transfer-Encoding: chunked which tells the client that the response is a series of chunks - we don't know how many there are or how big they are until we send them. This allows us to move from a single, atomic request/response to a more granular "stream" of data.

With SSE, instead of sending a full response once data becomes available, the server sends a chunk of data and then keeps the request open to send more data as needed. *SSE is perfect for scenarios where servers need to push data to clients, but clients don't need to send data back frequently.*

![real_time_updates_2.3_sse](/notes/images/real_time_updates_2.3_sse.png)

In our chat app, we would open up a request to stream messages and then each new message would be sent as a chunk to the client.

### How SSE Works

1. Client establishes SSE connection
2. Server keeps connection open
3. Server sends messages when data changes or updates happen
4. Client receives updates in real-time

Modern browsers have built-in support for SSE through the EventSource object, making the client-side implementation straightforward.

```jsx
// Client-side
const eventSource = new EventSource('/api/updates');

eventSource.onmessage = (event) => {
  const data = JSON.parse(event.data);
  updateUI(data);
};

// Server-side (Node.js/Express example)
app.get('/api/updates', (req, res) => {
  res.setHeader('Content-Type', 'text/event-stream');
  res.setHeader('Cache-Control', 'no-cache');
  res.setHeader('Connection', 'keep-alive');

  const sendUpdate = (data) => {
    res.write(`data: ${JSON.stringify(data)}\n\n`);
  };

  // Send updates when data changes
  dataSource.on('update', sendUpdate);

  // Clean up on client disconnect
  req.on('close', () => {
    dataSource.off('update', sendUpdate);
  });
});
```

### Advantages

- Built into browsers.
- Automatic reconnection.
- Works over HTTP.
- More efficient than long polling due to less connection initiation/teardown.
- Simple to implement.

### Disadvantages

- One-way communication only.
- Limited browser support (not an issue for modern browsers).
- Some proxies and networking equipment don't support streaming. Nasty to debug!
- Browsers limit the number of concurrent connections per domain, meaning you may only be able to have a few SSE connections per domain.
- Makes monitoring more painful since requests can hang around for a long time.

### When to Use SSE

SSE is a great upgrade to long-polling because it eliminates the issues around high-frequency updates while still building on top of standard HTTP. That said, it comes with lesser overall support because you'll need both browsers and/all infrastructure between the client and server to support streaming responses.

A very popular use case for SSE today is AI chat apps, which frequently involve the need to stream new tokens (words) to the user as they are generated to keep the UI responsive.

An example of an infra gap is that many proxies and load balancers don't support streaming responses. In these cases, the proxy will try to buffer the response until it completes, which effectively blocks our stream in an annoying, opaque way that is hard to debug! [**2**](https://www.hellointerview.com/learn/system-design/patterns/realtime-updates#user-content-fn-sse)

As an aside: most interviewers will not be familiar with the [**infrastructure considerations associated with SSE**](https://dev.to/miketalbot/server-sent-events-are-still-not-production-ready-after-a-decade-a-lesson-for-me-a-warning-for-you-2gie) and aren't going to ask you detailed questions about them. But if the role you're interviewing for is very frontend-centric, be prepared in case they expect you to know your stuff!

### Things to Discuss in Your Interview

SSE rides on existing HTTP infrastructure, so there's not a lot of extra infrastructure you'll need to talk through. You also don't have a polling interval to negotiate or tune.

Most SSE connections won't be super-long-lived (e.g. 30-60s is pretty typical), so if you need to send messages for a longer period you'll need to talk about how clients re-establish connections and how they deal with the gaps in between. The [**SSE standard**](https://html.spec.whatwg.org/multipage/server-sent-events.html) includes a "last event ID" which is intended to cover this gap, and the EventSource object in browsers explicitly handles this reconnection logic. If a client loses its connection, it can reconnect and provide the last event ID it received. The server can then use that ID to send all the events that occurred while the client was disconnected.

## Websockets: The Full-Duplex Champion

See [**What is WebSocket? Why is it used & how is it different from HTTP?**](https://youtu.be/favi7avxIag)

See [**Networking Essentials for System Design Interviews**](https://youtu.be/SHkbPm1Wrno?t=1834)

See [**We need to talk about WebSockets**](https://youtu.be/ylpn4QAOogE)

See [**How to scale WebSockets to millions of connections**](https://youtu.be/vXJsJ52vwAA)

See [**How to horizontally scale up websocket servers**](https://youtu.be/hl3_MANBiyc)

See [**WebSockets Aren’t as Reliable as You Think.. Here's Why**](https://youtu.be/ImzYxO3Lsvc)

WebSockets are the go-to choice for true bi-directional communication between client and server. If you have high frequency writes *and* reads, WebSockets are the champ.

### How WebSockets Works

Websockets build on HTTP through an "upgrade" protocol, which allows an existing TCP connection to change L7 protocols. This is super convenient because it means you can utilize some of the existing HTTP session information (e.g. cookies, headers, etc.) to your advantage.

Once a connection is established, both client and server can send "messages" to each other which are effectively opaque binary blobs. You can shove strings, JSON, Protobufs, or anything else in there. Think of WebSockets like a TCP connection with some niceties that make establishing the connection easier, especially for browsers.

1. Client initiates WebSocket handshake over HTTP
2. Connection upgrades to WebSocket protocol
3. Both client and server can send messages
4. Connection stays open until explicitly closed

![real_time_updates_3.1_websockets](/notes/images/real_time_updates_3.1_websockets.png)

For our chat app, we'd connect to a WebSocket endpoint over HTTP, sharing our authentication token via cookies. The connection would get upgraded to a WebSocket connection and then we'd be able to receive messages back to the client over the connection as they happen. Bonus: we'd also be able to send messages to other users in the chat room!

```jsx
// Client-side
const ws = new WebSocket('ws://api.example.com/socket');

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  handleUpdate(data);
};

ws.onclose = () => {
  // Implement reconnection logic
  setTimeout(connectWebSocket, 1000);
};

// Server-side (Node.js/ws example)
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
  ws.on('message', (message) => {
    // Handle incoming messages
    const data = JSON.parse(message);
    processMessage(data);
  });

  // Send updates to client
  dataSource.on('update', (data) => {
    ws.send(JSON.stringify(data));
  });
});
```

### Advantages

- Full-duplex (read and write) communication.
- Lower latency than HTTP due to reduced overhead (e.g. no headers).
- Efficient for frequent messages.
- Wide browser support.

### Disadvantages

- More complex to implement.
- Requires special infrastructure.
- Stateful connections, can make load balancing and scaling more complex.
- Need to handle reconnection.

### When to Use WebSockets

Generally speaking, if you need **high-frequency**, bi-directional communication, you're going to want to use WebSocket. I'm emphasizing high-frequency here because you can always make additional requests/connections for writes: a very common pattern is to have SSE subscriptions for updates and do writes over simple HTTP POST/PUT whenever they occur.

In interviews, we often too eagerly adopting Websockets when they could be using SSE or simple polling instead. Because of the additional complexity and infra lift, you'll want to defer to SSE unless you have a specific need for this bi-directional communication.

### Extra Challenges

Because the Websocket is a *persistent connection*, we need our infrastructure to support it. Some L7 load balancers support websockets, but support is generally spotty (remember that L7 load balancers aren't guaranteeing we're using the same TCP connection for each incoming request). L4 load balancers will support websockets natively since the same TCP connection is used for each request.

When we have long-running connections we have another problem: deployments. When servers are redeployed, we either need to sever all old connections and have them reconnect or have the new servers take over and keep the connections alive. Generally speaking you should prefer the former since it's simpler, but it does have some ramifications on how "persistent" you expect the connection to be. You also need to be able to handle situations where a client needs to reconnect and may have missed updates while they were disconnected.

Finally, balancing load across websocket servers can be more complex. If the connections are truly long-running, we have to "stick with" each allocation decision we made. If we have a load balancer that wants to send a new request to a different server, it can't do that if it would break an existing websocket connection.

Because of all the issues associated with *stateful* connections, many architectures will terminate WebSockets into a WebSocket service early in their infrastructure. This service can then handle the connection management and scaling concerns and allows the rest of the system to remain *stateless*. The WebSocket service is also less likely to change meaning it requires less deployments which churn connections.

![real_time_updates_3.2_websockets](/notes/images/real_time_updates_3.2_websockets.png)

### Things to Discuss in Your Interview

Websockets are a powerful tool, but they do come with a lot of complexity. You'll want to talk through how you'll manage connections and deal with reconnections. You'll also need to consider how your deployment strategy will handle server restarts.

Managing *statefulness* is a big part of the conversation. Senior/staff candidates will frequently talk about how to minimize the spread of state across their architecture.

There's also a lot to discuss about how to scale WebSocket servers. Load can be uneven which can result in hotspots and failures. Using a "least connections" strategy for the load balancer can help, as well as minimizing the amount of work the WebSocket servers need to do as they process messages. Using the reference architecture above and offloading more intensive processing to other services (which can scale independently) can help.

## WebRTC: The Peer-to-Peer Solution

See [**WebRTC Tutorial - How does WebRTC work?**](https://youtu.be/pGAp5rxv6II?list=PLinedj3B30sDxXVu4VXdFx678W2pJmORa)

See [**Want to make a video chat app? Watch this video for WebRTC!**](https://youtu.be/g42yNO_dxWQ)

Read about **Mesh, SFU, MCU, or P2P [[link1](https://www.digitalsamba.com/blog/p2p-sfu-and-mcu-webrtc-architectures-explained)] [[link2](https://medium.com/@toshvelaga/webrtc-architectures-mesh-mcu-and-sfu-12c502274d7)] [[link3](https://getstream.io/blog/what-is-a-selective-forwarding-unit-in-webrtc/)]**

Our last option is the most unique. WebRTC enables direct peer-to-peer communication between browsers, perfect for video/audio calls and some data sharing like document editors.

Clients talk to a central "signaling server" which keeps track of which peers are available together with their connection information. Once a client has the connection information for another peer, they can try to establish a direct connection without going through any intermediary servers.

In practice, most clients don't allow inbound connections for security reasons (the exception would be servers which broadcast their availability on specific ports at specific addresses) using devices like NAT (network address translation). So if we stopped there, most peers wouldn't be able to "speak" to each other.

The WebRTC standard includes two methods to work around these restrictions:

- **STUN**: "Session Traversal Utilities for NAT" is a protocol and a set of techniques like "hole punching" which allows peers to establish publically routable addresses and ports. I won't go into details here, but as hacky as it sounds it's a standard way to deal with NAT traversal and it involves repeatedly creating open ports and sharing them via the signaling server with peers.
- **TURN**: "Traversal Using Relays around NAT" is effectively a relay service, a way to bounce requests through a central server which can then be routed to the appropriate peer.

![real_time_updates_4_webrtc](/notes/images/real_time_updates_4_webrtc.png)

In practice, the signaling server is relatively lightweight and isn't handling much of the bandwidth as the bulk of the traffic is handled by the peer-to-peer connections. But interestingly the signaling server does effectively act as a real-time update system for its clients (so they can find their peers and update their connection info) so it either needs to utilize WebSockets, SSE, or some other approach detailed above.
For our chat app, we'd connect to our signaling server over a WebSocket connection to find all of our peers (others in the chat room). Once we'd identified them and exchanged connection information, we'd be able to establish direct peer-to-peer connections with them. Chat messages would be broadcast by room participants to all of their peers (or, if you want to be extra fancy, bounced between participants until they settle).

### How WebRTC Works

Ok, but how does it work?

1. Peers discover each other through signaling server.
2. Exchange connection info (ICE candidates)
3. Establish direct peer connection, using STUN/TURN if needed
4. Stream audio/video or send data directly

Pretty simple, apart from the acronyms and NAT traversal.

```jsx
// Simplified WebRTC setup
async function startCall() {
  const pc = new RTCPeerConnection();
  
  // Get local stream
  const stream = await navigator.mediaDevices.getUserMedia({
    video: true,
    audio: true
  });
  
  // Add tracks to peer connection
  stream.getTracks().forEach(track => {
    pc.addTrack(track, stream);
  });
  
  // Create and send offer
  const offer = await pc.createOffer();
  await pc.setLocalDescription(offer);
  
  // Send offer to signaling server
  signalingServer.send(offer);
}
```

### Advantages

- Direct peer communication
- Lower latency
- Reduced server costs
- Native audio/video support

### Disadvantages

- Complex setup (> WebSockets)
- Requires signaling server
- NAT/firewall issues
- Connection setup delay

### When to Use WebRTC

WebRTC is the most complex and heavyweight of the options we've discussed. It's overkill for most real-time update use cases, but it's a great tool for scenarios like video/audio calls, screen sharing, and gaming.

The notable exception is that it can be used to reduce server load. If you have a system where clients need to talk to each other frequently, you could use WebRTC to reduce the load on *your* servers by having clients establish their own connections. [**Canva took this approach with presence/pointer sharing in their canvas editor**](https://www.canva.dev/blog/engineering/realtime-mouse-pointers/) and it's a popular approach from collaborative document editing like [**Google Docs**](https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs) when used in conjunction with CRDTs which are better suited for a peer-to-peer architecture.

### Things to Discuss in Your Interview

If you're building a WebRTC app in a system design interview, it should be really obvious why you're using it. Either you're trying to do video conferencing or the scale strictly requires you to have clients talk to each other directly.

Having some knowledge of the infra requirements (STUN/TURN, signaling servers, etc.) will give you the flexibility to make the best design decision for your system. You'll also want to speak pretty extensively about the communication patterns between peer clients and any eventual synchronization to a central server (almost all design questions will have some calling home to the mothership to store data or report results).

# Which one to use when?

- **Simple Polling** → “Hey, got something new?” every few seconds. (Inefficient but easy)
- **Long Polling** → “Tell me when you’ve got something new.” (Better; older real-time trick)
- **SSE** → “Server keeps sending updates whenever something changes.” (Best for one-way updates)
- **WebSocket** → “Let’s keep a live conversation going both ways.” (Ideal for real-time apps)
- **WebRTC** → “Let’s talk directly without the server in between.” (Perfect for calls and media)

There are a lot of options for delivering events from the server to the client. Being familiar with the trade-offs associated with each will give you the flexibility to make the best design decision for your system. If you're in a hurry, the following flowchart will help you choose the right tool for the job.

- If you're not latency sensitive, **simple polling** is a great baseline. You should probably start here unless you have a specific need in your system.
- If you don't need bi-directional communication, **SSE** is a great choice. It's lightweight and works well for many use cases. There are some infrastructure considerations to keep in mind, but they're less invasive than with WebSocket and generally interviewers are less familiar with them or less critical if you don't address them.
- If you need frequent, bi-directional communication, **WebSocket** is the way to go. It's more complex, but the performance benefits are huge.
- Finally, if you need to do audio/video calls, **WebRTC** is the only way to go. In some instances peer-to-peer collaboration can be enhanced with WebRTC, but you're unlikely to see it in a system design interview.

![real_time_updates_5_which_one_to_use_when](/notes/images/real_time_updates_5_which_one_to_use_when.png)

### Comparison Table

| Feature / Aspect | **Simple Polling** | **Long Polling** | **Server-Sent Events (SSE)** | **WebSocket** | **WebRTC** |
| --- | --- | --- | --- | --- | --- |
| **Connection Type** | Multiple short HTTP requests | Single long-lived HTTP request | Single persistent HTTP connection (one-way) | Persistent full-duplex TCP (via HTTP upgrade) | Peer-to-peer connection |
| **Direction of Communication** | Client → Server (request only) | Mostly Client → Server (Server responds once) | Server → Client (unidirectional) | Bidirectional (Client ↔ Server) | Bidirectional (Peer ↔ Peer) |
| **Use Case Type** | Periodic data fetching | Real-time simulation (server push via delayed response) | Real-time server → client updates | Real-time two-way communication | Real-time peer-to-peer (audio/video/data) |
| **Typical Use Cases** | Checking for updates every few seconds (e.g., stock ticker) | Chat apps (older approach), notifications | Live scoreboards, dashboards, notifications | Chat, gaming, collaborative apps | Video/audio calls, file sharing, live streaming |
| **Connection Persistence** | None (new HTTP request every time) | One HTTP request stays open until server has data | Persistent (auto-reconnect supported) | Persistent full-duplex TCP socket | Persistent P2P over UDP (usually via STUN/TURN) |
| **Latency** | High | Medium-Low | Low | Very Low | Very Low |
| **Server Load** | High (many requests) | Moderate | Low | Low | Low (server only helps in setup) |
| **Scalability** | Poor (many requests per user) | Better but still costly for large scale | Good (built on HTTP/2 usually) | Good but requires WebSocket support | Complex (NAT traversal, signaling needed) |
| **Browser Support** | All | All | Most modern browsers (except old IE) | All modern browsers | All modern browsers |
| **Transport Protocol** | HTTP | HTTP | HTTP (text/event-stream) | TCP (after HTTP Upgrade) | UDP (via ICE/STUN/TURN) |
| **Message Type** | Request/Response | Request delayed until data | Streamed events (text-based) | Binary/Text frames | Media/Data channels |
| **Reconnection Handling** | Reconnects each time | Reconnects after response | Built-in auto-reconnect | Handled by client manually | Handled by signaling/STUN/TURN layers |

### When to Choose Which

| Scenario | Best Option | Reason |
| --- | --- | --- |
| **Low-frequency updates (every few seconds/minutes)** | ✅ *Simple Polling* | Simpler and sufficient if updates aren’t frequent. |
| **Real-time updates but server cannot push directly** | ✅ *Long Polling* | Works on all setups where WebSocket/SSE not supported. |
| **One-way live updates (server → client)** | ✅ *SSE* | Lightweight, automatic reconnect, ideal for notifications, dashboards, news feeds. |
| **Two-way interactive communication** | ✅ *WebSocket* | Full-duplex; ideal for chat, gaming, collaborative editing. |
| **Peer-to-peer media/data (audio, video, file sharing)** | ✅ *WebRTC* | Direct connection between clients; low latency, reduces server bandwidth. |

## Scaling Perspective

| Technology | Connection Type | Direction | Server Load | Horizontal Scalability | Infra Complexity | Ideal Use Case | Scalability Summary |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **Simple Polling** | Repeated short HTTP | Client → Server | 🔴 Very High (many requests) | ✅ Easy | 🟢 Low | Small apps, dashboards | ❌ Poor |
| **Long Polling** | Persistent HTTP (one at a time) | Server → Client | 🟠 Medium | ✅ Moderate | 🟡 Medium | Chat, notifications | ⚙️ Moderate |
| **SSE** | Persistent HTTP/1.1 stream | Server → Client | 🟢 Low | ✅ Good (with async infra) | 🟡 Medium | Stock feeds, dashboards | ✅ Good |
| **WebSocket** | Persistent TCP | Bi-directional | 🟢 Low | ⚙️ Complex (needs Pub/Sub) | 🔴 High | Chat, gaming, collab tools | 🚀 Excellent |
| **WebRTC** | Peer-to-Peer (TCP/UDP) | Bi-directional | 🟢 Low (server-side) | ⚙️ Hard (for many peers) | 🔴 High | Video calls, live media | ⚙️ Scales via SFU/MCU |

## Extras: **WebHooks**

See [**Webhooks explained again**](https://youtu.be/9zfAqoTm4-Q)

See [**Top 3 Things You Should Know About Webhooks!**](https://youtu.be/x_jjhcDrISk)

See [**This is why webhooks are important**](https://youtu.be/Dupp4PeXAlM)

# Server-Side Push/Pull (Second "hop")

Now that we've established our options for the hop from server to client (Simple Polling, Long-Polling, SSE, WebSockets, WebRTC), let's talk about how we can propagate updates from the source to the server.

![real_time_updates_6_step_2](/notes/images/real_time_updates_6_step_2.png)

Invariably, our system is somehow *producing* updates that we want to propagate to our clients. This could be other users making edits to a shared document, drivers making location updates, or friends sending messages to a shared chat.

Making sure these updates get to their ultimate destination is closely tied to how we propagate updates from the source to the server. Said differently, we need a **trigger**.

When it comes to triggering, there are three patterns that you'll commonly see:

1. Pulling via Polling
2. Pushing via Consistent Hashes
3. Pushing via Pub/Sub

## Pulling with Simple Polling

With Simple Polling, we're using a **pull-based** model. Our client is constantly asking the server for updates and the server needs to maintain the state necessary to respond to those requests. The most common way to do this is to have a database where we can store the updates (e.g. all of the messages in the chat room), and from this database our clients can pull the updates they need when they can. For our chat app, we'd basically be polling for "what messages have been sent to the room with a timestamp larger than the last message I received?".

![real_time_updates_7_pulling_with_simple_polling](/notes/images/real_time_updates_7_pulling_with_simple_polling.png)

Remember, with polling, we're tolerating delay! We use the poll itself as the trigger, even though the actual update may have occurred some time prior.

The nice thing about this from a system design perspective is that we've *decoupled* the source of the update from the client receiving it. The line that receives the updates is interrupted (by the DB) from the line that produces them - data is not *flowing* from the producer to the consumer. The downside is that we've lost the real-time aspect of our updates.

### Advantages

- Dead simple to implement.
- State is constrained to our DB.
- No special infrastructure.
- Doesn't take much time to explain.

### Disadvantages

- High latency.
- Excess DB load when updates are infrequent and polling is frequent.

### When to Use Pull-Based Polling

Pull-based polling is great when you want your user experience to be somewhat more responsive to changes that happen on the backend, but responding quickly is not the main thing. Generally speaking, if you need real-time updates this is not the best approach, but again there are a lot of use-cases where real-time updates are actually not required!

### Things to Discuss in Your Interview

When you're using Pull-based Polling, you'll want to talk about how you're storing the updates. If you're using a database, you'll want to discuss how you're querying for the updates and how that might change given your load.

In many instances where this approach might be used, the number of clients can actually be quite large. If you have a million clients polling every 10 seconds, you've got 100k TPS of read volume! This is easy to forget about.

## Pushing via Consistent Hashes

The remaining approaches involve **pushing** updates to the clients. In many of the client update mechanisms that we discussed above (long-polling, SSE, WebSockets) the client has a persistent connection to one server and that server is responsible for sending updates to the client.

But this creates a problem! For our chat application, in order to send a message to User C, we need to know which server they are connected to.

![real_time_updates_8_pushing_via_consistent_hashes](/notes/images/real_time_updates_8_pushing_via_consistent_hashes.png)

Ideally, when an a message needs to be sent, I would:

1. Figure out which server User C is connected to.
2. Send the message to that server.
3. That server will look up which (websocket, SSE, long-polling) request is associated with User C.
4. The server will then write the message via the appropriate connection.

There are two common ways to handle this situation, and the first is to use **hashing**. Let's build up our intuition for this in two steps.

### Simple Hashing

Our first approach might be to use a simple modulo operation to figure out which server is responsible for a given user. Then, we'll always have 1 server who "owns" the connections for that user.

To do this, we'll have a central service that knows how many servers there are N and can assign them each a number 0 through N-1. This is frequently Apache **ZooKeeper** or Etcd which allows us to manage this metadata and allows the servers to keep in sync as it updates, though in practice there are many alternatives.

We'll make the server number n responsible for user u % N. When clients initially connect to our service, we can either:
a. Directly connect them to the appropriate server (e.g. by having them know the hash, N, and the server addressess associated with each index).
b. Have them randomly connect to any of the servers and have that server redirect them to the appropriate server based on internal data.

![real_time_updates_9_simple_hashing](/notes/images/real_time_updates_9_simple_hashing.png)

When a client connects, the following happens:

1. The client connects to a random server.
2. The server looks up the client's hash in Zookeeper to figure out which server is responsible for them.
3. The server redirects the client to the appropriate server.
4. The client connects to the correct server.
5. The server adds that client to a map of connections.

Now we're ready to send updates and messages!

When we want to send messages to User C, we can simply hash the user's id to figure out which server is responsible for them and send the message there.

![real_time_updates_10_simple_hashing](/notes/images/real_time_updates_10_simple_hashing.png)

1. Our Update Server stays connected to Zookeeper and knows the addresses of all servers and the modulo N.
2. When the Update Server needs to send a message to User C, it can hash the user's id to figure out which server is responsible for them (Server 2) and sends the message there.
3. Server 2 receives the message, looks up which connection is associated with User C, and sends the message to that connection.

This approach works because we always know that a single server is responsible for a given user (or entity, or ID, or whatever). All inbound connections go to that server and, if we want to use the connection associated with that entity, we know to pass it to that server for forwarding to the end client.

### Consistent Hashing

The hashing approach works great when N is fixed, but becomes problematic when we need to scale our service up or down. With simple modulo hashing, changing the number of servers would require almost all users to disconnect and reconnect to different servers - an expensive operation that disrupts service.

**Consistent hashing** solves this by minimizing the number of connections that need to move when scaling. It maps both servers and users onto a hash ring, and each user connects to the next server they encounter when moving clockwise around the ring.

See:

1. [**Consistent Hashing | Algorithms You Should Know**](https://www.youtube.com/watch?v=UF9Iqmg94tk)
2. [**Consistent Hashing: Easy Explanation**](https://www.youtube.com/watch?v=vccwdhfqIrI)
3. [**Consistent Hashing in Hindi with Example**](https://youtu.be/jqUNbqfsnuw)

**Advantages**

- Predictable server assignment
- Minimal connection disruption during scaling
- Works well with stateful connections
- Easy to add/remove servers

**Disadvantages**

- Complex to implement correctly
- Requires coordination service (like Zookeeper)
- All servers need to maintain routing information
- Connection state is lost if a server fails

**When to Use Consistent Hashing**

Consistent hashing is ideal when you need to maintain persistent connections (WebSocket/SSE) and your system needs to scale dynamically. It's particularly valuable when each connection requires significant server-side state that would be expensive to transfer between servers.

For example, in **the Google Docs design**, connections are associated with specific documents that require substantial state to maintain collaborative editing functionality. Consistent hashing helps keep that state on a single server while allowing for scaling.

However, if you're just passing small messages to clients without much associated state, you're probably better off using the next approach: Pub/Sub.

**Things to Discuss in Your Interview**

If you introduce a consistent hashing approach in an interview, you'll want to be able to discuss not only how the updates are routed (e.g. a cooordination service like Zookeeper or etcd). Interviewers are usually interested to understand how the system scales: what happens when we need to increase or decrease the number of nodes. For those instances, you'll want to be able to share your knowledge about consistent hashing but also talk about the orchestration logic necessary to make it work. In practice, this usually means:

1. Signaling the beginning of a scaling event. Recording both the old and new server assignments.
2. Slowly disconnecting clients from the old server and having them reconnect to their newly assigned server.
3. Signaling the end of the scaling event and updating the coordination service with the new server assignments.
4. In the interim, having messages which are sent to both the old and new server until they're fully transitioned.

The mechanics of discovering the initial server assignments is also interesting. Having clients know about the internal structure of your system can be problematic, but there are performance tradeoffs associated with redirecting clients to the correct server or requiring them to do a round-trip to a central server to look up the correct one. Especially during scaling events, any central registration service may become a bottleneck so it's important to discuss the tradeoffs with your interviewer.

## Pushing via Pub/Sub

Another approach to triggering updates is to use a **pub/sub** model. In this model, we have a single service that is responsible for collecting updates from the source and then broadcasting them to all interested clients. Popular options here include Kafka and Redis.

The pub/sub service becomes the biggest source of *state* for our realtime updates. Our persistent connections are now made to lightweight servers which simply subscribe to the relevant topics and forward the updates to the appropriate clients. I'll refer to these servers as *endpoint* servers.

When clients connect, we don't need them to connect to a specific endpoint server (like we did with consistent hashing) and instead can connect to any of them. Once connected, the endpoint server will register the client with the pub/sub server so that any updates can be sent to them.

![real_time_updates_11_pushing_via_pubsub](/notes/images/real_time_updates_11_pushing_via_pubsub.png)

On the connection side, the following happens:

1. The client establishes a connection with an endpoint server.
2. The endpoint server registers the client with the Pub/Sub service, often by creating a topic, subscribing to it, and keeping a mapping from topics to the connections associated with them.

![real_time_updates_12_pushing_via_pubsub](/notes/images/real_time_updates_12_pushing_via_pubsub.png)

On the update broadcasting side, the following happens:

1. Updates are pushed to the Pub/Sub service to the relevant topic.
2. The Pub/Sub service broadcasts the update to all clients subscribed to that topic.
3. The endpoint server receives the update, looks up which client is subscribed to that topic, and forwards the update to that client over the existing connection.

For our chat application, we'll create a topic for each user. When the client connects to our endpoint, it will subscribe to the topic associated with the connected user. When we need to send messages, we publish them to that user's topic and the Pub/Sub service will broadcast them to all of the subscribed servers - then these servers will forward them to the appropriate clients over the existing connections.

### Advantages

- Managing load on endpoint servers is easy, we can use a simple load balancer with "least connections" strategy.
- We can broadcast updates to a large number of clients efficiently.
- We minimize the proliferation of state through our system.

### Disadvantages

- We don't know whether subscribers are connected to the endpoint server, or when they disconnect.
- The Pub/Sub service becomes a single point of failure and bottleneck.
- We introduce an additional layer of indirection which can add to latency.
- There exist many-to-many connections between Pub/Sub service hosts and the endpoint servers.

### When to Use Pub/Sub

Pub/Sub is a great choice when you need to broadcast updates to a large number of clients. It's easy to set up and requires little overhead on the part of the endpoint servers. The latency impact is minimal (<10ms). If you don't need to respond to connect/disconnect events or maintain a lot of state associated with a given client, this is a great approach.

### Things to Discuss in Your Interview

If you're using a pub/sub model, you'll probably need to talk about the single point of failure and bottleneck of the pub/sub service. Redis cluster is a popular way to scale pub/sub service which involves sharding the subscriptions by their key across multiple hosts. This scales up the number of subscriptions you can support and the throughput.

Introducing a cluster for the Pub/Sub component means you'll manage the many-to-many connections between the pub/sub service and the endpoint servers (each endpoint server will be connected to all hosts in the cluster). In some cases this can be managed by carefully choosing topics to partition the service, but in many cases the number of nodes in the cluster is small.

For inbound connections to the endpoint servers, you'll probably want to use a load balancer with a "least connections" strategy. This will help ensure that you're distributing the load across the servers in the cluster. Since the connection itself (and the messages sent across it) are effectively the only resource being consumed, load balancing based on connections is a great way to manage the load.

# When to Use in Interviews

| Pattern | Analogy |
| --- | --- |
| **Polling** | You keep calling a friend every few minutes: “Any news?” |
| **Consistent Hash Push** | Each friend calls *their specific contact person* directly. |
| **Pub/Sub** | Everyone joins a WhatsApp group; any message is automatically broadcasted to subscribers. |

Real-time updates appear in almost every system design interview that involves user interaction or live data. Rather than waiting for the interviewer to ask about real-time features, proactively identify where immediate updates matter and address them in your initial design.

A strong candidate recognizes real-time requirements early. When designing a chat application, immediately acknowledge that "messages need to be delivered instantly to all participants - I'll address that with WebSockets." For collaborative editing, mention that "character-level changes need sub-second propagation between users."

### Common Interview Scenarios

**Chat Applications** - The classic real-time use case. Messages must appear instantly across all participants. WebSockets handle the bidirectional communication perfectly, while pub/sub distributes messages to the right servers. Consider message ordering, typing indicators, and presence status.

**Live Comments** - High-volume, real-time social interaction during live events. Millions of viewers commenting simultaneously creates extreme fan-out problems. Hierarchical aggregation and careful batching prevent system overload while maintaining the live feel.

**Collaborative Document Editing** - Character-level changes need instant propagation between users. WebSockets provide the low-latency communication, while operational transforms or CRDTs handle conflict resolution. User cursors and selection highlighting add additional real-time complexity.

**Live Dashboards and Analytics** - Business metrics and operational data that changes constantly. Server-Sent Events work well for one-way data flow from servers to dashboards. Consider data aggregation intervals and what constitutes "real-time enough" for business decisions.

**Gaming and Interactive Applications** - Multiplayer games need the lowest latency possible. WebRTC enables peer-to-peer communication for reduced latency, while WebSockets handle server coordination. Consider different update frequencies for different game elements.

### When NOT to Use

Avoid real-time updates when you can get away with a simple polling model. If you're not latency sensitive, polling is a great baseline and minimizes complexity — a property highly valued in senior+ interviews. By polling you avoid both hops: you don't need to worry about the client → server protocols, AND you don't have to handle propagation from the event source.

![real_time_updates_13_when_not_to_use](/notes/images/real_time_updates_13_when_not_to_use.png)

### Comparison Table

| Aspect | **Pulling via Polling** | **Pushing via Consistent Hashes** | **Pushing via Pub/Sub** |
| --- | --- | --- | --- |
| **Basic Idea** | Server *periodically asks* data sources for updates | Data sources *directly push* updates to responsible servers based on consistent hashing | Data sources *publish* updates to topics; servers *subscribe* to relevant topics |
| **Flow Type** | *Pull-based* | *Push-based* | *Push-based* |
| **Trigger Direction** | Server initiates | Producer initiates | Producer initiates |
| **Data Routing** | Centralized or round-robin polling | Routed deterministically using hash of data/entity ID | Routed dynamically by topic/channel |
| **Scalability** | Poor at large scale (polling overhead) | Scales horizontally; good load distribution | Excellent horizontal scalability (decoupled architecture) |
| **Latency** | Higher (depends on polling interval) | Low | Very low |
| **Server Load** | High (polling creates unnecessary checks) | Balanced (only relevant servers get data) | Efficient (only subscribers get messages) |
| **Complexity** | Simple | Medium (requires hash ring management) | Medium-High (requires message broker setup) |
| **Reliability** | Depends on polling schedule | Deterministic (if hash ring stable) | Very reliable (message queue/broker handles delivery) |
| **When to Use** | When updates are infrequent or source cannot push | When you have many sources and want deterministic routing without central broker | When you need scalable fan-out/fan-in messaging (many-to-many updates) |
| **Typical Examples** | Server checking DB every 10s for changes | Chat app routing messages to a specific server for a room/user | Redis Pub/Sub, Kafka topics, Google Pub/Sub, AWS SNS |

### When to Choose

| Scenario | Recommended Pattern | Why |
| --- | --- | --- |
| Small system, infrequent updates | ✅ *Pulling via Polling* | Simpler, no infra overhead |
| Real-time, deterministic routing (e.g., chat rooms, GPS tracking) | ✅ *Consistent Hash Push* | Fast, evenly distributed |
| Large-scale, multi-subscriber event propagation (e.g., analytics, notifications, microservices) | ✅ *Pub/Sub* | Decoupled, reliable, scalable |

# Common Deep Dives

Interviewers love to probe the operational challenges and edge cases of real-time systems. Here are the most common follow-up questions you'll encounter.

### How do you handle connection failures and reconnection?

Real-world networks are unreliable. Mobile users lose connections constantly, WiFi drops out, and servers restart. Your real-time system needs graceful degradation and recovery.

The key challenge is detecting disconnections quickly and resuming without data loss. WebSocket connections don't always signal when they break - a client might think it's connected while the server has already cleaned up the connection. Implementing heartbeat mechanisms helps detect these "zombie" connections.

For recovery, you need to track what messages or updates a client has received. When they reconnect, they should get everything they missed. This often means maintaining a per-user message queue or implementing sequence numbers that clients can reference during reconnection. Using **Redis** streams for this is a popular option.

### What happens when a single user has millions of followers who all need the same update?

This is the classic "celebrity problem" in real-time systems. When a celebrity posts, millions of users need that update simultaneously. Naive approaches create massive fan-out that can crash your system.

The solution involves strategic caching and hierarchical distribution. Instead of writing the update to millions of individual user feeds, cache the update once and distribute through multiple layers. Regional servers can pull the update and push to their local clients, reducing the load on any single component. We will see this in details in the Scaling Writes pattern.

![real_time_updates_14_single_user_has_millions_of_followers](/notes/images/real_time_updates_14_single_user_has_millions_of_followers.png)

### How do you maintain message ordering across distributed servers?

When multiple servers handle real-time updates, ensuring consistent ordering becomes complex. Two messages sent milliseconds apart might arrive out of order if they travel different network paths or get processed by different servers.

Vector clocks or logical timestamps help establish ordering relationships between messages. Each server maintains its own clock, and messages include timestamp information that helps recipients determine the correct order.

For critical ordering requirements, you might need to funnel all related messages through a single server or partition. This trades some scalability for consistency guarantees, but simplifies the ordering problem significantly.

Note: For most *product*-style system design interviews, using a single server or partition is the way to go. There's a place for vector clocks and other techniques but they most often apply to deep infra rather than a question like "Design an Online Auction System". If all your messages make their way to a single host, stamping them with the correct timestamp and establishing a total order is straightforward.

# Conclusion

Real-time updates are among the most challenging patterns in system design, appearing in virtually every interactive application from messaging to collaborative editing. The key insight is that real-time systems require solving two distinct problems: client-server communication protocols and server-side update propagation.

Start simple and escalate based on requirements. If polling every few seconds meets your needs, don't jump to complex WebSocket architectures. Most candidates over-engineer real-time solutions when simpler approaches would suffice. However, when true real-time performance is required, understanding the trade-offs between protocols becomes crucial.

For client communication, SSE and WebSockets handle most real-time scenarios effectively. SSE works brilliantly for one-way updates like live dashboards, while WebSockets excel when you need bidirectional communication. Both are well-supported and understood by most infrastructure teams.

On the server side, pub/sub systems provide the best balance of simplicity and scalability for most applications. They decouple update sources from client connections, making your system easier to reason about and scale. Reserve consistent hashing approaches for scenarios where connection state management becomes a primary concern.

Overall real-time update applications bring a lot of interesting complexity to system design: low latency, scaling, networking issues, and the need to manage multiple services and state. By learning about the options available to you, you'll be able to make the best design decision for your system and communicate your reasoning to your interviewer.

# Suggestion for Interview Strength

1. **Scaling WebSockets & SSE**
    1. ✅ **Sticky Sessions:** Use session affinity (based on client ID or cookie) to route subsequent connections to the same backend node, as WebSockets/SSE are stateful.
    2. ✅ **Connection Offloading:** Move connection management to a **dedicated real-time gateway** (like NGINX, HAProxy, or AWS API Gateway WebSocket API) to offload CPU/memory from application servers.
    3. ✅ **Pub/Sub or Event Bus Integration:** Use **Redis Pub/Sub**, **Kafka**, or **NATS** to propagate messages between nodes so that any backend instance can push messages to any connected client.
    4. ✅ **Horizontal Scaling:** Ensure all WebSocket/SSE nodes share connection metadata through a shared store (e.g., Redis, DynamoDB, etcd).
    5. ✅ **Fan-out Optimization:** For large broadcast systems (like stock tickers, sports scores), add an **intermediate push layer** (e.g., **Fanout.io**, **Ably**, or a custom gateway) to efficiently handle 100K+ concurrent clients.
    6. ✅ **Backpressure & Flow Control:** Implement throttling mechanisms to prevent flooding clients with too frequent updates. Use message queues and batching for controlled delivery.
    7. ✅ **Connection Lifecycle Management:** Track and clean up stale or disconnected sessions to free up resources.
2. **Fallback Strategy & Compatibility**
    - ✅ **Progressive Enhancement:** Real-world clients may not support WebSockets (e.g., corporate networks, proxies). Libraries like **Socket.IO** and **SignalR** automatically downgrade:

        ```
        WebSocket → SSE → Long Polling → Short Polling
        ```

    - ✅ **Protocol Detection:** At startup, detect supported technologies and switch dynamically (feature detection).
    - ✅ **Network Resilience:** Implement **auto-reconnect with exponential backoff** and **retry jitter** to avoid reconnection storms.
    - ✅ **Version Compatibility:** Use standardized message formats (JSON or Protobuf) across protocols to ease migration.
3. **Resiliency, Fault Tolerance & Monitoring**
    - ✅ **Reconnection & Resume Logic:** Assign unique connection IDs and use message sequence numbers so that, on reconnect, clients can **resume from last message** instead of starting fresh.
    - ✅ **Load Shedding:** When under heavy load, degrade gracefully — e.g., drop low-priority updates (like typing indicators in chat) to keep critical updates alive.
    - ✅ **Health Checks:** Periodically send ping/pong (WS) or heartbeat events (SSE) to ensure connection liveness.
    - ✅ **Metrics to Track:**
        - Number of active connections
        - Message throughput (messages/sec)
        - Reconnection rate
        - Latency (server → client delivery time)
        - Dropped connections
    - ✅ **Alerting:** Set alerts for sudden connection drops, message delays, or reconnection spikes.
4. **Security & Governance**
    - ✅ **Authentication:** Use **JWT tokens** or **OAuth2 access tokens** during connection handshake. Validate before upgrading to WS/SSE.
    - ✅ **Authorization:** Enforce topic-level or channel-level ACLs — ensure clients can only subscribe to authorized events.
    - ✅ **Encryption:** Always use **WSS (WebSocket Secure)** or HTTPS-based SSE to protect data in transit.
    - ✅ **Rate Limiting & Abuse Prevention:** Throttle message sends per client to avoid spam or DoS.
    - ✅ **Idle Timeout & Cleanup:** Automatically close idle or unauthorized connections.
    - ✅ **Replay Protection:** Sign messages with a timestamp or nonce to prevent replay attacks.
5. **Observability & Debugging**
    - ✅ **Structured Logging:** Include connection IDs, message types, and timestamps for tracing end-to-end flow.
    - ✅ **Distributed Tracing:** Integrate with **OpenTelemetry** to track message latency from source → backend → client.
    - ✅ **Replay Capability:** Store recent messages in Redis/Kafka for debugging or client catch-up after reconnection.
6. **Design Trade-offs to Discuss (Interview Gold 💡)**
    - **SSE vs WebSocket:**
        - SSE: simpler, lightweight, good for 1-way updates.
        - WebSocket: complex, bi-directional, required for chat/games.
    - **When Not to Use WebSocket:** Avoid for small-scale or low-frequency updates — polling might be simpler and cheaper.
    - **Real-time vs Near Real-time:** Highlight trade-off between latency and infrastructure complexity.
    - **Scaling across Regions:** Use global brokers (e.g., AWS Global Accelerator + regional WebSocket clusters) for geo-distributed clients.
    - **Cloud Native Choices:** Mention AWS API Gateway (WS), Google Cloud Pub/Sub with SSE, or Azure SignalR Service for managed scalability.