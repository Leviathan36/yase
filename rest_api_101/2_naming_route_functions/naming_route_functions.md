# Naming Functions Related to Routes

The most common practice, often seen in FastAPI's official documentation, is to use a **`verb_resource`** or **`action_resource`** pattern, where:

1.  **`verb` / `action`**: Describes the operation performed by the route, often aligned with CRUD verbs (Create, Read, Update, Delete) or specific actions.
    * **`read_`**: for `GET` operations (reading one or more items).
    * **`create_`**: for `POST` operations (creating a new item).
    * **`update_`**: for `PUT` or `PATCH` operations (updating an existing item).
    * **`delete_`** (or `remove_`): for `DELETE` operations (deleting an item).
    * Other descriptive verbs for specific actions (e.g., `search_`, `upload_`, `login_`, `process_`).
2.  **`resource`**: describes the entity or resource the route operates on (generally singular for operations on a single item, plural for operations on collections).
    * `item`, `items`
    * `user`, `users`
    * `order`, `orders`
    * `product`, `products`

**Practical Examples:**

* **`GET /items/`**: reads a collection of items.
    * Recommended function name: `read_items`
* **`GET /items/{item_id}`**: reads a single specific item.
    * Recommended function name: `read_item`
* **`POST /items/`**: creates a new item.
    * Recommended function name: `create_item`
* **`PUT /items/{item_id}`**: completely updates an item.
    * Recommended function name: `update_item`
* **`PATCH /items/{item_id}`**: partially updates an item.
    * Recommended function name: `patch_item` (or sometimes `update_item` is also used for PATCH)
* **`DELETE /items/{item_id}`**: deletes an item.
    * Recommended function name: `delete_item`
* **`POST /users/{user_id}/avatar`**: uploads an avatar for a specific user.
    * Recommended function name: `upload_user_avatar`
* **`POST /token`**: generates an access token (login).
    * Recommended function name: `login_for_access_token` or `create_access_token`
* **`GET /products/search`**: searches for products.
    * Recommended function name: `search_products`

**Additional rules and good practices:**

* **PEP 8:** always follow Python's PEP 8 style conventions, so use `snake_case` (lowercase words separated by underscores) for function names.
* **Clarity:** the name should immediately make the function's purpose clear in relation to its route. If you see `def read_item(...)`, it should be obvious that it handles a request to read a single item.
* **Consistency:** the most important thing is to choose *one* convention (preferably the one described above) and apply it *consistently* throughout the project. This makes the code predictable and easy for you and other developers to navigate.
* **Avoid Generic Names:** names like `handle_request`, `process_data`, or `endpoint1` are too generic and should be avoided.

**In conclusion:** the "rule" to follow is the **`verb_action_resource` convention in `snake_case`**, applied with **consistency**, because it maximizes the readability and maintainability of your FastAPI code.
