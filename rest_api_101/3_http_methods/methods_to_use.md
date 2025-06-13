# Which methods to use (get/post/put...)

Unlike other types of APIs, such as RPC APIs, REST APIs use standard HTTP methods to indicate the type of operation to be performed on a resource, data, database entity, or code object. For example, if we want to add a new record to the "orders" table in the database, we would code a function that, upon a `POST /v1/order` request, adds a new order with that ID to the database (along with any other parameters in the body of the POST request).

Why did we use the POST method specifically, and not, say, PUT?

Well, each HTTP method, or at least the main ones, has a specific meaning in the context of REST APIs. Let's briefly look at which methods to use based on the type of operation we want to perform on resources.

***PREMISE***: an operation (like an HTTP request) is **idempotent** if performing it multiple times produces exactly the same end result as performing it just once.

Basically: it doesn't matter if you make the same request once or 10 times in a row; the system's state will be identical to the state after the first successful execution.

In the context of HTTP methods (used in REST APIs), a '**safe**' operation or method is defined as one that **is not intended to change the state of the resource on the server**.

-   **GET**
    -   **usage:** retrieve a representation of a resource or a collection of resources.
    -   **target:** specific resource uri (e.g., `/users/123`) or collection uri (e.g., `/users`).
    -   **properties:** safe, idempotent.
    -   **body:** request body ignored; response body contains the representation.
-   **POST**
    -   **usage:** create a new resource within a collection (server assigns uri), or trigger a specific process/action on a resource. sometimes used as a default method if it's not clear which other method to use, such as for triggering a process on the server. these are rare cases; generally, there's a specific method for each use case.
    -   **target:** usually a collection uri (e.g., `/users`) or a specialized action uri (e.g., `/tasks/123/start`).
    -   **properties:** not safe, not idempotent.
    -   **body:** request body contains data for the new resource or action parameters; response body may contain the newly created resource or status information.
-   **PUT**
    -   **usage:** replace an existing resource entirely with the provided representation, or create a resource at a specific, client-defined uri if it doesn't exist. the difference between put and post is that if you know the resource's id, meaning you're targeting a specific resource, then you use put. otherwise, you use post. in fact, put is idempotent, so executing it multiple times will always have the same effect (if the resource was created, it won't be recreated). in contrast, post is used when targeting a generic resource, without specifying the id for example, which means executing it multiple times will change the db state, add a new record, or whatnot.
    -   **target:** specific resource uri (e.g., `/users/123`).
    -   **properties:** not safe, idempotent.
    -   **body:** request body contains the *full* new representation of the resource; response body often empty or contains status information.
-   **DELETE**
    -   **usage:** remove/delete a specific resource.
    -   **target:** specific resource uri (e.g., `/users/123`).
    -   **properties:** not safe, idempotent.
    -   **body:** request body usually ignored; response body often empty or contains status information.
-   **PATCH**
    -   **usage:** apply *partial* modifications to an existing resource. describes the changes to be made.
    -   **target:** specific resource uri (e.g., `/users/123`).
    -   **properties:** not safe, *not necessarily* idempotent (depends on the patch semantics).
    -   **body:** request body contains instructions describing the partial update (e.g., json patch, json merge patch); response body may contain the modified resource or status information.
-   **HEAD**
    -   **usage:** retrieve only the headers associated with a resource (identical to get, but without the response body). useful for checking metadata (e.g., `content-type`, `last-modified`) without downloading the full content.
    -   **target:** specific resource uri or collection uri.
    -   **properties:** safe, idempotent.
    -   **body:** request body ignored; response body is empty.
-   **OPTIONS**
    -   **usage:** discover the communication options (e.g., allowed http methods, cors details) available for a target resource or the server itself.
    -   **target:** specific resource uri, collection uri, or server (`*`).
    -   **properties:** safe, idempotent.
    -   **body:** request body usually ignored; response headers (e.g., `allow`, `access-control-allow-methods`) contain the options.