# 5xx Errors

Use [CORSMiddleware](https://fastapi.tiangolo.com/tutorial/cors/)
You can configure it in your FastAPI application using the CORSMiddleware.

- Import `CORSMiddleware`.
- Create a list of allowed origins (as strings).
- Add it as a "middleware" to your FastAPI application.

You can also specify if your backend allows:

- Credentials (Authorization headers, Cookies, etc).
- Specific HTTP methods (`POST`, `PUT`) or all of them with the wildcard `"*"`.
- Specific HTTP headers or all of them with the wildcard `"*"`.

```python
from fastapi.middleware.cors import CORSMiddleware

origins = [
    "http://localhost.tiangolo.com",
    "https://localhost.tiangolo.com",
    "http://localhost",
    "http://localhost:8080",
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

If a 5xx error code is returned, the middleware doesn't get to insert their headers so the request fails to materialize in the frontend. You want to create a separate Session for each request; you do this by using a `Depends` in your view together with a function to create a local Session. Instead of having a global session.

We need to have an independent database session/connection [`SessionLocal`](https://fastapi.tiangolo.com/tutorial/sql-databases/#create-a-sessionlocal-class) per request, use the same session through all the request and then close it after the request is finished.

And then a new session will be created for the next request.

For that, we will create a new dependency with yield, as explained before in the section about [Dependencies with yield](https://fastapi.tiangolo.com/tutorial/dependencies/dependencies-with-yield/)

The dependency will create a new SQLAlchemy SessionLocal that will be used in a single request, and then close it once the request is finished.

```python
from fastapi import Depends, FastAPI, HTTPException

app = FastAPI()


# Dependency
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# we then use the session like so using Depends
@app.get('/some-endpoint')
def get_data(db: Session = Depends(get_db))
    db.query(...)

    return {"message": " ... "}

```

Setting it up this way will give each request to our API it's own session, which in turn should prevent 5xx errors.