# Puma

Puma is a threaded Ruby HTTP application server processing requests across a TCP and/or UNIT socket

Puma processes (there can be one or many) accept connections from the socket via a thread. The connection, once fully
buffered and read, moves into the todo list, where an available thread will pick it up

Puma works in two main modes: cluster and single. On single mode, only one Puma process boots. In cluster mode, a
`master` process is booted which prepares( and may boot)  the applicaton and then uses the fork() system call to create
one or more child processes.
These child processes all listen to the same socket. The master process does not listen to the socket or process
requests its purpose is primarily to manage and listen for UNIX singals and possibliy kill or boot child processes.

We sometimes call child processes( or Puma processes in single mode) workers, and we sometimes call the thread created
by Puma's ThreadPool worker thread

## How request works

- Upon startup, Puma listens on a TCP or UNIX socket.
  - The backlog of this socket is configured with a default of 1024, but actual backlog value is capped by the
  `net.core.somaxconn` sysctl value. The backlog determines the size of the queue for unaccepted connections. If the
  backlog is full, the operating system is not accepting new connections.
  - This socket backlog is distinct from the backlog of work as reported by Puma.stats or the control server. The
    backlog that `Puma.stats` refers to represents the number of connections in the porcess' todo set waiting for a
    thread from the `ThreadPool`
- By default, a single, separate thread (created by the Reactor class) reads and buffers request from the socket.
  - When at least one worker thread is available for work the reactor thread listens to the socket and accepts a request
    ( if one is waiting )
  - The reactor thread waits for the entire HTTP request to be received
    - Puma exposes the time spent waiting for the HTTP request body to be received to the Rack app as
  `env['puma.request_body_wait']` Milliseconds
  - Once full buffered and received, the connection is pushed into the todo set.
- Worker threads pop work off the todo set processing
  - The worker thread processes the request via calling the configured Rack application. The Rack application generates
    the HTTP response.
  -  The worker thread writes the response to the connection. While puma buffers request via a separate thread, it does
     not use a separate thread for responses
  - Once done, the thread becomes available to prcess another connection in the todo set

- Queue requests
  - The queue_request options is true by default, enabling the separate reactor thread used to buffer reques as
    described above
  - If set to false, this buffer will not be used for connectons while waiting for the request to arrive
  - In this mode, when a connection is accepted, it's added to the todo queue immediately, and a worker will
    synchronously do anw waiting necessary to read the HTTP request from the socket

## Restart

### Hot restart

- Work in cluster mode and single mode
- Supported on all platforms
- Client experience
  - All platforms: Client with an in-flight request are served responses before the connection is closed gracefully.
    Puma gracefully disconnects any idle HTTP persitent connection before restarting.
  - On MRI or TruffleRuby on Linux and BDS: Client who connect just before the server restarts may experience increased
    latency while the server stops and starts again, but there connections will not be closed permaturely
  - On Windows and JRuby: Clients who connect just before a restart may experience connection reset error
  - Additional notes:
    - Only one version of application is running at a time
    - `on_restart` is invoked just before the server shut down. This can be used to clean up resources(like long live db
      connections ) gracefully.

### Phased restart

Replace all running workers in a Puma cluster. This is a usful way to upgrade the application that Puma is serving
gracefully. A phased restart works by first killing an old worker, then starting a new worker, waiting until the new
worker has successully started before proceeding to the next worker. This process continues until all workers are
replaced. The master process is not restarted

- Client experience
- In-flight request are always served responsed before the connection is closed gracefully
- Idle persistent connection are gracefully disconnected
- New connection are not lost and client will not experence any increase in latency
