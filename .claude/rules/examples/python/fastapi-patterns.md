# FastAPI Patterns (Python)

## Pydantic for All Request/Response Models

Never use raw dicts for request bodies or response shapes. All data flowing in and out must be typed Pydantic models.

```python
# WRONG
@app.post("/users")
async def create_user(data: dict):  # No validation, no docs
    ...

# CORRECT
class CreateUserRequest(BaseModel):
    email: EmailStr
    name: str = Field(..., min_length=1, max_length=100)
    role: Literal["admin", "user"] = "user"

class UserResponse(BaseModel):
    id: int
    email: str
    name: str
    created_at: datetime

    class Config:
        from_attributes = True  # ORM mode

@app.post("/users", response_model=UserResponse, status_code=201)
async def create_user(data: CreateUserRequest, db: Session = Depends(get_db)):
    user = user_service.create(db, data)
    return user
```

## Router Organization

```python
# routers/users.py
router = APIRouter(prefix="/users", tags=["users"])

@router.get("/", response_model=list[UserResponse])
async def list_users(...): ...

@router.post("/", response_model=UserResponse, status_code=201)
async def create_user(...): ...

# main.py
app.include_router(users.router)
app.include_router(orders.router)
```

## Dependency Injection

```python
# dependencies.py
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

async def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: Session = Depends(get_db)
) -> User:
    user = auth_service.verify_token(db, token)
    if not user:
        raise HTTPException(status_code=401, detail="Invalid token")
    return user

# Usage
@router.get("/me", response_model=UserResponse)
async def get_me(current_user: User = Depends(get_current_user)):
    return current_user
```

## Async Endpoints

Use `async def` for all I/O-bound endpoints. Use `def` (sync) only for CPU-bound operations:

```python
# I/O bound — use async
@router.get("/users/{id}")
async def get_user(id: int, db: AsyncSession = Depends(get_async_db)):
    user = await user_service.get_by_id(db, id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

## Error Handling

```python
# Use HTTPException for expected errors
raise HTTPException(status_code=404, detail="Order not found")
raise HTTPException(status_code=403, detail="Insufficient permissions")

# Use exception handlers for unexpected errors
@app.exception_handler(Exception)
async def generic_exception_handler(request, exc):
    logger.error("Unhandled error", exc_info=exc)
    return JSONResponse(status_code=500, content={"detail": "Internal server error"})
```
