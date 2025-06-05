# CrabEvenOdd Backend

## Overview
Backend service for the CrabEvenOdd game application built on **Frappe Framework**. This is a specialized even/odd number guessing game backend that provides API endpoints for game questions management, user authentication, and game data processing.

## Screenshots

### Question Manager Interface
![Question Manager](Question%20Manager.png)
*Admin interface for managing game questions with CRUD operations*

### Login Screen
![Login Screen](Login%20Screen.png)
*Custom login interface for game administrators*

## Project Structure
This is a **Frappe/Bench** based application with the following structure:
- **Framework**: Frappe Framework v14+ (Python-based full-stack web framework)
- **Custom App**: `crab_game` - Main application module
- **Database**: MariaDB (via Frappe)
- **Cache & Queue**: Redis (for caching and background tasks)
- **Process Management**: Supervisor/Honcho

## Features
- **Game Question Management**: CRUD operations for even/odd game questions
- **Admin Dashboard**: Question manager interface for game administrators
- **Live Questions API**: Real-time fetching of active game questions
- **User Authentication**: Role-based access control with custom redirects
- **Level-based Questions**: Questions organized by difficulty levels
- **Time-limited Questions**: Configurable time limits for each question
- **CORS Support**: Cross-origin resource sharing for frontend integration

## Tech Stack
- **Python**: 3.10+ (Frappe Framework requirement)
- **Framework**: Frappe Framework v14+
- **Database**: MariaDB
- **Cache**: Redis
- **Queue**: Redis Queue (RQ)
- **Process Manager**: Supervisor/Honcho
- **Frontend Assets**: Node.js (for building assets)

## API Endpoints

### Game Questions API
- **GET** `/api/method/crab_game.api.questions.get_live_questions` - Get all live questions
- **GET** `/api/method/crab_game.api.questions.get_questions_by_level` - Get questions by level
- **POST** `/api/method/crab_game.api.questions.update_question` - Update existing question
- **DELETE** `/api/method/crab_game.api.questions.delete_question` - Delete question

### Authentication & Management
- **Web Interface**: `/question_manager` - Admin interface for question management
- **Custom Login**: `/crab_login` - Specialized login for game managers
- **Auto-redirect**: Users with "Game Question Manager" role are automatically redirected

## Prerequisites
- **Python**: 3.10 or higher
- **Node.js**: 16+ (for building frontend assets)
- **MariaDB**: 10.3+
- **Redis**: 6.0+
- **Git**: For version control and app management

## Installation

### Using Frappe Bench (Recommended)

1. **Install Frappe Bench** (if not already installed)
   ```bash
   pip install frappe-bench
   ```

2. **Clone and setup the project**
   ```bash
   git clone <your-repo-url> crabevenodd-backend
   cd crabevenodd-backend
   ```

3. **Initialize Bench** (if starting fresh)
   ```bash
   bench init frappe-bench --frappe-branch version-14
   cd frappe-bench
   ```

4. **Get the crab_game app**
   ```bash
   bench get-app /path/to/crab_game
   # or from git:
   # bench get-app https://github.com/yourusername/crab_game.git
   ```

5. **Create a new site**
   ```bash
   bench new-site site1.local
   bench --site site1.local install-app crab_game
   ```

6. **Start the development server**
   ```bash
   bench start
   ```

### Manual Setup (Development)

1. **Navigate to existing bench**
   ```bash
   cd /path/to/your/frappe-bench
   ```

2. **Install the app**
   ```bash
   bench --site your-site install-app crab_game
   ```

3. **Migrate database**
   ```bash
   bench --site your-site migrate
   ```

4. **Build assets**
   ```bash
   bench build --app crab_game
   ```

## Development Commands

### Common Bench Commands
```bash
# Start development server
bench start

# Restart specific services
bench restart

# Run migrations
bench --site site1.local migrate

# Create new DocType
bench --site site1.local console

# Build assets
bench build

# Install app on site
bench --site site1.local install-app crab_game

# Update app
bench update --apps crab_game

# Backup site
bench --site site1.local backup

# View logs
tail -f logs/frappe.log
```

### App-specific Commands
```bash
# Create new Game Question record via console
bench --site site1.local console
> doc = frappe.get_doc({
    "doctype": "Game Question",
    "level": 1,
    "numbers": "1,2,3,4,5",
    "correct_answer": "odd",
    "time_limit": 30,
    "is_live": 1
})
> doc.insert()
```

## Configuration

### Site Configuration
The main configuration is in `sites/site1.local/site_config.json`:
```json
{
 "db_name": "your_db_name",
 "db_password": "your_password",
 "developer_mode": 1,
 "redis_cache": "redis://localhost:13000",
 "redis_queue": "redis://localhost:11000"
}
```

### App Configuration
Key configurations in `crab_game/hooks.py`:
- Custom after-login redirect for Game Question Managers
- API endpoints whitelist
- Module configuration

## Project Architecture

### App Structure
```
crab_game/
├── crab_game/
│   ├── api/                 # API endpoints
│   │   ├── questions.py     # Game questions CRUD API
│   │   └── auth.py          # Authentication helpers
│   ├── crabgame/           # Module definition
│   ├── config/             # Desktop and module config
│   ├── utils/              # Utility functions
│   │   └── redirect.py     # Custom login redirects
│   ├── www/                # Web pages
│   │   ├── question_manager # Admin interface
│   │   └── crab_login      # Custom login page
│   ├── hooks.py            # App hooks and configuration
│   └── modules.txt         # Module listing
├── pyproject.toml          # Python package configuration
└── README.md
```

### Key Components
1. **Game Question DocType**: Stores question data (level, numbers, answer, time_limit)
2. **Question Manager**: Web interface for CRUD operations
3. **API Layer**: RESTful endpoints for frontend integration
4. **Custom Authentication**: Role-based redirects and permissions

## Database Schema

### Game Question DocType
| Field | Type | Description |
|-------|------|-------------|
| level | Int | Difficulty level (1-10) |
| numbers | Text | Comma-separated numbers for the question |
| correct_answer | Select | "odd" or "even" |
| time_limit | Int | Time limit in seconds |
| is_live | Check | Whether question is active |

## Testing

### Running Tests
```bash
# Run Frappe tests
bench --site site1.local run-tests --app crab_game

# Run specific test
bench --site site1.local run-tests --test path.to.test
```

### API Testing
```bash
# Test live questions endpoint
curl "http://localhost:8000/api/method/crab_game.api.questions.get_live_questions"

# Test with authentication
curl -H "Authorization: Bearer <token>" \
     "http://localhost:8000/api/method/crab_game.api.questions.get_questions_by_level?level=1"
```

## Deployment

### Production Setup
1. **Setup production bench**
   ```bash
   sudo bench setup production $USER
   ```

2. **Enable HTTPS**
   ```bash
   bench setup lets-encrypt site1.local
   ```

3. **Setup supervisor**
   ```bash
   bench setup supervisor
   sudo supervisorctl reload
   ```

4. **Configure nginx**
   ```bash
   bench setup nginx
   sudo service nginx reload
   ```

### Docker Deployment
```bash
# Using official Frappe Docker
git clone https://github.com/frappe/frappe_docker.git
cd frappe_docker
# Follow frappe_docker setup instructions
```

## Contributing
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-game-mode`)
3. Make your changes
4. Run tests (`bench --site site1.local run-tests --app crab_game`)
5. Commit changes (`git commit -am 'Add new game mode'`)
6. Push to branch (`git push origin feature/new-game-mode`)
7. Create Pull Request

## Troubleshooting

### Common Issues
1. **Redis connection failed**: Check Redis is running on ports 11000, 13000
2. **Database connection error**: Verify MariaDB credentials in site_config.json
3. **Permission denied**: Ensure proper file permissions for bench user
4. **Asset build failed**: Run `bench build --app crab_game`

### Logs
- **Frappe logs**: `frappe-bench/logs/frappe.log`
- **Error logs**: `frappe-bench/logs/worker.error.log`
- **Database logs**: `frappe-bench/logs/database.log`

## License
This project is licensed under the MIT License. See the [LICENSE](license.txt) file for details.

## Contact
- **Developer**: Comini
- **Email**: biswas.siv@gmail.com
- **App Version**: 0.0.1
