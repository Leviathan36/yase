# Auth in FastAPI

Of course, FastAPI also implements several plug-and-play authentication methods. Then, if we want to write our own authentication function, obviously, we can do that, but it's highly discouraged for obvious reasons.

[OAuth2 with Password (and hashing), Bearer with JWT tokens - FastAPI](https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/#advanced-usage-with-scopes)

Let's look at the minimum necessary steps to implement JWT-based authentication.

First of all, what is JWT?

> JWT (JSON Web Token) based authentication is a stateless method where, after login, the server issues a digitally signed token containing information (claims) about the user. The client sends this token with each subsequent request in the `Authorization` header. The server verifies the token's signature and validity (e.g., expiration) to authenticate the user without needing to consult a session state on the server.
>

So, we would need these 2 functions at a minimum:

- **create_access_token**: to create the encrypted token with the user information passed as input. This token, as mentioned above, will be used by the server to verify that the user is authenticated and authorized to perform operations. Indeed, since the token is encrypted, it can only be forged by the server, and any attempt at tampering will invalidate the token. In essence, if the server receives a valid token, by decrypting it, it will be able to find the information of the user to whom this token is associated and verify if they can indeed perform the requested operation. Obviously, if this token is stolen, the user who obtains it can impersonate the user from whom it was stolen, and the server will not be able to detect this, allowing them to perform operations that the legitimate user (from whom the token was stolen) has access to.
- **get_current_user**: this function, unlike the previous one, decrypts the token, retrieving the user information contained within it.

Let's see how to implement them:

```python
import jwt
from datetime import datetime, timedelta

# The key should be created with this command:
# openssl genrsa -out ./private.key 4096
# This one is just an example to keep the code simple
SECRET_KEY = "your-secret-key"
ALGORITHM = "HS256"

def create_access_token(data: dict, expires_delta: timedelta = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=12000) # Default to a long expiration if not provided
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt
```

```python
from fastapi import Depends, HTTPException
from fastapi.security import OAuth2PasswordBearer
import jwt

SECRET_KEY = "your-secret-key" # Must be the same as defined above
ALGORITHM = "HS256" # Must be the same as defined above

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="login") # tokenUrl points to your login endpoint

def get_current_user(token: str = Depends(oauth2_scheme)) -> str: # Changed Depends to use the oauth2_scheme instance
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub") # It's good practice to add type hints
        if username is None:
            raise HTTPException(status_code=400, detail="Invalid token: username missing") # More specific error
        # token_data = username # This line isn't strictly necessary if you just return username
    except jwt.ExpiredSignatureError:
        raise HTTPException(status_code=401, detail="Token has expired")
    except jwt.JWTError: # Catch any other JWT related errors
        raise HTTPException(status_code=401, detail="Invalid token")
    return username # Return the username directly
```

Finally, let's put everything together and create the route to handle authentication:

```python
from fastapi import APIRouter, Depends, HTTPException
from fastapi.security import OAuth2PasswordRequestForm
from passlib.context import CryptContext
from datetime import timedelta

import sqlmodel as sm # Assuming you're using SQLModel
from sqlmodel import Session # For type hinting

from app.db.session import get_session # see sqlmodel/fastapi project section
from app.datamodel.user import User # Assuming User is your SQLModel user model

ACCESS_TOKEN_EXPIRE_MINUTES = 30

auth_router = APIRouter() # Renamed to auth_router for clarity, standard practice

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

@auth_router.post("/login") # Changed to use auth_router
async def login_for_access_token(form_data: OAuth2PasswordRequestForm = Depends(), db: Session = Depends(get_session)):
    # It's generally better to query by a unique username or email
    # Assuming form_data.username is the unique identifier for the user (e.g., email or username field)
    user_query = sm.select(User).where(User.id == form_data.username) # Assuming User.id is the username field
    user_in_db = db.exec(user_query).first() # .first() is often more appropriate for unique lookups

    if not user_in_db:
        raise HTTPException(
            status_code=400,
            detail="Incorrect username or password", # Generic message for security
        )
    
    # Assuming your User model has a 'password_hash' field
    if not pwd_context.verify(form_data.password, user_in_db.password_hash):
        raise HTTPException(
            status_code=400,
            detail="Incorrect username or password", # Generic message for security
        )

    access_token_expires = timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    access_token = create_access_token(
        data={"sub": user_in_db.id}, expires_delta=access_token_expires # Use the actual user ID or unique identifier from DB
    )
    return {"access_token": access_token, "token_type": "bearer"}
```

For completeness, here's the DB interaction part again:

```python
# pip install sqlmodel
from sqlmodel import create_engine, Session, SQLModel # Added SQLModel import

DATABASE_URL = "sqlite:///./test.db"  # You can also use other databases (PostgreSQL, MySQL, etc.)
engine = create_engine(DATABASE_URL, echo=True)

def create_db_and_tables():
    SQLModel.metadata.create_all(engine) # SQLModel should be imported

def get_session():
    with Session(engine) as session:
        yield session
```

Now, all that's left is to see how to make an API "authenticated." The step is very simple and doesn't require rewriting the API. This means that if you want, you can transform a non-authenticated API into an authenticated one very easily and quickly:

```python
import sqlmodel as sm # Assuming you are using SQLModel, 'sa' usually refers to SQLAlchemy core
from typing import Any # For type hinting

from app.db.session import get_session
from app.datamodel.order import Order # Assuming Order is your SQLModel order model
**from app.auth import get_current_user** # Assuming your auth functions are in app.auth

# Assuming 'app' is your FastAPI instance or an APIRouter
# from fastapi import APIRouter
# app = APIRouter() # Or your FastAPI app instance

@app.get("/order", response_model=Order) # Ensure 'app' is defined (FastAPI instance or APIRouter)
async def get_order_detail(
    id: int,
    **current_user_id: str = Depends(get_current_user)**, # Renamed for clarity that it's the user_id from token
    db: Session = Depends(get_session)
) -> Any: # Or more specifically -> Order
    # Assuming Order model has a user_id field to associate orders with users
    query_select_order_by_id = sm.select(Order).where(sm.and_(Order.id == id, Order.user_id == current_user_id))
    order = db.exec(query_select_order_by_id).first()  # 'order' is a more conventional name for the result

    if order:
        return order
    else:
        # It's good practice to use FastAPI's HTTPException here
        raise HTTPException(status_code=404, detail=f"No order found with id: {id} for the current user.")
        # raise_error(404, f"No asset found with id: {id}.") # Assuming raise_error is a custom function
```

If the JWT token is not valid, the `get_current_user` function itself will raise an exception. The user's privilege check, i.e., which orders they can actually access, is instead done directly through the query in the code.