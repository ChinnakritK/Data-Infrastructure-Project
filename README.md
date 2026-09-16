# My Django React App 

Multi-container web application with Django REST API, React frontend, PostgreSQL database, and Nginx reverse proxy.


## Features

✅ **Django REST API** - Modern Python web framework
✅ **React Frontend** - Interactive user interface
✅ **PostgreSQL** - Reliable relational database
✅ **Nginx** - High-performance web server
✅ **Docker** - Containerized deployment
✅ **SSL/TLS** - HTTPS encryption
✅ **Multi-stage Build** - Optimized image sizes
✅ **Non-root User** - Security best practice
✅ **Health Checks** - Automatic monitoring
✅ **Rate Limiting** - DDoS protection
✅ **Caching** - Performance optimization
✅ **CORS** - Cross-origin support

## Prerequisites

- Docker & Docker Compose
- Git
- Terminal/Command Prompt

## Quick Start

### 1. Clone or Download

```bash
cd my-django-react-app
```

### 2. Create Environment File

```bash
cp .env.example .env
```

**Edit `.env` and set:**
- `DATABASE_PASSWORD` - Strong password
- `SECRET_KEY` - Django secret key
- `STRIPE_API_KEY` - Stripe API key
- `SENDGRID_API_KEY` - SendGrid API key
- And other API keys...

### 3. Create SSL Certificates

```bash
mkdir -p nginx/certs
openssl req -x509 -newkey rsa:4096 \
  -keyout nginx/certs/key.pem \
  -out nginx/certs/cert.pem \
  -days 365 -nodes
```

### 4. Backend is Ready

Django project files already copied:
- config/settings.py
- config/urls.py
- config/wsgi.py
- manage.py

No additional setup needed.


### 5. Install Frontend Dependencies

cd frontend

# Install dependencies
npm install

cd ..

### 6. Build and Run

```bash
# Build images
docker-compose build

# Start services
docker-compose up -d

# Run migrations
docker-compose exec django python manage.py migrate

# Create superuser
docker-compose exec django python manage.py createsuperuser
```

### 7. Access Application

- **Frontend**: https://localhost/
- **Admin**: https://localhost/admin
- **API**: https://localhost/api/

## File Structure

```
my-django-react-app/
├── backend/                    # Django REST API
│   ├── Dockerfile             # Docker build instructions
│   ├── requirements.txt        # Python dependencies
│   ├── manage.py              # Django management
│   ├── config/                # Django settings
│   │   ├── settings.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   ├── users/                 # Example app
│   └── staticfiles/           # Compiled static files
│
├── frontend/                  # React SPA
│   ├── Dockerfile            # Docker build instructions
│   ├── package.json          # Node dependencies
│   ├── src/                  # React source code
│   ├── public/               # Static files
│   └── build/                # Production build
│
├── nginx/                    # Web Server
│   ├── nginx.conf           # Nginx configuration
│   ├── certs/               # SSL certificates
│   │   ├── cert.pem
│   │   └── key.pem
│   └── logs/                # Nginx logs
│
├── docker-compose.yml       # Container orchestration
├── .env                     # Environment variables (SECRET!)
├── .gitignore               # Git ignore rules
├── Makefile                 # Useful commands
└── README.md                # This file
```

## Common Commands

### Docker Compose

```bash
# View status
docker-compose ps

# View logs
docker-compose logs -f

# Stop services
docker-compose stop

# Remove everything
docker-compose down -v
```

### Django Management

```bash
# Run migrations
docker-compose exec django python manage.py migrate

# Create superuser
docker-compose exec django python manage.py createsuperuser

# Django shell
docker-compose exec django python manage.py shell

# Collect static files
docker-compose exec django python manage.py collectstatic

# Run tests
docker-compose exec django python manage.py test
```

### Bash Access

```bash
# Access Django container
docker-compose exec django bash

# Access PostgreSQL
docker-compose exec db psql -U postgres -d myapp_db

# Access Nginx
docker-compose exec nginx bash
```

### Using Makefile

```bash
make help              # Show all commands
make build             # Build images
make up                # Start services
make down              # Stop services
make migrate           # Run migrations
make createsuperuser   # Create admin user
make logs              # View logs
make clean             # Remove everything
```

## Development Workflow

### 1. Update Django Code

```bash
# Modify code in backend/
# Changes auto-reflect in container (via volume mount)

# Run migrations if models changed
docker-compose exec django python manage.py makemigrations
docker-compose exec django python manage.py migrate
```

### 2. Update React Code

```bash
# Modify code in frontend/
# Changes auto-reflect in dev server
# Browser auto-reloads (hot reload)
```

### 3. Add Python Dependencies

```bash
# Edit backend/requirements.txt
# Add new package

# Rebuild Django image
docker-compose build django

# Restart service
docker-compose up -d django
```

### 4. Add Node Dependencies

```bash
# In frontend folder
npm install package-name

# Or manually add to package.json
# Rebuild React image
docker-compose build react

# Restart service
docker-compose up -d react
```

## Production Deployment

### 1. Security Checklist

```bash
# Edit .env
DEBUG=False
SECURE_SSL_REDIRECT=True
SESSION_COOKIE_SECURE=True
CSRF_COOKIE_SECURE=True
```

### 2. Update ALLOWED_HOSTS

```bash
ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com
```

### 3. Use Real SSL Certificate

```bash
# Instead of self-signed, use Let's Encrypt
# Or purchase from CA
```

### 4. Update Nginx server_name

```nginx
server_name yourdomain.com www.yourdomain.com;
```

### 5. Remove Debug Ports

```yaml
# In docker-compose.yml, remove:
db:
  ports:
    - "5432:5432"  # Remove!

django:
  ports:
    - "8000:8000"  # Remove!
```

### 6. Set Strong Passwords

```bash
DATABASE_PASSWORD=very-long-random-secure-password
SECRET_KEY=very-long-random-secret-key
```

## Troubleshooting

### "Connection refused"

```bash
docker-compose logs django
# Check if Django is running

docker-compose ps
# Check service status
```

### "Database error"

```bash
docker-compose exec db pg_isready -U postgres
# Check PostgreSQL health

docker-compose logs db
# View database logs
```

### "Port already in use"

```bash
# Find process using port
lsof -i :80    # HTTP
lsof -i :443   # HTTPS
lsof -i :5432  # PostgreSQL
lsof -i :8000  # Django

# Kill process if needed
kill -9 <PID>
```

### "Certificate verification failed"

This is normal for self-signed certificates in development.

```bash
# Use -k flag to skip verification
curl -k https://localhost/

# Or accept in browser: Advanced → Proceed
```

## API Documentation

### Health Check

```bash
GET /health/
Response: 200 OK
```

### Django Admin

```
https://localhost/admin/
Username: (from createsuperuser)
Password: (from createsuperuser)
```

### API Endpoints

Add your API endpoints in `backend/config/urls.py`

```python
path('api/users/', include('users.urls')),
```

## Environment Variables

See `.env.example` for all available variables.

**Critical variables:**
- `DATABASE_PASSWORD` - Database password
- `SECRET_KEY` - Django encryption key
- `STRIPE_API_KEY` - Stripe payment key
- `SENDGRID_API_KEY` - Email service key

## License

This project is open source and available under the MIT License.

## Support

For issues or questions:
1. Check logs: `docker-compose logs`
2. Review `.env` configuration
3. Ensure all images built successfully
4. Check network connectivity between services

## Resources

- [Django Documentation](https://docs.djangoproject.com/)
- [React Documentation](https://react.dev/)
- [Docker Documentation](https://docs.docker.com/)
- [Nginx Documentation](https://nginx.org/en/docs/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

---

