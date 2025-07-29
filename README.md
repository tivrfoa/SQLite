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

## How SQLite is tested

From: [https://www.sqlite.org/testing.html](https://www.sqlite.org/testing.html)

The reliability and robustness of SQLite is achieved in part by thorough and careful testing.

As of version 3.42.0 (2023-05-16), the SQLite library consists of approximately 155.8 KSLOC of C code. (KSLOC means thousands of "Source Lines Of Code" or, in other words, lines of code excluding blank lines and comments.) By comparison, the project has 590 times as much test code and test scripts - 92053.1 KSLOC.

---

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
