
# Laravel + Nginx + MySQL using Docker Compose
Docker Compose setup to run a **Laravel application** with **Nginx**,and **MySQL**.

---

## Prerequisites
- Docker Desktop (Docker Compose v2)
- Git

```bash
docker --version
docker compose version
```

---

## Project Structure

```text
docker/        → Nginx + PHP configs
src/           → Laravel application
docker-compose.yml
```

---

## Setup Guide (Step-by-Step)

### 1. Clone Repository

```bash
git clone <your-repo-url>
cd laravel-app
```

---

### 2. Environment Setup

```bash
cp src/.env.example src/.env
```

Edit `src/.env`:

```env
DB_CONNECTION=mysql
DB_HOST=mysql-db
DB_PORT=3306
DB_DATABASE=mysqldb
DB_USERNAME=dbuser1
DB_PASSWORD=pass123
```

---

### 3. Build and Start Containers

```bash
docker compose up -d --build
```

---

### 4. Install Dependencies

```bash
docker compose exec php-fpm composer install
```

---

### 5. Set Permissions

```bash
docker compose exec php-fpm chown -R www-data:www-data storage bootstrap/cache
```

---

### 6. Generate App Key

```bash
docker compose exec php-fpm php artisan key:generate
```

---

### 7. Run Migrations

```bash
docker compose exec php-fpm php artisan migrate
```

---

### 8. Access Application

Open:

```
http://localhost:8080
```

---

## How It Works

```
Browser → Nginx → PHP-FPM → MySQL
```

- Nginx serves `public/` and proxies PHP requests
- PHP-FPM runs Laravel
- MySQL data persists using Docker volumes

---

## Nginx (Key Configuration)

```nginx
root /var/www/public;

location / {
    try_files $uri $uri/ /index.php?$query_string;
}

location ~ \.php$ {
    fastcgi_pass php-fpm:9000;
}
```

- Docker DNS resolves `php-fpm`
- `try_files` enables Laravel routing

---

## Laravel App (Reference)

Laravel source is already included in `src/`.

Created originally using:

```bash
composer global require laravel/installer
```
Add to PATH (zsh — default on macOS)
```bash
Run:
echo 'export PATH="$HOME/.composer/vendor/bin:$PATH"' >> ~/.zshrc
echo 'export PATH="$HOME/.config/composer/vendor/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```
```bash
laravel new laravel-app
```

---

## Useful Commands

```bash
docker compose ps
docker compose logs -f mysql-db
docker compose down
```





