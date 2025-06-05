# CrabEvenOdd Backend

## Overview
Backend service for the CrabEvenOdd application, providing API endpoints for game logic, user management, and data processing.

## Features
- User authentication and authorization
- Game state management
- Real-time data processing
- Statistical analysis of game outcomes
- Admin dashboard APIs
- Payment integration

## Tech Stack
- Python (specify version)
- Framework: (e.g., Flask, Django, FastAPI)
- Database: (e.g., PostgreSQL, MongoDB)
- Cache: (e.g., Redis)
- Task Queue: (e.g., Celery)

## Prerequisites
- Python 3.8+
- pip
- virtualenv (recommended)
- Docker (optional, for containerized deployment)

## Installation

### Local Development Setup
1. Clone the repository
   ```bash
   git clone https://github.com/BiswasSekhar/crabevenoddbackend.git
   cd crabevenoddbackend
   ```

2. Create and activate a virtual environment
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies
   ```bash
   pip install -r requirements.txt
   ```

4. Set up environment variables
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

5. Run database migrations
   ```bash
   # Depending on your framework, e.g. for Django:
   python manage.py migrate
   ```

6. Start the development server
   ```bash
   # Depending on your framework, e.g. for Django:
   python manage.py runserver
   # or for Flask:
   flask run
   ```

### Docker Setup
```bash
docker-compose up -d
```

## API Documentation

### Authentication
- POST `/api/auth/register` - Register a new user
- POST `/api/auth/login` - Login user
- POST `/api/auth/logout` - Logout user
- GET `/api/auth/profile` - Get user profile

### Game Operations
- GET `/api/games` - List all games
- POST `/api/games` - Create a new game
- GET `/api/games/{id}` - Get game details
- PUT `/api/games/{id}` - Update game
- DELETE `/api/games/{id}` - Delete game

### Admin Operations
- GET `/api/admin/users` - List all users
- GET `/api/admin/stats` - Get platform statistics

## Testing
```bash
# Run tests
pytest

# Run with coverage
pytest --cov=app
```

## Deployment
Instructions for deploying to production environments:

1. Set up production environment variables
2. Configure web server (Nginx, Apache)
3. Set up SSL certificates
4. Configure database for production
5. Set up monitoring and logging

## Contributing
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License
This project is licensed under the [MIT License](LICENSE).
