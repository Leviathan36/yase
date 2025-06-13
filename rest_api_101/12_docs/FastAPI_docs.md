# Documentation | Pdoc + Swagger + ReDoc

Documentation is one of the most important parts of a project, especially for API development where the end-user has to interface with an endpoint (function) they know nothing about and don't have the source code to study its behavior.

To maximize results with minimal effort, I suggest using these three tools for creating documentation in a FastAPI project:

- **ReDoc**: for endpoint documentation. This is the documentation to recommend to the client (already included in FastAPI).
- **Swagger UI**: for debugging and testing endpoints (for both developers and end-users) (already included in FastAPI).
- **Pdoc**: a tool for generating docs. Useful for developers and the internal team. Use it for documenting the technical specifications of both endpoints and private utility functions.

## ReDoc

You can find the ReDoc documentation once the application is deployed at the following address:

```
http://<your_domain>/redoc
# If deployed locally: http://localhost:8000/redoc (assuming default FastAPI port)
```

## Swagger UI

The same applies to Swagger UI:

```
http://<your_domain>/docs
# If deployed locally: http://localhost:8000/docs (assuming default FastAPI port)
```

## Pdoc

```bash
poetry add pdoc # If using Poetry
# Or: pip install pdoc
pdoc -o <output_dir> <target_module>
```

My advice is to create a `docs` folder within the project root and build the documentation for the entire app there.

Even if the resulting documentation is quite extensive and verbose, it doesn't matter because Pdoc produces easily navigable documentation. The fact that it's complete (covering the whole app) will allow any programmer to fully understand the application and, if necessary, look up a function that isn't their direct responsibility if they need to.

So, with this simple command, we're all set:

```bash
pdoc -o docs app
```
(This assumes your main application code is in a directory named `app`.)

### Why Pdoc instead of Sphinx?

Pdoc was chosen over Sphinx, even though Sphinx is much more well-known, popular, and probably more widely used, because Pdoc is much simpler. For projects of this size, it's likely the better choice. It allows you to focus solely on writing code; it installs and runs with a simple command. It creates a few well-organized files—maximum output for minimum effort.

## Documenting API Endpoints

1.  **Inline Documentation**
    * **Recommendation**: maintain up-to-date documentation (e.g., with OpenAPI/Swagger which FastAPI generates from your code and docstrings) that precisely defines each endpoint’s usage, parameters, and response schema.
    * **Reasoning**: properly documented endpoints prevent misunderstandings and reduce maintenance overhead.
2.  **Examples and Edge Cases**
    * **Recommendation**: provide request/response examples, including edge cases (like pagination with no results, or various error states).
    * **Reasoning**: realistic examples accelerate consumption, troubleshooting, and adoption by users of your API.
