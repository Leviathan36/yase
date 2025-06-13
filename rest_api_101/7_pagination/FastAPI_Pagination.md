# FastAPI Pagination

**Pagination** in APIs is an essential technique used when an endpoint might return a large number of results (for example, a list of products, users, or posts). Instead of sending all the data in a single, potentially huge and slow response, the API divides the complete result set into smaller, more manageable "pages."

The client can then request these pages one at a time, typically by specifying parameters like the desired page number and the page size (how many items per page). This approach significantly improves performance, reduces the load on both the server and the client, and makes data transfer over the network more efficient.

It can be implemented manually, quite simply, by adding two additional parameters to each API:

- page
- size

*For more granular control over the results, you can use the keywords `offset` and `limit`. These can, hypothetically, return a batch of records starting from any position (offset).*

These two parameters (whose names are standard) will then be used to construct the database query like this:

```sql
SELECT * FROM people ORDER BY first_name, id LIMIT <size> OFFSET <page*size>
```

Obviously, there are also faster, "pre-packaged" methods for implementing pagination, especially if you're using FastAPI. Specifically, let's look at the `fastapi-pagination` package ([https://github.com/uriyyo/fastapi-pagination](https://github.com/uriyyo/fastapi-pagination)).

This package allows you to wrap your entire application (`app`) so that all routes implicitly implement pagination without needing to modify their function signatures; in fact, the `size` and `page` parameters are added by the package itself. This feature can be very useful if you want to add pagination to an existing application.

Then, you just need to add the final line where you call the `paginate` function, which automatically handles the pagination:

```python
    return paginate(db, select(User).order_by(User.created_at))
```

ATTENTION: The `paginate` function depends on the type of DB/ORM being used, so make sure to import the correct function based on the database you are using. For example, for SQLAlchemy, you need to import:

```python
from fastapi_pagination.ext.sqlalchemy import paginate
```

Let's quickly see how to use this package:

- Installation:

```python
pip install fastapi-pagination
```

- Wrapping the app:

```python
from fastapi import FastAPI

# import all you need from fastapi-pagination
from fastapi_pagination import Page, add_pagination, paginate

app = FastAPI()  # create FastAPI app
add_pagination(app)  # important! add pagination to your app
```

- Return "paginate" function result:

```python
from sqlalchemy import select
from fastapi_pagination.ext.sqlmodel import paginate # change this to be consistent with your DB

@app.get("/users")
def get_users(db: Session = Depends(get_db)) -> **Page[UserOut]**:
    return **paginate**(db, select(User).order_by(User.created_at)) # change the query based on your requirements
```
