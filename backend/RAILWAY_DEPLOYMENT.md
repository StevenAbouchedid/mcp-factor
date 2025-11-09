# Railway Deployment Guide

## Files Created for Railway Deployment

1. **Procfile** - Tells Railway how to start the app
2. **railway.json** - Railway configuration (build and deploy settings)
3. **requirements.txt** - Python dependencies (fallback if uv detection fails)
4. **pyproject.toml** - Updated with `uvicorn` dependency

## Railway Configuration Steps

### 1. Set the Root Directory
In Railway dashboard:
- Go to your service settings
- Set **Root Directory** to: `backend`

### 2. Set Environment Variables
Add these environment variables in Railway:
- `PORT` - Railway sets this automatically
- `PYTHONPATH` - Set to `/app/src`
- Any other env vars from your `.env` file (OpenAI API key, etc.)

### 3. Railway Should Auto-detect
Railway will automatically:
- Detect Python project from `pyproject.toml` or `requirements.txt`
- Install dependencies using pip or uv
- Use the `Procfile` to start the server

### 4. Deploy Command (Alternative)
If the Procfile doesn't work, set this custom start command in Railway:
```bash
uvicorn src.dynamic_tools.api.app:app --host 0.0.0.0 --port $PORT
```

## Troubleshooting

### Error: "uvicorn could not be found"
✅ Fixed! Added `uvicorn>=0.27.0` to dependencies

### Error: Module not found
- Make sure Root Directory is set to `backend`
- Add `PYTHONPATH=/app/src` environment variable

### Error: Port binding
- Railway automatically provides `$PORT` environment variable
- The Procfile uses `$PORT` to bind correctly

## Testing Locally with UV

```bash
cd backend
uv sync
uv run uvicorn src.dynamic_tools.api.app:app --host 0.0.0.0 --port 8000 --reload
```

## Testing Locally with Docker

```bash
cd backend
docker-compose up
```

## Monitoring

Once deployed, you can:
- View logs in Railway dashboard
- Access the API at your Railway URL
- Check `/docs` endpoint for FastAPI Swagger documentation

