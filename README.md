# rtCamp Technical Interview Rapid-Fire Cheat Sheet

GROUP 1: Networking & DNS

GROUP 2: Web Server, Nginx & PHP Execution

GROUP 3: Operating System Basics

GROUP 4: Security

GROUP 5: Database, Indexing & Caching

GROUP 6: System Design & Scaling

GROUP 7: JavaScript Core & Advanced

GROUP 8: WordPress Core (Advanced)

GROUP 9: Git (Advanced)

## **GROUP 1: Networking & DNS (Very Common in rtCamp)**

### DNS

**Q: What is DNS?**  
A: DNS translates human-readable domain names into IP addresses.

**Q: Why do we need DNS when we have IPs?**  
A: IPs are hard to remember; DNS provides human-friendly naming.

**Q: What happens when you type `example.com` in a browser?**  
A:

1.  Browser checks cache
    
2.  OS checks DNS cache
    
3.  Resolver queries recursive DNS
    
4.  Root → TLD → Authoritative DNS
    
5.  IP returned → TCP connection → HTTP request
    

**Q: What is a DNS resolver?**  
A: A server that performs recursive DNS lookup on behalf of the client.

**Q: What is an authoritative DNS server?**  
A: The server that holds the final DNS records for a domain.

**Q: What is TTL in DNS?**  
A: Time To Live — how long a DNS record is cached.

**Q: What happens if TTL is very high?**  
A: Faster lookups, but slower propagation of changes.

**Q: Common DNS record types?**  
A:

-   A → IPv4
    
-   AAAA → IPv6
    
-   CNAME → Alias
    
-   MX → Mail server
    
-   TXT → Verification / SPF / DKIM
    
-   NS → Name server
    

**Q: Difference between A and CNAME?**  
A:

-   A maps directly to IP
    
-   CNAME maps to another domain name
    

----------

### Network Basics

**Q: What is TCP?**  
A: Connection-oriented, reliable protocol.

**Q: What is UDP?**  
A: Connectionless, faster, no delivery guarantee.

**Q: Why HTTP uses TCP?**  
A: Reliability is required for web content.

**Q: What is a 3-way handshake?**  
A: SYN → SYN-ACK → ACK.

**Q: What port does HTTP/HTTPS use?**  
A:

-   HTTP → 80
    
-   HTTPS → 443
    

**Q: What is HTTPS?**  
A: HTTP over TLS encryption.

**Q: What problem does HTTPS solve?**  
A: Eavesdropping, tampering, MITM attacks.

----------

### OSI / TCP-IP Layers (Interview Favorite)

**Q: OSI model layers (top → bottom)?**  
A:

1.  Application
    
2.  Presentation
    
3.  Session
    
4.  Transport
    
5.  Network
    
6.  Data Link
    
7.  Physical
    

**Q: TCP/IP model layers?**  
A:

-   Application
    
-   Transport
    
-   Internet
    
-   Network Interface
    

**Q: Where does HTTP live?**  
A: Application layer.

**Q: Where does TCP live?**  
A: Transport layer.

**Q: Where does IP live?**  
A: Network layer.

----------

## **GROUP 1 QUICK CHECKPOINT (Interview-Style)**

**Q: Why DNS caching exists?**  
A: To reduce latency and DNS load.

**Q: What happens if DNS fails?**  
A: Domain becomes unreachable even if server is running.

**Q: Is DNS over HTTPS a thing?**  
A: Yes — DoH prevents DNS snooping.

---

## **GROUP 2: Web Server, Nginx, PHP-FPM, FastCGI (rtCamp Core Area)**

### Web Server Fundamentals

**Q: What is a web server?**  
A: Software that handles HTTP requests and returns responses.

**Q: Examples of web servers?**  
A: Nginx, Apache, LiteSpeed.

**Q: Difference between web server and application server?**  
A:

-   Web server handles HTTP, static files
    
-   App server executes business logic (PHP, Node, Java)
    

----------

### Nginx

**Q: What is Nginx?**  
A: An event-driven, asynchronous web server and reverse proxy.

**Q: Why is Nginx fast?**  
A: Uses non-blocking, event-based architecture instead of per-request threads.

**Q: Nginx vs Apache (core difference)?**  
A:

-   Apache → process/thread based
    
-   Nginx → event loop (single master, worker processes)
    

**Q: What is a worker process in Nginx?**  
A: A process that handles multiple concurrent connections asynchronously.

**Q: Does Nginx spawn a process per request?**  
A: No.

**Q: What is `worker_connections`?**  
A: Maximum connections per worker.

**Q: Why Nginx is good as reverse proxy?**  
A: Efficient connection handling, buffering, load balancing.

----------

### Static vs Dynamic Content

**Q: How does Nginx serve static files?**  
A: Directly from disk without involving PHP.

**Q: Why static content is faster?**  
A: No interpreter or application execution.

----------

### PHP Execution Model

**Q: Can Nginx execute PHP directly?**  
A: No.

**Q: Why Nginx cannot run PHP?**  
A: It has no embedded PHP interpreter.

**Q: How does PHP run with Nginx?**  
A: Via PHP-FPM using FastCGI.

----------

### FastCGI & PHP-FPM

**Q: What is FastCGI?**  
A: A protocol for communicating between web servers and application servers.

**Q: What is PHP-FPM?**  
A: PHP FastCGI Process Manager.

**Q: Why PHP-FPM exists?**  
A: To efficiently manage PHP worker processes.

**Q: What does PHP-FPM manage?**  
A:

-   PHP worker pool
    
-   Process lifecycle
    
-   Memory usage
    
-   Request handling
    

**Q: What happens when a PHP request comes?**  
A:

1.  Nginx receives request
    
2.  Passes to PHP-FPM via FastCGI
    
3.  PHP executes script
    
4.  Response returned to Nginx
    
5.  Sent to client
    

----------

### PHP Internals (Interview Gold)

**Q: Is PHP single-threaded?**  
A: Yes, per request.

**Q: Can PHP handle multiple requests?**  
A: Yes, via multiple PHP-FPM worker processes.

**Q: Is PHP blocking?**  
A: Traditionally yes, but async extensions exist.

**Q: Does one PHP process handle multiple requests simultaneously?**  
A: No.

**Q: What limits PHP concurrency?**  
A: Number of PHP-FPM workers.

----------

### Memory & Performance

**Q: Why long PHP scripts are dangerous?**  
A: They block a worker and reduce throughput.

**Q: What is `pm.max_children`?**  
A: Maximum PHP-FPM worker processes.

**Q: What happens if all workers are busy?**  
A: Requests queue or time out.

----------

## **GROUP 2 QUICK CHECKPOINT**

**Q: Why Nginx + PHP-FPM is popular?**  
A: Efficient I/O handling + controlled PHP execution.

**Q: Where does FastCGI sit?**  
A: Between web server and PHP interpreter.

----------

## **GROUP 3: Operating System Basics — Process, Threads, Concurrency**

### Process

**Q: What is a process?**  
A: A running instance of a program with its own memory space.

**Q: What does a process contain?**  
A:

-   Code
    
-   Heap
    
-   Stack
    
-   Open file descriptors
    
-   Process ID (PID)
    

**Q: Can two processes share memory by default?**  
A: No.

**Q: How do processes communicate?**  
A: IPC (pipes, sockets, shared memory, signals).

----------

### Thread

**Q: What is a thread?**  
A: A lightweight execution unit inside a process.

**Q: Difference between process and thread?**  
A:

-   Process → separate memory
    
-   Threads → shared memory
    

**Q: Why threads are faster than processes?**  
A: Less context switching and shared memory.

**Q: Risk of threads?**  
A: Race conditions, deadlocks.

----------

### Concurrency vs Parallelism

**Q: What is concurrency?**  
A: Handling multiple tasks by interleaving execution.

**Q: What is parallelism?**  
A: Executing multiple tasks at the same time.

**Q: Can you have concurrency without parallelism?**  
A: Yes (single core).

**Q: Can you have parallelism without concurrency?**  
A: No.

----------

### OS Scheduling

**Q: What is context switching?**  
A: OS switching CPU from one task to another.

**Q: Is context switching expensive?**  
A: Yes, but necessary.

----------

### `ps` Command (Linux)

**Q: What does `ps` show?**  
A: Currently running processes.

**Q: Common `ps` usage?**  
A:

-   `ps aux` → all processes
    
-   `ps -ef` → full format
    

**Q: Meaning of columns in `ps aux`?**  
A:

-   PID → process ID
    
-   %CPU → CPU usage
    
-   %MEM → memory usage
    
-   COMMAND → executed command
    

**Q: How to find PHP-FPM processes?**  
A: `ps aux | grep php-fpm`

----------

### PHP & OS

**Q: Is PHP multi-threaded?**  
A: No (per request).

**Q: How PHP achieves concurrency?**  
A: Multiple PHP-FPM worker processes.

**Q: One request = one PHP process?**  
A: Typically yes.

----------

### Nginx & OS

**Q: Does Nginx use threads?**  
A: Mostly no — uses event loop + worker processes.

**Q: Why event loop is efficient?**  
A: One worker can handle thousands of connections.

----------

### Blocking vs Non-Blocking

**Q: What is blocking I/O?**  
A: Task waits until operation completes.

**Q: What is non-blocking I/O?**  
A: Task continues while operation is pending.

**Q: Nginx uses blocking or non-blocking?**  
A: Non-blocking.

**Q: PHP uses blocking or non-blocking?**  
A: Blocking (by default).

----------

## **GROUP 3 QUICK CHECKPOINT**

**Q: Why too many PHP-FPM workers is bad?**  
A: High memory usage, CPU thrashing.

**Q: Why too few workers is bad?**  
A: Requests queue, slow response.

---

## **GROUP 3: Operating System Basics — Process, Threads, Concurrency**


### Process

**Q: What is a process?**  
A: A running instance of a program with its own memory space.

**Q: What does a process contain?**  
A:

-   Code
    
-   Heap
    
-   Stack
    
-   Open file descriptors
    
-   Process ID (PID)
    

**Q: Can two processes share memory by default?**  
A: No.

**Q: How do processes communicate?**  
A: IPC (pipes, sockets, shared memory, signals).

----------

### Thread

**Q: What is a thread?**  
A: A lightweight execution unit inside a process.

**Q: Difference between process and thread?**  
A:

-   Process → separate memory
    
-   Threads → shared memory
    

**Q: Why threads are faster than processes?**  
A: Less context switching and shared memory.

**Q: Risk of threads?**  
A: Race conditions, deadlocks.

----------

### Concurrency vs Parallelism

**Q: What is concurrency?**  
A: Handling multiple tasks by interleaving execution.

**Q: What is parallelism?**  
A: Executing multiple tasks at the same time.

**Q: Can you have concurrency without parallelism?**  
A: Yes (single core).

**Q: Can you have parallelism without concurrency?**  
A: No.

----------

### OS Scheduling

**Q: What is context switching?**  
A: OS switching CPU from one task to another.

**Q: Is context switching expensive?**  
A: Yes, but necessary.

----------

### `ps` Command (Linux)

**Q: What does `ps` show?**  
A: Currently running processes.

**Q: Common `ps` usage?**  
A:

-   `ps aux` → all processes
    
-   `ps -ef` → full format
    

**Q: Meaning of columns in `ps aux`?**  
A:

-   PID → process ID
    
-   %CPU → CPU usage
    
-   %MEM → memory usage
    
-   COMMAND → executed command
    

**Q: How to find PHP-FPM processes?**  
A: `ps aux | grep php-fpm`

----------

### PHP & OS

**Q: Is PHP multi-threaded?**  
A: No (per request).

**Q: How PHP achieves concurrency?**  
A: Multiple PHP-FPM worker processes.

**Q: One request = one PHP process?**  
A: Typically yes.

----------

### Nginx & OS

**Q: Does Nginx use threads?**  
A: Mostly no — uses event loop + worker processes.

**Q: Why event loop is efficient?**  
A: One worker can handle thousands of connections.

----------

### Blocking vs Non-Blocking

**Q: What is blocking I/O?**  
A: Task waits until operation completes.

**Q: What is non-blocking I/O?**  
A: Task continues while operation is pending.

**Q: Nginx uses blocking or non-blocking?**  
A: Non-blocking.

**Q: PHP uses blocking or non-blocking?**  
A: Blocking (by default).

----------

## **GROUP 3 QUICK CHECKPOINT**

**Q: Why too many PHP-FPM workers is bad?**  
A: High memory usage, CPU thrashing.

**Q: Why too few workers is bad?**  
A: Requests queue, slow response.

----------

## **GROUP 4: Security — Sessions, Cookies, CSRF, XSS, Attacks**

----------

### Cookies

**Q: What is a cookie?**  
A: Small data stored in the browser and sent with every request.

**Q: Why cookies exist?**  
A: HTTP is stateless.

**Q: Cookie size limit?**  
A: ~4KB.

**Q: Cookie scope?**  
A: Domain + path.

**Q: Secure cookie flag?**  
A: Sent only over HTTPS.

**Q: HttpOnly cookie?**  
A: Not accessible via JavaScript.

**Q: SameSite cookie?**  
A: Controls cross-site cookie sending.

----------

### Sessions

**Q: What is a session?**  
A: Server-side storage of user state.

**Q: How sessions work internally?**  
A: Session ID stored in cookie → server maps it to data.

**Q: Session vs cookie?**  
A:

-   Cookie → client-side
    
-   Session → server-side
    

**Q: Why sessions are safer than cookies?**  
A: Sensitive data not exposed to client.

----------

### Session Attacks

**Q: What is session hijacking?**  
A: Attacker steals session ID.

**Q: How session hijacking happens?**  
A: XSS, insecure cookies, sniffing.

**Q: Session fixation?**  
A: Forcing victim to use attacker-known session ID.

**Q: Prevention of session attacks?**  
A:

-   Regenerate session ID
    
-   Secure + HttpOnly cookies
    
-   HTTPS
    

----------

### CSRF (Very Important)

**Q: What is CSRF?**  
A: Forged request using victim’s authenticated session.

**Q: Why CSRF works?**  
A: Browsers automatically attach cookies.

**Q: What CSRF cannot do?**  
A: Read response data.

**Q: CSRF protection techniques?**  
A:

-   CSRF tokens
    
-   SameSite cookies
    
-   Double-submit cookie
    

**Q: How WordPress prevents CSRF?**  
A: Nonces.

----------

### WordPress Nonces

**Q: What is a WP nonce?**  
A: Time-based token to protect against CSRF.

**Q: Is WP nonce truly random?**  
A: No, it is time-based and user-specific.

**Q: Nonce lifetime in WP?**  
A: 12–24 hours (two ticks).

**Q: What nonce does NOT protect against?**  
A: XSS.

----------

### XSS

**Q: What is XSS?**  
A: Injecting malicious JavaScript into pages.

**Q: Types of XSS?**  
A:

-   Stored
    
-   Reflected
    
-   DOM-based
    

**Q: Why XSS is dangerous?**  
A: Can steal cookies, sessions, tokens.

**Q: XSS prevention?**  
A:

-   Output escaping
    
-   Input validation
    
-   Content Security Policy
    

**Q: WordPress escaping functions?**  
A:

-   `esc_html()`
    
-   `esc_attr()`
    
-   `wp_kses()`
    

----------

### Password Attacks

**Q: What is a dictionary attack?**  
A: Trying common passwords from a list.

**Q: What is a brute-force attack?**  
A: Trying all combinations.

**Q: What is a rainbow table?**  
A: Precomputed hash lookup table.

**Q: Why rainbow tables work?**  
A: Unsalted hashes produce same output.

**Q: How to prevent rainbow table attacks?**  
A:

-   Salting
    
-   Strong hashing (bcrypt, Argon2)
    

**Q: WordPress password hashing?**  
A: Uses salted hashes with portable phpass / bcrypt.

----------

### SQL Injection (Quick)

**Q: What is SQL injection?**  
A: Injecting malicious SQL via input.

**Q: Prevention?**  
A: Prepared statements.

**Q: WordPress function for safe queries?**  
A: `$wpdb->prepare()`.

----------

## **GROUP 4 QUICK CHECKPOINT**

**Q: CSRF vs XSS?**  
A:

-   CSRF → unwanted action
    
-   XSS → code execution
    

**Q: Why HttpOnly cookie matters?**  
A: Prevents JS access even if XSS exists.

----------

## **GROUP 5: Database, Indexing & Caching**

### Database Index

**Q: What is a database index?**  
A: A data structure that speeds up data retrieval.

**Q: Why indexes are fast?**  
A: They avoid full table scans.

**Q: Common index data structure?**  
A: B-tree.

**Q: What is a primary index?**  
A: Index on primary key.

**Q: What is a composite index?**  
A: Index on multiple columns.

**Q: When indexes slow things down?**  
A: On INSERT, UPDATE, DELETE.

----------

### Index Usage

**Q: Why index on `WHERE` columns?**  
A: To speed up filtering.

**Q: Does index help with `LIKE '%abc%'`?**  
A: No.

**Q: Does index help with `ORDER BY`?**  
A: Yes, if index order matches query.

**Q: Why too many indexes are bad?**  
A: Slower writes and more disk usage.

----------

### WordPress & DB

**Q: Default WP database engine?**  
A: InnoDB.

**Q: Why InnoDB preferred?**  
A: Row-level locking, transactions.

**Q: WP table prefix purpose?**  
A: Multiple installs in one DB, security through obscurity.

----------

### Query Optimization

**Q: What is N+1 query problem?**  
A: One query per item instead of batching.

**Q: How to avoid N+1 in WP?**  
A: Use meta queries wisely, caching, joins.

**Q: How to inspect slow queries?**  
A: Enable slow query log.

----------

### Caching (Very Important)

**Q: What is caching?**  
A: Storing data to avoid recomputation.

**Q: Types of caching?**  
A:

-   Browser
    
-   Page
    
-   Object
    
-   Opcode
    
-   Database
    

----------

### Browser Cache

**Q: Browser caching headers?**  
A: Cache-Control, Expires, ETag.

----------

### Page Cache

**Q: What is page caching?**  
A: Caching full HTML output.

**Q: When page cache is ineffective?**  
A: Logged-in users, personalized content.

----------

### Object Cache

**Q: What is object caching?**  
A: Caching PHP objects or query results.

**Q: WP object cache function?**  
A: `wp_cache_get()` / `wp_cache_set()`.

**Q: Persistent object cache tools?**  
A: Redis, Memcached.

----------

### Opcode Cache

**Q: What is OPcache?**  
A: Caches compiled PHP bytecode.

**Q: Why OPcache improves performance?**  
A: Avoids recompiling PHP files.

----------

### Cache Invalidation

**Q: Hardest problem in caching?**  
A: Cache invalidation.

**Q: Common invalidation strategy?**  
A: TTL, manual purge.

----------

## **GROUP 5 QUICK CHECKPOINT**

**Q: Why caching improves scalability?**  
A: Reduces DB and CPU load.

**Q: Can caching cause bugs?**  
A: Yes, stale data.

----------

## **GROUP 6: System Design, Scaling & Architecture**

### Scaling

**Q: What is scalability?**  
A: Ability to handle increased load.

**Q: Vertical scaling?**  
A: Adding more resources to a single machine.

**Q: Horizontal scaling?**  
A: Adding more machines.

**Q: Vertical scaling limitations?**  
A: Hardware limits, downtime.

**Q: Horizontal scaling challenges?**  
A: Data consistency, session management.

----------

### Load Balancing

**Q: What is a load balancer?**  
A: Distributes traffic across servers.

**Q: Common load balancing algorithms?**  
A:

-   Round robin
    
-   Least connections
    
-   IP hash
    

**Q: Where sessions break in load balancing?**  
A: When requests hit different servers.

**Q: Solutions for session handling?**  
A:

-   Sticky sessions
    
-   Central session store (Redis)
    

----------

### High Availability

**Q: What is high availability?**  
A: System remains operational during failures.

**Q: Single point of failure?**  
A: A component whose failure brings down the system.

**Q: How to remove SPOF?**  
A: Redundancy.

----------

### Stateless Systems

**Q: What is a stateless application?**  
A: No user state stored on server.

**Q: Why stateless systems scale better?**  
A: Any request can go to any server.

----------

### Database Scaling

**Q: Read scaling approach?**  
A: Read replicas.

**Q: Write scaling approach?**  
A: Sharding.

**Q: What is sharding?**  
A: Splitting data across databases.

----------

### Caching in System Design

**Q: Cache location options?**  
A: Client, CDN, server, DB.

**Q: CDN purpose?**  
A: Cache static content close to users.

----------

### Queue & Async Processing

**Q: Why background jobs exist?**  
A: Avoid blocking user requests.

**Q: Examples in WP?**  
A: Email sending, exports, cron jobs.

**Q: Tools?**  
A: WP-Cron, Action Scheduler.

----------

### Rate Limiting

**Q: What is rate limiting?**  
A: Restricting number of requests.

**Q: Why needed?**  
A: Prevent abuse, DDoS.

----------

### Logging & Monitoring

**Q: Why logging is important?**  
A: Debugging and auditing.

**Q: What should be logged?**  
A: Errors, warnings, critical actions.

----------

## **GROUP 6 QUICK CHECKPOINT**

**Q: First thing to cache in WP?**  
A: Page cache.

**Q: First scaling move?**  
A: Vertical, then horizontal.

----------

## **GROUP 7: JavaScript — Core & Advanced Concepts**

### JavaScript Basics

**Q: What is JavaScript?**  
A: Single-threaded, event-driven, interpreted language.

**Q: Where does JS run?**  
A: Browser and server (Node.js).

**Q: Is JavaScript single-threaded?**  
A: Yes (main execution thread).

----------

### Execution Model

**Q: What is the call stack?**  
A: Stack that tracks function execution.

**Q: What is the event loop?**  
A: Mechanism that handles async callbacks.

**Q: What is the task queue?**  
A: Queue of async callbacks waiting to execute.

**Q: Why JS is non-blocking?**  
A: Async tasks are offloaded and resumed via event loop.

----------

### Synchronous vs Asynchronous

**Q: What blocks the JS thread?**  
A: Long-running synchronous code.

**Q: Does `setTimeout` run exactly on time?**  
A: No, it runs after the delay when stack is free.

----------

### Hoisting

**Q: What is hoisting?**  
A: Moving declarations to top during compilation.

**Q: Are `let` and `const` hoisted?**  
A: Yes, but in Temporal Dead Zone.

**Q: `var` vs `let`?**  
A:

-   `var` → function-scoped
    
-   `let` → block-scoped
    

----------

### Closures

**Q: What is a closure?**  
A: Function with access to its lexical scope.

**Q: Why closures are useful?**  
A: Data encapsulation, callbacks.

----------

### `this` Keyword

**Q: What determines `this` value?**  
A: How the function is called.

**Q: Arrow function `this` behavior?**  
A: Inherits from lexical scope.

----------

### Prototype & Objects

**Q: How JS objects work internally?**  
A: Via prototype chain.

**Q: Why prototype is memory-efficient?**  
A: Shared methods across instances.

**Q: Difference between class and prototype?**  
A: Classes are syntactic sugar over prototypes.

----------

### Promises

**Q: What is a Promise?**  
A: Object representing future value.

**Q: Promise states?**  
A: Pending, fulfilled, rejected.

**Q: `async/await` is built on?**  
A: Promises.

----------

### Event Delegation

**Q: What is event delegation?**  
A: Handling events at parent level.

**Q: Why useful?**  
A: Performance and dynamic elements.

----------

### Memory Management

**Q: What is garbage collection?**  
A: Automatic memory cleanup.

**Q: Common memory leak cause?**  
A: Unremoved event listeners.

----------

## **GROUP 7 QUICK CHECKPOINT**

**Q: Why JS is single-threaded?**  
A: Simplicity and DOM safety.

**Q: How JS handles concurrency?**  
A: Event loop.

----------

## **GROUP 8: WordPress Core — Advanced Concepts**

### WordPress Architecture

**Q: Is WordPress MVC?**  
A: No.

**Q: WordPress architecture style?**  
A: Event-driven, hook-based.

**Q: Core components of WP?**  
A:

-   Core
    
-   Themes
    
-   Plugins
    
-   Database
    

----------

### Hooks (Very Important)

**Q: What is a hook?**  
A: A point where code can modify behavior.

**Q: Types of hooks?**  
A:

-   Actions
    
-   Filters
    

**Q: Action vs Filter?**  
A:

-   Action → do something
    
-   Filter → modify data
    

**Q: Can actions return values?**  
A: No.

**Q: Can filters return values?**  
A: Yes (must).

----------

### Plugin Loading

**Q: When plugins load?**  
A: After `plugins_loaded`.

**Q: When theme loads?**  
A: After `after_setup_theme`.

**Q: Earliest safe hook?**  
A: `muplugins_loaded`.

----------

### WP Query

**Q: What is `WP_Query`?**  
A: Core class for fetching posts.

**Q: Difference between `query_posts` and `WP_Query`?**  
A:

-   `query_posts` modifies main query (bad)
    
-   `WP_Query` is safe
    

----------

### REST API

**Q: WP REST API purpose?**  
A: Expose data via JSON endpoints.

**Q: REST authentication methods?**  
A: Cookie, OAuth, Application Passwords.

----------

### Database Layer

**Q: What is `$wpdb`?**  
A: Database abstraction class.

**Q: Why `$wpdb->prepare()` important?**  
A: Prevent SQL injection.

----------

### Security in WP

**Q: What are nonces used for?**  
A: CSRF protection.

**Q: Capability vs Role?**  
A:

-   Role → collection of capabilities
    
-   Capability → permission
    

**Q: Function to check capability?**  
A: `current_user_can()`.

----------

### Data Handling

**Q: Sanitization vs Escaping?**  
A:

-   Sanitize → before saving
    
-   Escape → before output
    

**Q: Example sanitize functions?**  
A: `sanitize_text_field()`, `sanitize_email()`.

----------

### Performance

**Q: What is object cache in WP?**  
A: In-memory caching layer.

**Q: Transients purpose?**  
A: Cache data with expiration.

----------

### Cron

**Q: What is WP-Cron?**  
A: Pseudo-cron triggered by page loads.

**Q: Limitation of WP-Cron?**  
A: Depends on traffic.

----------

### Gutenberg

**Q: What is Gutenberg?**  
A: Block-based editor.

**Q: Gutenberg built on?**  
A: React.

----------

## **GROUP 8 QUICK CHECKPOINT**

**Q: Why `query_posts()` is discouraged?**  
A: Breaks main query and pagination.

**Q: Safest way to extend WP?**  
A: Hooks.

----------

Perfect. This is the **last core group**. Git questions are usually short but decisive.

----------

## **GROUP 9: Git — Advanced Concepts**

----------

### Git Basics (Very Short)

**Q: What is Git?**  
A: Distributed version control system.

**Q: Git vs GitHub?**  
A:

-   Git → tool
    
-   GitHub → hosting platform
    

----------

### Branching & Merging

**Q: What is a branch?**  
A: Pointer to a commit.

**Q: What is merge?**  
A: Combining branches.

**Q: Merge conflict occurs when?**  
A: Same lines changed differently.

----------

### Cherry-pick

**Q: What is `git cherry-pick`?**  
A: Apply a specific commit from another branch.

**Q: When to use cherry-pick?**  
A: When you need one commit, not entire branch.

**Q: Risk of cherry-pick?**  
A: Duplicate commits, history complexity.

----------

### Rebase (Very Important)

**Q: What is `git rebase`?**  
A: Move commits to a new base.

**Q: Rebase vs merge?**  
A:

-   Merge → preserves history
    
-   Rebase → linear history
    

**Q: When not to rebase?**  
A: On shared/public branches.

**Q: Why rebase is dangerous?**  
A: Rewrites commit history.

----------

### Stash

**Q: What is `git stash`?**  
A: Temporary storage for uncommitted changes.

**Q: Does stash include untracked files?**  
A: No (unless `-u`).

**Q: `git stash pop` vs `apply`?**  
A:

-   pop → apply + delete
    
-   apply → apply only
    

----------

### Reset

**Q: `git reset --soft`?**  
A: Moves HEAD, keeps changes staged.

**Q: `git reset --mixed`?**  
A: Moves HEAD, keeps changes unstaged.

**Q: `git reset --hard`?**  
A: Discards everything.

----------

### Checkout vs Switch

**Q: `git checkout` used for?**  
A: Switch branches or restore files.

**Q: `git switch` purpose?**  
A: Only branch switching.

----------

### Reflog

**Q: What is `git reflog`?**  
A: History of HEAD movements.

**Q: Why reflog is powerful?**  
A: Recover lost commits.

----------

### Pull vs Fetch

**Q: `git fetch`?**  
A: Downloads changes only.

**Q: `git pull`?**  
A: Fetch + merge/rebase.

----------

## **GROUP 9 QUICK CHECKPOINT**

**Q: Safest way to clean commit history?**  
A: Interactive rebase (before pushing).

**Q: Emergency recovery command?**  
A: `git reflog`.

----------

## **FINAL INTERVIEW MINDSET (Important)**

-   You are **not expected to answer everything**
    
-   Say **“I don’t recall exactly, but conceptually…”** when unsure
    
-   Structure answers: **What → Why → How**
    
-   Stay calm
    
