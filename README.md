# MLflow Training Exercises

This repository contains MLflow training exercises in Jupyter notebooks.

## Prerequisites

- **Option 1 (Docker)**: Docker and Docker Compose installed
- **Option 2 (uv)**: [uv](https://github.com/astral-sh/uv) package manager and Python 3.12

## Setup and Launch

### Option A: Using Docker Compose (Recommended)

This method launches everything in containers:
- JupyterLab with all Python dependencies
- MLflow server connected to MinIO
- MinIO (local S3-compatible storage)
- MySQL (MLflow backend store)
- phpMyAdmin (MySQL administration interface)

1. **Start all services:**
   ```bash
   docker-compose up
   ```
Note: if needed, remove the previous existing volume with previous mysql database version:
   ```bash
   docker volume rm mlflow_cookbook_mysql_data
   ```

2. **Access the services:**
   - JupyterLab: http://localhost:8888
   - MLflow UI: http://localhost:5000
   - MinIO Console: http://localhost:9001 (username: `ROOTNAME`, password: `CHANGEME123`)
   - phpMyAdmin: http://localhost:8080 (username: `mlflow`, password: `mlflow`)

3. **Stop all services:**
   ```bash
   docker-compose down
   ```

### Option B: Using uv

This method runs JupyterLab locally using uv for dependency management.

1. **Install uv** (if not already installed):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Create and install the Python environment:**
   ```bash
   uv sync
   ```

3. **Start JupyterLab:**
   ```bash
   uv run jupyter lab
   ```

5. **Configure MLflow connection:**
   - Make sure you have a local MLflow server running, or
   - Set the `MLFLOW_TRACKING_URI` environment variable to point to your MLflow server
   - If using MinIO locally, set `MLFLOW_S3_ENDPOINT_URL` and AWS credentials

## Project Structure

- `mlflow_basics.ipynb`: Basic MLflow concepts
- `mlflow_hyperparameter_tuning.ipynb`: MNIST example with MLflow
- `mlflow_s3.ipynb`: MLflow with S3 storage

## Environment Variables (for uv method)

When using the uv method, you may need to set these environment variables:

```bash
export MLFLOW_TRACKING_URI=http://localhost:5000
export MLFLOW_S3_ENDPOINT_URL=http://localhost:9000
export AWS_ACCESS_KEY_ID=ROOTNAME
export AWS_SECRET_ACCESS_KEY=CHANGEME123
```

## Connecting to MySQL via phpMyAdmin

After starting the services, you can access phpMyAdmin at http://localhost:8080:

1. **Login to phpMyAdmin:**
   - Username: `mlflow`
   - Password: `mlflow`

2. **The MySQL connection is automatically configured!**
   - The `mlflow` database should be visible in the left sidebar
   - You can browse the MLflow database schema and tables directly
   - You can also use the root account: username `root`, password `rootpassword`

## Notes

- The Docker Compose setup automatically configures all connections between services
- MinIO credentials: `ROOTNAME` / `CHANGEME123`
- MLflow artifacts are stored in the `s3://mlflow-artifacts/` bucket in MinIO
- MySQL credentials: `mlflow` / `mlflow` (database: `mlflow`, port: `3306`)
- MySQL root credentials: `root` / `rootpassword`
