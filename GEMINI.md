# GEMINI.md - RediSolar Python

## Project Overview
RediSolar Python is a sample application for the [RU102PY: Redis for Python Developers](https://university.redis.io/learningpath/ikoq5va7id3qko) course. It's a solar energy monitoring system that demonstrates how to build a real-world application using Redis as its primary data store.

The application manages solar sites, tracks power generation metrics in real-time, and provides capacity reporting and status updates.

## Tech Stack
- **Backend:** Python 3.8, Flask, Flask-RESTful
- **Database:** Redis (requires RedisTimeSeries module)
- **Frontend:** Vue.js (located in `/frontend`)
- **Testing:** Pytest
- **Static Analysis:** MyPy (typing), Pylint (linting)
- **Dependency Management:** pip-tools (`requirements.in` / `requirements.txt`)

## Building and Running

### Prerequisites
- Python 3.8
- Redis 5+ with RedisTimeSeries module

### Commands (via Makefile)
- `make env`: Sets up the virtual environment and installs all dependencies.
- `make timeseries-docker`: Starts a Redis container with the required RedisTimeSeries module.
- `make load`: Loads sample solar site data and generates example readings in Redis.
- `make dev`: Starts the Flask development server at `http://localhost:8081`.
- `make test`: Runs the complete test suite.
- `make lint`: Runs Pylint for code quality checks.
- `make mypy`: Runs MyPy for static type checking.
- `make frontend`: Builds the Vue.js frontend and syncs assets to the backend static directory.

## Architecture and Key Files

### Backend (`/redisolar`)
- `api/`: RESTful API endpoints and Flask blueprints.
- `dao/`: Data Access Objects. This is the core of the project where Redis interactions are implemented.
    - `dao/redis/`: Redis-specific DAO implementations.
    - `dao/redis/key_schema.py`: Centralized management of Redis key patterns and prefixes.
- `models/`: Python dataclasses representing the domain entities (Site, MeterReading, etc.).
- `schema.py`: Marshmallow schemas for serialization/deserialization.
- `scripts/`: Lua scripts for complex, atomic Redis operations.
- `command/load.py`: CLI command for populating the database.

### Frontend (`/frontend`)
- A Vue.js application that provides a dashboard for visualizing solar site data and metrics.

## Development Conventions
- **DAO Pattern:** Always use or extend the DAO classes in `redisolar.dao` for database interactions to maintain a clean separation between business logic and the data layer.
- **Key Management:** Never hardcode Redis keys. Use the `KeySchema` class to generate prefixed keys.
- **Challenges:** The codebase contains "Challenge" markers (e.g., `START Challenge #1`) indicating where students are expected to implement core Redis functionality.
- **Configuration:** Environment-specific settings are stored in `redisolar/instance/*.cfg`. Local overrides can be placed in a `.env` file.
- **Type Safety:** The project uses type hints extensively; run `make mypy` to verify type correctness before submitting changes.
