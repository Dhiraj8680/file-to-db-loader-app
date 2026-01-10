# CSV to PostgreSQL Loader

A high-performance Python script for loading CSV files into PostgreSQL databases with built-in optimizations for handling large datasets.

## Performance Optimizations

### 1. Parallel Processing
- **Multiprocessing**: Utilizes Python's `multiprocessing.Pool` to process multiple datasets concurrently
- **Dynamic worker allocation**: Automatically scales worker processes (max 4) based on the number of datasets
- Enables simultaneous loading of multiple tables, significantly reducing overall processing time

### 2. Chunked Reading
- **Memory efficiency**: Processes CSV files in 10,000-row chunks using pandas `chunksize` parameter
- Prevents memory overflow when handling large files
- Allows processing of files larger than available RAM

### 3. Batch Inserts
- **Multi-row inserts**: Uses `method='multi'` in `to_sql()` for batch insertion
- Reduces database round trips by inserting multiple rows per transaction
- Significantly faster than row-by-row insertion

### 4. Append Mode
- **Incremental loading**: Uses `if_exists='append'` to add data without recreating tables
- Enables resume capability if process fails mid-execution
- Supports processing files in parts without data loss

## Requirements

```bash
pip install pandas python-dotenv sqlalchemy psycopg2-binary
```

## Configuration

Create a `.env` file:

```
SRC_BASE_DIR=/path/to/data
DB_HOST=localhost
DB_PORT=5432
DB_NAME=your_database
DB_USER=your_username
DB_PASS=your_password
```

## Usage

Load all datasets:
```bash
python app.py
```

Load specific datasets:
```bash
python app.py '["dataset1", "dataset2"]'
```

## Directory Structure

```
SRC_BASE_DIR/
├── schemas.json
└── dataset_name/
    ├── part-001
    ├── part-002
    └── ...
```