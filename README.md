## Hamza Elyoussfi

Software engineer from Morocco, INPT graduate. I work on backend systems and I'm moving toward systems programming and AI infrastructure.

I learn a system by building a small version of it: an inference server, a Redis-compatible server, a load balancer, a JSON parser. Then I measure it, so my claims about its behavior come from data, not assumptions.

### Current focus

- **Inference serving on CPUs:** scheduling, dynamic batching and runtime integration (ONNX Runtime), measured under load.
- **Backend systems:** concurrency, protocols, queues, and the limits that keep a server stable under overload.
- **Lower-level direction:** modern C++, Rust, networking and operating systems, with GPU computing (CUDA) as the next step.

### Selected projects

#### [finton](https://github.com/ELyoussfiHamza/finton): a dynamic-batching inference server for CPUs (C++20)

A Triton-inspired server that serves an ONNX model over HTTP. Requests that arrive together are grouped into one batch and run in a single model call. I built it to answer one question with measurements: what does dynamic batching actually buy you on a CPU?

- **Implemented by hand:** the scheduling core (a bounded thread-safe queue, a batch scheduler driven by maximum batch size and maximum delay, a worker pool), the batching logic of the ONNX Runtime backend, and the first version of the HTTP/1.1 server (socket setup, accept loop, partial reads, size limits). No HTTP library is used.
- **Design points:** one promise/future per request instead of a shared result table; the queue rejects work with `503` when full instead of growing; workers pull batches when free, so batch size adapts to load.
- **Measured:** a load generator and sweep scripts, with benchmarks on MNIST and MobileNetV2 (median of three runs, spread shown). On MNIST, batching was 3–4× slower at low concurrency and 24% faster at 64 clients. On MobileNetV2, adding cores mattered more than batching.
- **Checked for correctness:** ThreadSanitizer build, HTTP parser tests, Docker image.
- **Status:** works end to end for single-input, single-output float models. Next on the roadmap: multi-model serving, multi-input models, and an `epoll` event loop to replace one thread per connection. The README states which parts were written with AI assistance.

#### [redis](https://github.com/Elyoussf/redis): a Redis-compatible server (Go)

A learning implementation that real Redis clients can talk to. It has a hand-written serializer and deserializer for RESP, Redis's binary-safe, length-prefixed protocol, plus `SET`, `GET`, `HSET`, `HGET` and `HGETALL`. Each connection runs in its own goroutine, and the stores are guarded by read-write locks.

#### [Load_Balancer](https://github.com/Elyoussf/Load_Balancer): a TCP load balancer (Rust, Tokio)

An async load balancer where backend servers register themselves at startup. Five selection strategies: round robin, sticky round robin, weighted round robin, least connections, and least response time (with periodic health checks). Includes a demo client and server.

#### Smaller from-scratch builds

- [json_parser](https://github.com/Elyoussf/json_parser) (Go): a lexer and parser mapping JSON to Go values, written without a tutorial.
- [chatAppCLI](https://github.com/Elyoussf/chatAppCLI) (Go): a real-time multi-client chat over WebSocket.
- [PingPulse](https://github.com/Elyoussf/PingPulse) (Rust): a CLI that measures HTTP endpoint response times from a YAML config.
- [ping](https://github.com/ELyoussfiHamza/ping) (Rust, in progress): builds ICMP echo packets and sends them over a raw ICMP socket.

#### Backend and applied AI

- [Qrib](https://github.com/ELyoussfiHamza/Qrib) (Django, DRF, Docker): phone OTP login with hashed, expiring codes and JWT, plus worker-profile onboarding drafts.
- [duck](https://github.com/Elyoussf/duck) (Django, PostgreSQL, Neo4j): ingests feed interaction events and projects them into a Neo4j knowledge graph.
- [careeros](https://github.com/ELyoussfiHamza/careeros) (Python, early stage): explainable job matching. Policy lives in deterministic rules; an LLM handles only ambiguous input.

### Experience

**Software Engineer, [Ostorlab](https://ostorlab.co)** (mobile and application security)
Python backend development on an automated security analysis platform: scan reporting, workflow automation for email notifications driven by scan-state conditions, and features around mobile application analysis and shielding. Stack: Python, Django, GraphQL, PostgreSQL, Docker, Kubernetes, cloud infrastructure.

**Education:** Software engineering degree, INPT (Institut National des Postes et Télécommunications), Rabat.

### Skills

| | |
|---|---|
| **Used in production** | Python, Django, GraphQL, PostgreSQL, Docker, Kubernetes, Linux |
| **Built projects with** | C++20 (threads, sync primitives, CMake, ThreadSanitizer), Go, Rust (Tokio), ONNX Runtime, Neo4j, FastAPI, sockets and HTTP/1.1 |
| **Currently learning** | Operating systems internals, networking, performance engineering, CUDA / GPU computing |

### Contact

[X: @helyoussfii](https://x.com/helyoussfii) · [elyoussfiihaamza@gmail.com](mailto:elyoussfiihaamza@gmail.com) · Older experiments and coursework: [@Elyoussf](https://github.com/Elyoussf)
