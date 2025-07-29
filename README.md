# SQLite

>SQLite is the most used database engine in the world.
>
>https://www.sqlite.org/index.html

## Most Widely Deployed and Used Database Engine

From: [https://www.sqlite.org/mostdeployed.html](https://www.sqlite.org/mostdeployed.html)

SQLite is likely used more than all other database engines combined. Billions and billions of copies of SQLite exist in the wild. SQLite is found in:

- Every Android device
- Every iPhone and iOS device
- Every Mac
- Every Windows10 machine
- Every Firefox, Chrome, and Safari web browser
...

---

## Database is mission-critical

Losing it means losing control of your business.

You should choose a mature and battle-tested database software.

Be careful with forks and rewrites — there’s often little to gain, but a lot to lose.

There's probably an [extension](#list-of-sqlite-extensions---by-chatgpt) for everything that
you want to do, enabling you to achive your goal while maintaining the stability and portability
of the core SQLite library.

## How SQLite is tested

From: [https://www.sqlite.org/testing.html](https://www.sqlite.org/testing.html)

The reliability and robustness of SQLite is achieved in part by thorough and careful testing.

As of version 3.42.0 (2023-05-16), the SQLite library consists of approximately 155.8 KSLOC of C code. (KSLOC means thousands of "Source Lines Of Code" or, in other words, lines of code excluding blank lines and comments.) By comparison, the project has 590 times as much test code and test scripts - 92053.1 KSLOC.

---

- [Extensions - by Gemini](#extensions---by-gemini)
- [List of SQLite Extensions - by ChatGPT](#list-of-sqlite-extensions---by-chatgpt)
- [Extensions for Sync & Replication - by ChatGPT](#sqlite-extensions-for-sync--replication---by-chatgpt)
- [How to create isolated Copy-on-Write branches - by Gemini](#how-to-create-isolated-copy-on-write-branches---by-gemini)

## Extensions - by Gemini

Yes, using SQLite with its robust extension ecosystem is often a superior choice to creating or using a fork. Extensions provide specialized functionality while maintaining the stability and portability of the core SQLite library.

### How SQLite Extensions Work

SQLite extensions are shared libraries (e.g., `.so` on Linux, `.dll` on Windows, `.dylib` on macOS) that can be loaded into a running SQLite process at runtime. This dynamic loading allows developers to add new features without altering the core SQLite source code.

The mechanism is straightforward:
1.  **Loading**: The host application uses a specific API call, `sqlite3_load_extension()`, to load the shared library file into memory. Many command-line shells and toolkits provide a simpler command, like `.load ./my_extension`.
2.  **Initialization**: Upon loading, SQLite looks for a specific C function within the library, typically named `sqlite3_extension_init`.
3.  **Registration**: This initialization function is responsible for registering new application-defined SQL functions, virtual tables, collating sequences, or other features with the SQLite core using the runtime APIs (e.g., `sqlite3_create_function_v2()`, `sqlite3_create_module()`).

Once registered, these new features can be used in any SQL statement executed through that database connection, just as if they were built-in.

---

### A Brief History of Extensions

The extension loading mechanism, `sqlite3_load_extension()`, was introduced in **SQLite version 3.3.0** on January 10, 2006. This was a pivotal moment, transforming SQLite from a self-contained database engine into a powerful, extensible platform.

Initially, this feature was disabled by default for security reasons, as loading arbitrary code could be a risk. However, its immense utility led to its widespread adoption. Early extensions were often created for specific needs, but a more formalized ecosystem began to emerge as developers recognized the power of adding features like full-text search and custom data processing directly into the database layer. The development of the **Full-Text Search (FTS)** modules (now on FTS5) is a prime example of an early, highly successful extension that eventually became a standard part of the official SQLite amalgamation.

---

### Most Used Extensions

Several extensions have become de facto standards, widely used in various applications.

* **JSON1**: This is arguably the most important modern extension. It adds a comprehensive set of functions (e.g., `json()`, `json_extract()`, `json_object()`) for manipulating JSON stored in text fields. It's now included by default in most standard SQLite compilations.
* **FTS5 (Full-Text Search 5)**: The latest iteration of the full-text search extension. It allows for efficient, complex text searches on document collections, similar to dedicated search engines. It supports phrase searching, ranking, and prefix queries.
* **R*Tree**: This extension provides an R-tree index, a specialized data structure for indexing multi-dimensional data. It's essential for performing fast geospatial queries, like finding all points within a specific rectangle.
* **Session**: Creates a "changeset" that records modifications to a database. These changesets can be stored, inverted, and applied to other databases, enabling features like undo/redo stacks or synchronizing databases between different devices.
* **Crypto**: Extensions like `SEE` (the official SQLite Encryption Extension), `SQLCipher`, and `sqleet` provide transparent, page-level encryption of the entire database file.

---

### Common Use Cases

The extension ecosystem makes SQLite suitable for a much wider range of tasks beyond simple data storage.

* **Geospatial Applications 🗺️**: Using the **R\*Tree** extension, developers can build applications like mapping software or location-based services that need to quickly query for data within geographical boundaries (e.g., "find all cafes within this map view").
* **Content and Document Search 🔍**: **FTS5** is used extensively in applications that need to search through large volumes of text, such as email clients, note-taking apps, and content management systems.
* **NoSQL-style Data Handling**: The **JSON1** extension allows SQLite to function as a capable document database. Applications can store flexible, schema-less JSON objects and still use SQL to query into their structure, offering a hybrid SQL/NoSQL approach.
* **Data Synchronization and Versioning 🔄**: The **Session** extension is perfect for applications that need to work offline and sync changes later. It can determine what changed and create a compact patch to update a remote server or another client, without transmitting the entire database.
* **Secure Local Storage 🔒**: Encrypting a local SQLite database using an extension like **SQLCipher** is a standard practice for mobile and desktop applications that handle sensitive user data, such as password managers or health apps.

---

### Why Use Extensions Over a Fork?

Using the official SQLite core with extensions is almost always better than maintaining a fork of SQLite.

* **Maintainability and Upgrades**: SQLite is under constant, expert development. When a new version is released with performance improvements, bug fixes, and security patches, updating is simple: you just swap out the core library. If you use a fork, you are responsible for merging all those upstream changes into your custom codebase, which is a complex and error-prone process.
* **Stability and Reliability**: The official SQLite core is one of the most rigorously tested software components in the world. A fork introduces custom code that has not undergone the same level of scrutiny, potentially introducing new bugs and instabilities.
* **Interoperability**: Extensions are designed to be modular. You can load multiple extensions into the same database connection. Forks, however, are monolithic. If you need features from two different forks, it's impossible to combine them. With extensions, you simply load both.
* **Ecosystem and Community Support**: By sticking with the official SQLite, you benefit from the entire ecosystem of tools, documentation, and community knowledge. When you have a problem, it's far easier to get help for standard SQLite with a well-known extension than for a custom, one-off fork. A fork isolates you from this community.

## List of SQLite Extensions - by ChatGPT

Here are **25 popular SQLite extensions**. These include general-purpose utility packs, data processing tools, and modern features like vector search and HTTP integration:

---

### 🧩 **[Sqlean Extensions (by Anton Zhiyanov)](https://github.com/nalgeon/sqlean)**

A standard library of SQLite extensions — portable, fast, and easy to load.

1. 🔐 [crypto](https://github.com/nalgeon/sqlean/tree/main/crypto) – hashing, encoding/decoding
2. 📂 [fileio](https://github.com/nalgeon/sqlean/tree/main/fileio) – file system access
3. 🧠 [fuzzy](https://github.com/nalgeon/sqlean/tree/main/fuzzy) – fuzzy string matching
4. 🌐 [ipaddr](https://github.com/nalgeon/sqlean/tree/main/ipaddr) – IP address parsing
5. ➕ [math](https://github.com/nalgeon/sqlean/tree/main/math) – extra math functions
6. 🔍 [regexp](https://github.com/nalgeon/sqlean/tree/main/regexp) – regular expression matching
7. 📊 [stats](https://github.com/nalgeon/sqlean/tree/main/stats) – statistics (mean, median, percentiles)
8. 🔤 [text](https://github.com/nalgeon/sqlean/tree/main/text) – Unicode string functions
9. ⏱️ [time](https://github.com/nalgeon/sqlean/tree/main/time) – extended date/time ops
10. 🆔 [uuid](https://github.com/nalgeon/sqlean/tree/main/uuid) – UUID generation
11. 📄 [vsv](https://github.com/nalgeon/sqlean/tree/main/vsv) – query CSV/TSV files
12. 📐 [define](https://github.com/nalgeon/sqlean/tree/main/define) – define new SQL functions at runtime

Bundle info: [https://antonz.org/sqlean](https://antonz.org/sqlean)

---

### 🔬 **[SQLite Ecosystem Extensions (by asg017)](https://github.com/asg017/sqlite-ecosystem)**

Modern, composable extensions written in Rust and WASM for SQLite apps and CLI.

13. 🌍 [sqlite-http](https://github.com/asg017/sqlite-http) – send HTTP requests from SQL
14. 🕸️ [sqlite-html](https://github.com/asg017/sqlite-html) – parse HTML with CSS selectors
15. 📁 [sqlite-lines](https://github.com/asg017/sqlite-lines) – read files line-by-line
16. 🧭 [sqlite-path](https://github.com/asg017/sqlite-path) – manipulate file paths
17. 🌐 [sqlite-url](https://github.com/asg017/sqlite-url) – parse and build URLs
18. 🧬 [sqlite-ulid](https://github.com/asg017/sqlite-ulid) – generate sortable ULIDs
19. 🔢 [sqlite-fastrand](https://github.com/asg017/sqlite-fastrand) – fast random numbers
20. 🧠 [sqlite-vss](https://github.com/asg017/sqlite-vss) – vector similarity search
21. 🧪 [sqlite-jsonschema](https://github.com/asg017/sqlite-jsonschema) – validate JSON with JSON Schema
22. 🧮 [sqlite-regex](https://github.com/asg017/sqlite-regex) – Rust-based regex support

Project page: [https://github.com/asg017/sqlite-ecosystem](https://github.com/asg017/sqlite-ecosystem)

---

### ⚡ Other Notable Extensions

23. 🧠 [SQLite-Vector](https://github.com/asg017/sqlite-vss) – vector search with index support
24. 🤖 [SQLite-AI](https://github.com/sqlite/sqlite/blob/trunk/ext/ai/README.md) – run AI models in SQL (experimental)
25. 🔁 [rqlite Extensions](https://github.com/rqlite/rqlite/tree/main/contrib) – examples include `rot13`, `carray`, and ICU support

---

## SQLite Extensions for Sync & Replication - by ChatGPT

#### 1. [Mycelite](https://github.com/mycelial/mycelite)

A **VFS‑based extension** for **single‑writer, physical replication** between SQLite instances. It intercepts page writes and stores binary diffs for transmission and replay, creating bit‑perfect replicas. Currently supports only one active writer. ([GitHub][1], [Hacker News][2])

#### 2. [sqlite‑sync (SQLiteSync)](https://github.com/sqliteai/sqlite-sync)

A **CRDT‑powered extension** that enables **multi‑master, conflict‑free synchronization** across devices—even offline. Changes merge automatically without conflict resolution logic. Ideal for real-time collaborative, local‑first apps. ([GitHub][3], [SQLite Cloud][4])

#### 3. [cr‑sqlite](https://github.com/vlcn-io/cr-sqlite)

Transforms tables into **Conflict‑free Replicated Data Types**, enabling **logical, row‑level replication**, automatic merge resolution, and offline support via CRDTs. ([Hacker News][5], [SQLSync][6])

---

### 🧱 Distributed SQLite Cluster Solutions

These are full-fledged systems built around SQLite to handle replication at scale:

#### 4. [rqlite](https://github.com/rqlite/rqlite)

A **distributed database** layer on top of SQLite using the **Raft consensus protocol**. Enables **high‑availability, transactional replication** across nodes. Interaction happens via HTTP API rather than raw SQLite files. ([SQLite][7], [Hacker News][5])

#### 5. [LiteSync](https://litesync.io)

A multi-master sync system letting all nodes write to their local SQLite, even offline. Synchronization is efficient and automatic, compressing only new data without full database transfers. Configurable via URI and works across mobile, desktop, and IoT. ([litesync.io][8])

#### 6. [Litestream](https://litestream.io)

A **streaming backup and replication tool** for SQLite that continuously pushes WAL frames (via file‑watchers) to durable object storage (e.g. S3). Great for point‑in‑time recovery and async replicas. Suited for read‑only failover rather than live write sync. ([litestream.io][9])

---

### 🌐 External Sync Frameworks Supporting SQLite

#### 7. [SymmetricDS](https://github.com/JumpMind/symmetric-ds)

A **general-purpose database sync platform** supporting SQLite among many others. Provides **multi-master, filtered, and scheduled synchronization** across unreliable networks. Requires Java or server setup. ([Wikipedia][10])

---

### 🧠 Community Insights & Hybrid Approaches

* SQLite’s official **“session” extension** can track change sets and be integrated with CRDT‑based logic (e.g., cr‑sqlite) for custom sync workflows. Reddit developers have successfully combined them:

  > “The trick with the session extension is to only track one table ‘changes’ and sync that via the changesets” ([Reddit][11])

* Mycelite and Litestream/LiteFS offer **physical page-level replication**, leaving conflict resolution to the application. CRDT‑based tools (sqlite-sync, cr-sqlite) offer richer merge logic at the logical level.

---

### 📋 Comparison Overview

| Name        | Type                          | Sync Model           | Multi‑Writer?  | Install / Load                         |
| ----------- | ----------------------------- | -------------------- | -------------- | -------------------------------------- |
| Mycelite    | VFS Extension                 | Physical, page‑level | ❌ Only single  | `.load mycelite` or via VFS wrapper    |
| sqlite‑sync | CRDT Extension                | Logical row‑level    | ✅ Multi‑master | `.load sqlite-sync` + SQLiteCloud libs |
| cr‑sqlite   | CRDT Table Extension          | Logical row‑level    | ✅ Multi‑master | Load cr‑sqlite extension               |
| rqlite      | HTTP‑based distributed SQLite | Raft consensus       | ✅ Multi‑master | Use rqlite CLI / HTTP API              |
| LiteSync    | Library fork                  | Logical & paged diff | ✅ Multi‑master | Use custom SQLite URI for nodes        |
| Litestream  | External WAL streaming tool   | Physical, archival   | ❌ Master only  | Separate process monitoring WAL        |
| SymmetricDS | External Java sync framework  | Tabular sync         | ✅ Multi‑master | Run as service; configure SQLite nodes |

---

### ✅ Quick Recommendation

* For **offline-capable, conflict-free synchronization** across devices: go with **sqlite‑sync** or **cr‑sqlite**.
* For simple **read-replica backup and point-in-time restores**: consider **Litestream**.
* For **distributed, transactionally consistent clusters with high availability** via HTTP: try **rqlite**.
* To build a custom sync workflow with direct SQLite schema control: use **session extension** plus **cr‑sqlite** or analog.

---

[1]: https://github.com/mycelial/mycelite?utm_source=chatgpt.com "Mycelite is a SQLite extension that allows you to synchronize ..."
[2]: https://news.ycombinator.com/item?id=36475081&utm_source=chatgpt.com "Mycelite: SQLite extension to synchronize changes ... - Hacker News"
[3]: https://github.com/sqliteai/sqlite-sync?utm_source=chatgpt.com "sqliteai/sqlite-sync - GitHub"
[4]: https://docs.sqlitecloud.io/docs/sqlite-sync?utm_source=chatgpt.com "SQLite-Sync | SQLite Cloud Docs"
[5]: https://news.ycombinator.com/item?id=34265261&utm_source=chatgpt.com "LiteSync – Easy synchronization of SQLite databases | Hacker News"
[6]: https://sqlsync.dev/posts/stop-syncing-everything/?utm_source=chatgpt.com "Stop syncing everything - SQLSync"
[7]: https://sqlite.org/forum/info/4fc617ecaf47d544ea8baae0930553a3a52e63067d3adfff9ce1a217cc470aba?utm_source=chatgpt.com "Master-Slave replication for sqlite3? - SQLite User Forum"
[8]: https://litesync.io/en/?utm_source=chatgpt.com "LiteSync - SQLite Replication and Synchronization"
[9]: https://litestream.io/?utm_source=chatgpt.com "Litestream - Streaming SQLite Replication"
[10]: https://en.wikipedia.org/wiki/SymmetricDS?utm_source=chatgpt.com "SymmetricDS"
[11]: https://www.reddit.com/r/sqlite/comments/1jay572/sqlite_session_extension_crdt/?utm_source=chatgpt.com "SQLite Session Extension + CRDT : r/sqlite - Reddit"

## How to create isolated Copy-on-Write branches - by Gemini

You can create isolated, copy-on-write (COW) branches for an SQLite database using **LiteFS**.

SQLite doesn't have this feature built-in. LiteFS is a FUSE-based file system that exposes this functionality, allowing you to create cheap, instantaneous branches of your database, much like you would with Git.

-----

### Using LiteFS for Branching

LiteFS works by intercepting file system calls to your SQLite database. When you create a branch, it doesn't duplicate the entire database. Instead, it creates a new layer that only stores the *changes* (new pages). Reads for unchanged data are served from the original database.

#### How It Works

1.  **Mount**: You run LiteFS and point it to a directory. It creates a "mount point" where your databases will be managed.
2.  **Branch**: You issue a command to create a branch. LiteFS creates a new virtual database file for that branch.
3.  **Write**: When you write to the branched database, LiteFS intercepts the write. It copies the original page, applies the change, and saves this new page to the branch's unique storage. This is the **copy-on-write** action.
4.  **Read**: When you read from the branch, LiteFS first checks if a modified page exists in the branch's storage. If so, it returns it. If not, it reads the original, unmodified page from the main database.

-----

### Step-by-Step Example

Here's how you'd typically use the `litefs` command-line tool.

1.  **Mount the Database Directory**

    First, you mount the directory containing your database using the `litefs mount` command.

    ```bash
    # Mounts the contents of './data' to the './mnt' directory
    litefs mount ./mnt -config ./litefs.yml
    ```

2.  **Create a Branch**

    Use the `litefs branch create` command to create a new branch. This operation is instant.

    ```bash
    # Create a branch named 'testing' for the 'my_app.db' database
    litefs branch create my_app.db testing
    ```

3.  **Access the Branched Database**

    LiteFS exposes branches as regular files in a special `.branches` directory within your mount point. You can now connect to this database file and your changes will be completely isolated to the `testing` branch.

    Your application would connect to: `./mnt/.branches/testing/my_app.db`

4.  **Merge Changes (Optional)**

    After you're done with your work on the branch, you can merge the changes back into the main database.

    ```bash
    # Merge the 'testing' branch back into the main my_app.db
    litefs branch merge my_app.db testing
    ```