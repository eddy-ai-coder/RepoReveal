# In-Depth Guide: FastAPI

## Getting Started

### Prerequisites

Before using FastAPI, ensure you have:

- Python 3.7 or higher
- pip (Python package installer)
- Basic knowledge of Python type hints
- (Optional) Virtual environment tool (venv, virtualenv)

### Installation

#### Step 1: Create Virtual Environment

```bash
# Create virtual environment
python -m venv fastapi-env

# Activate it (Unix/macOS)
source fastapi-env/bin/activate

# Activate it (Windows)
fastapi-env\Scripts\activate
```

#### Step 2: Install FastAPI and Uvicorn

```bash
# Install FastAPI with all dependencies
pip install "fastapi[all]"

# Or minimal installation
pip install fastapi uvicorn
```

#### Step 3: Create Your First App

Create a file `main.py`:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```

#### Step 4: Run the Application

```bash
uvicorn main:app --reload
```

### Quick Start

Here's a minimal API with multiple endpoints:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float
    is_offer: bool = None

@app.get("/")
async def read_root():
    return {"Hello": "World"}

@app.get("/items/{item_id}")
async def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}

@app.post("/items/")
async def create_item(item: Item):
    return {"item": item, "price_with_tax": item.price * 1.2}
```

**Expected Output**:

Visit `http://localhost:8000/docs` to see interactive API documentation.

### Verification

Verify your installation:

```bash
# Check if server is running
curl http://localhost:8000

# Should return: {"Hello":"World"}
```

## Common Use Cases

### Use Case 1: CRUD API with Database

**Scenario**: Create a RESTful API for managing users with SQLite database

**Prerequisites**: `pip install sqlalchemy databases`

**Implementation**:

```python
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, Session
from pydantic import BaseModel

# Database setup
SQLALCHEMY_DATABASE_URL = "sqlite:///./users.db"
engine = create_engine(SQLALCHEMY_DATABASE_URL)
SessionLocal = sessionmaker(bind=engine)
Base = declarative_base()

# Database model
class UserDB(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String, index=True)
    email = Column(String, unique=True, index=True)

Base.metadata.create_all(bind=engine)

# Pydantic models
class UserCreate(BaseModel):
    name: str
    email: str

class User(BaseModel):
    id: int
    name: str
    email: str
    
    class Config:
        from_attributes = True

# FastAPI app
app = FastAPI()

# Dependency
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# Endpoints
@app.post("/users/", response_model=User)
def create_user(user: UserCreate, db: Session = Depends(get_db)):
    db_user = UserDB(name=user.name, email=user.email)
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user

@app.get("/users/{user_id}", response_model=User)
def read_user(user_id: int, db: Session = Depends(get_db)):
    user = db.query(UserDB).filter(UserDB.id == user_id).first()
    if user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

**Step-by-Step Explanation**:

1. **Database Setup**: Create SQLite database connection and session factory
2. **Models**: Define database model (UserDB) and API models (Pydantic)
3. **Dependency**: `get_db()` manages database sessions
4. **Endpoints**: CRUD operations with automatic validation

**Expected Output**:
```bash
# Create user
curl -X POST "http://localhost:8000/users/" \
     -H "Content-Type: application/json" \
     -d '{"name":"John","email":"john@example.com"}'

# Response: {"id":1,"name":"John","email":"john@example.com"}
```

**Common Issues**:
- **Issue**: "Table already exists" error
  **Solution**: Delete `users.db` and restart
- **Issue**: Email not unique error
  **Solution**: Use different email or add update endpoint

### Use Case 2: Authentication with JWT

**Scenario**: Add JWT-based authentication to protect endpoints

**Implementation**:

```python
from fastapi import FastAPI, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from jose import JWTError, jwt
from passlib.context import CryptContext
from datetime import datetime, timedelta
from pydantic import BaseModel

# Configuration
SECRET_KEY = "your-secret-key-keep-it-safe"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

app = FastAPI()

# Security utilities
pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

# Models
class Token(BaseModel):
    access_token: str
    token_type: str

class User(BaseModel):
    username: str
    email: str = None

# Fake database
fake_users_db = {
    "testuser": {
        "username": "testuser",
        "email": "test@example.com",
        "hashed_password": pwd_context.hash("secret")
    }
}

def create_access_token(data: dict):
    to_encode = data.copy()
    expire = datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

async def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            raise HTTPException(status_code=401, detail="Invalid token")
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")
    
    user = fake_users_db.get(username)
    if user is None:
        raise HTTPException(status_code=401, detail="User not found")
    return User(username=user["username"], email=user["email"])

@app.post("/token", response_model=Token)
async def login(form_data: OAuth2PasswordRequestForm = Depends()):
    user = fake_users_db.get(form_data.username)
    if not user or not pwd_context.verify(form_data.password, user["hashed_password"]):
        raise HTTPException(status_code=401, detail="Incorrect credentials")
    
    access_token = create_access_token(data={"sub": user["username"]})
    return {"access_token": access_token, "token_type": "bearer"}

@app.get("/users/me")
async def read_users_me(current_user: User = Depends(get_current_user)):
    return current_user
```

## Advanced Features

### Feature 1: Background Tasks

**Purpose**: Execute tasks after returning response (emails, notifications, cleanup)

**When to Use**: When you don't want the client to wait for long-running operations

**Usage Example**:

```python
from fastapi import BackgroundTasks

def write_log(message: str):
    with open("log.txt", "a") as f:
        f.write(f"{message}\n")

@app.post("/send-notification/")
async def send_notification(
    email: str,
    background_tasks: BackgroundTasks
):
    background_tasks.add_task(write_log, f"Notification sent to {email}")
    return {"message": "Notification sent in background"}
```

### Feature 2: Dependency Injection for Authentication

**Purpose**: Reuse authentication logic across endpoints

**Configuration**:

```python
from typing import Optional

async def get_current_user_optional(
    token: Optional[str] = Depends(oauth2_scheme)
) -> Optional[User]:
    if token is None:
        return None
    return await get_current_user(token)

@app.get("/items/")
async def read_items(user: Optional[User] = Depends(get_current_user_optional)):
    if user:
        return {"items": ["item1", "item2"], "user": user.username}
    return {"items": ["item1"]}  # Limited results for anonymous
```

## Best Practices

### Performance Optimization

#### ✅ Do:
- Use async endpoints for I/O-bound operations
- Implement database connection pooling
- Use response caching for expensive queries
- Enable Gzip compression middleware

#### ❌ Don't:
- Block async endpoints with synchronous operations
- Make database queries inside loops
- Return entire database tables without pagination
- Ignore proper indexing on databases

#### 💡 Tips:
- Use `async def` for database queries with async drivers
- Implement pagination with `skip` and `limit` parameters
- Use FastAPI's `BackgroundTasks` for non-critical operations

**Example - Good**:
```python
@app.get("/items/")
async def read_items(skip: int = 0, limit: int = 10, db: Session = Depends(get_db)):
    items = db.query(Item).offset(skip).limit(limit).all()
    return items
```

**Example - Bad**:
```python
@app.get("/items/")
async def read_items(db: Session = Depends(get_db)):
    # Don't return all items without pagination!
    return db.query(Item).all()
```

### Security Best Practices

1. **Always Validate Input**
   - Use Pydantic models for all request bodies
   - Validate query parameters with type hints
   
2. **Secure Secrets**
   - Never hardcode SECRET_KEY
   - Use environment variables or secret management
   - Rotate keys regularly

3. **Enable CORS Carefully**
```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://yourdomain.com"],  # Specific origins, not "*"
    allow_credentials=True,
    allow_methods=["GET", "POST"],  # Only needed methods
    allow_headers=["*"],
)
```

## Troubleshooting

### Common Errors

#### Error: `RuntimeError: no running event loop`

**Cause**: Calling async functions from synchronous code

**Solution**:
```python
# Bad - mixing sync/async incorrectly
def my_sync_endpoint():
    result = await some_async_function()  # Error!
    
# Good - use async endpoint
async def my_async_endpoint():
    result = await some_async_function()  # Correct!
```

#### Error: `422 Unprocessable Entity`

**Cause**: Request data doesn't match Pydantic model

**Solution**: Check the error details in response body
```bash
# The response will show exactly what's wrong:
{
  "detail": [
    {
      "loc": ["body", "price"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

### Debugging Tips

1. **Enable Debug Mode**:
   ```bash
   uvicorn main:app --reload --log-level debug
   ```

2. **Check OpenAPI Schema**:
   Visit `/openapi.json` to see generated schema

3. **Use Exception Handlers**:
   ```python
   @app.exception_handler(ValueError)
   async def value_error_handler(request, exc):
       return JSONResponse(
           status_code=400,
           content={"message": str(exc)}
       )
   ```

## FAQ

**Q: Should I use `def` or `async def` for my endpoints?**

A: Use `async def` when making I/O calls (database, HTTP requests, file operations). Use `def` for CPU-bound operations or when calling sync libraries.

**Q: How do I deploy FastAPI in production?**

A: Use uvicorn with multiple workers: `uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4`. Consider using Docker and a process manager like supervisord or systemd.

**Q: Can I use FastAPI with websockets?**

A: Yes! FastAPI has built-in WebSocket support:
```python
from fastapi import WebSocket

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    while True:
        data = await websocket.receive_text()
        await websocket.send_text(f"Echo: {data}")
```

**Q: How do I handle file uploads?**

A: Use `UploadFile`:
```python
from fastapi import File, UploadFile

@app.post("/upload/")
async def upload_file(file: UploadFile = File(...)):
    contents = await file.read()
    return {"filename": file.filename, "size": len(contents)}
```

**Q: How do I implement API versioning?**

A: Use router prefixes:
```python
from fastapi import APIRouter

v1_router = APIRouter(prefix="/v1")
v2_router = APIRouter(prefix="/v2")

@v1_router.get("/items/")
async def get_items_v1():
    return {"version": "1.0"}

app.include_router(v1_router)
app.include_router(v2_router)
```

## References

- [Repository Overview](./repository-overview.md)
- [Architecture](./architecture.md)
- [Technical Details](./technical-details.md)
- [Official FastAPI Documentation](https://fastapi.tiangolo.com/)
