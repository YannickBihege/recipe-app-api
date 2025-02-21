# Django Application Backend

A containerized Django application backend with PostgreSQL database support and automated deployment using GitHub Actions.

## Technologies Used

- Python 3.9
- Django 3.2.4
- Django REST Framework 3.12.4
- PostgreSQL 13
- Docker & Docker Compose
- GitHub Actions for CI/CD
- Flake8 for code quality

## Project Structure

```
.
├── app/                    # Django application root
├── .github/               
│   └── workflows/         # GitHub Actions workflows
├── requirements/
│   ├── requirements.txt   # Production dependencies
│   └── requirements.dev.txt # Development dependencies
├── .dockerignore
├── .gitignore
├── docker-compose.yml
├── Dockerfile
└── README.md
```

## Prerequisites

- Docker and Docker Compose installed
- Git installed
- GitHub account (for deployment)

## Development Setup

1. Clone the repository:
```bash
git clone <repository-url>
cd <project-directory>
```

2. Create environment files:
```bash
cp .env.example .env
```

3. Build and run the Docker containers:
```bash
docker-compose up --build
```

4. Run migrations:
```bash
docker-compose exec app python manage.py migrate
```

5. Create a superuser:
```bash
docker-compose exec app python manage.py createsuperuser
```

The application will be available at http://localhost:8000

## Environment Variables

Key environment variables used in the project:

```
DB_HOST=db
DB_NAME=devdb
DB_USER=devuser
DB_PASS=changeme
```

## Docker Configuration

The project uses two main services:

### App Service
- Built from Python 3.9 Alpine image
- Runs the Django application
- Configured for development with volume mounting
- Exposes port 8000

### Database Service
- PostgreSQL 13 Alpine
- Persistent volume for data storage
- Configured with environment variables

## Running Tests

To run the test suite:

```bash
docker-compose exec app python manage.py test
```

To run code quality checks with Flake8:

```bash
docker-compose exec app flake8
```

## Deployment

The application uses GitHub Actions for automated deployment. The workflow includes:

1. Code Quality Check
2. Unit Tests
3. Build and Push Docker Image
4. Deploy to Production

### GitHub Actions Workflow

The workflow is triggered on:
- Push to main branch
- Pull requests to main branch

Configuration file: `.github/workflows/checks.yml`

## Development Guidelines

### Code Quality

The project uses Flake8 for code quality enforcement. Configuration can be found in `.flake8`:

```ini
[flake8]
exclude =
    migrations,
    __pycache__,
    manage.py,
    settings.py
max-line-length = 100
```

### Making Changes

1. Create a new branch:
```bash
git checkout -b feature/your-feature-name
```

2. Make your changes and ensure tests pass:
```bash
docker-compose exec app python manage.py test
docker-compose exec app flake8
```

3. Commit your changes:
```bash
git add .
git commit -m "Description of changes"
git push origin feature/your-feature-name
```

4. Create a Pull Request on GitHub

## API Documentation

API documentation is available at `/api/docs/` when running the application.

## Database Management

### Creating Migrations

```bash
docker-compose exec app python manage.py makemigrations
```

### Applying Migrations

```bash
docker-compose exec app python manage.py migrate
```

### Backing Up Data

```bash
docker-compose exec db pg_dump -U devuser devdb > backup.sql
```

## Troubleshooting

Common issues and solutions:

1. Database connection issues:
   - Ensure PostgreSQL service is running
   - Check environment variables
   - Verify network connectivity between containers

2. Permission issues:
   - Check file ownership in mounted volumes
   - Verify user permissions in containers

3. Migration issues:
   - Reset migrations if needed
   - Check for conflicting migrations

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

[Your License Here]

## Contact

Maintainer: bihegeyannick

## Acknowledgments

- Django documentation
- Docker documentation
- GitHub Actions documentation