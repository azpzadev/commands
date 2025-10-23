# Ubuntu Troubleshooting Commands for Laravel, PHP, Nginx & Redis

## 🔍 Check Service Status
```bash
systemctl status nginx
systemctl status php8.3-fpm
systemctl status redis
systemctl status mysql
```

## 🔄 Restart Services
```bash
sudo systemctl restart nginx
sudo systemctl restart php8.3-fpm
sudo systemctl restart redis
sudo systemctl restart mysql
```

## 📊 Check What's Running on Ports
```bash
# Check port 80 (HTTP)
sudo netstat -tulpn | grep :80
sudo ss -tulpn | grep :80

# Check port 443 (HTTPS)
sudo netstat -tulpn | grep :443

# Check port 3306 (MySQL)
sudo netstat -tulpn | grep :3306

# Check port 6379 (Redis)
sudo netstat -tulpn | grep :6379

# Check port 9000 (PHP-FPM)
sudo netstat -tulpn | grep :9000
```

## 🚨 Kill Conflicting Processes
```bash
# Kill Apache2 (if blocking nginx)
sudo killall apache2
sudo systemctl stop apache2
sudo systemctl disable apache2

# Kill all PHP-FPM processes
sudo killall php-fpm
sudo killall php8.3-fpm

# Kill nginx
sudo killall nginx
```

## 📝 View Logs (Real-time)
```bash
# Nginx error log
sudo tail -f /var/log/nginx/error.log

# Nginx access log
sudo tail -f /var/log/nginx/access.log

# PHP-FPM log
sudo tail -f /var/log/php8.3-fpm.log

# Laravel log
tail -f storage/logs/laravel.log

# Redis log
sudo tail -f /var/log/redis/redis-server.log

# System logs
sudo journalctl -u nginx -f
sudo journalctl -u php8.3-fpm -f
```

## 🧪 Test Configurations
```bash
# Test nginx config
sudo nginx -t

# Test PHP-FPM config
sudo php-fpm8.3 -t

# Reload nginx without downtime
sudo nginx -s reload
```

## 🔧 Laravel Specific
```bash
# Clear all caches
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear

# Clear compiled classes
php artisan clear-compiled

# Optimize for production
php artisan config:cache
php artisan route:cache
php artisan view:cache

# Fix permissions
sudo chown -R www-data:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache

# Queue worker (check if running)
ps aux | grep queue:work

# Restart queue workers
php artisan queue:restart
```

## 🔴 Redis Troubleshooting
```bash
# Check Redis is running
redis-cli ping
# Should return: PONG

# Connect to Redis
redis-cli

# Inside Redis CLI:
INFO                    # Show Redis info
DBSIZE                  # Number of keys
KEYS *                  # List all keys (don't use in production!)
FLUSHALL                # Clear all data (careful!)
CONFIG GET maxmemory    # Check memory limit

# Clear Laravel cache in Redis
redis-cli FLUSHDB
```

## 🔌 PHP-FPM Troubleshooting
```bash
# Check PHP-FPM is listening
sudo netstat -pl | grep php

# Check PHP version
php -v
php-fpm8.3 -v

# Check loaded PHP modules
php -m

# Test PHP file
echo "<?php phpinfo(); ?>" | php

# Check PHP-FPM pool config
sudo nano /etc/php/8.3/fpm/pool.d/www.conf
```

## 🌐 Nginx Troubleshooting
```bash
# Check nginx version
nginx -v

# Check nginx config file
sudo nano /etc/nginx/sites-available/default

# List enabled sites
ls -la /etc/nginx/sites-enabled/

# Check if nginx is running
ps aux | grep nginx

# Start nginx
sudo systemctl start nginx
```

## 💾 Check Disk Space & Memory
```bash
# Disk space
df -h

# Memory usage
free -h

# Check inode usage (can cause "disk full" errors)
df -i

# Top processes
top
htop  # if installed
```

## 🔐 Permission Issues
```bash
# Laravel storage permissions
sudo chown -R www-data:www-data /var/www/html/your-project
sudo chmod -R 755 /var/www/html/your-project
sudo chmod -R 775 storage bootstrap/cache

# Check current permissions
ls -la storage/
ls -la bootstrap/cache/
```

## 🔄 Complete Service Restart Order
```bash
# Stop all
sudo systemctl stop nginx
sudo systemctl stop php8.3-fpm
sudo systemctl stop redis

# Start in order
sudo systemctl start redis
sudo systemctl start php8.3-fpm
sudo systemctl start nginx

# Check all statuses
systemctl status redis php8.3-fpm nginx
```

## 🆘 Emergency Nuclear Option
```bash
# When everything is broken
sudo killall apache2
sudo killall nginx
sudo killall php-fpm

sudo systemctl restart redis
sudo systemctl restart php8.3-fpm
sudo systemctl restart nginx

# Clear Laravel cache
php artisan cache:clear
php artisan config:clear

# Check everything
systemctl status nginx php8.3-fpm redis
```

## 📱 Quick Health Check Script
```bash
#!/bin/bash
echo "=== Service Status ==="
systemctl status nginx | grep Active
systemctl status php8.3-fpm | grep Active
systemctl status redis | grep Active

echo -e "\n=== Port Check ==="
sudo netstat -tulpn | grep -E ':80|:443|:3306|:6379|:9000'

echo -e "\n=== Disk Space ==="
df -h | grep -E 'Filesystem|/dev/'

echo -e "\n=== Memory ==="
free -h
```

**Usage:** Save as `check.sh`, make executable with `chmod +x check.sh`, and run `./check.sh`

---

## 📚 Quick Reference Table

| Service | Start | Stop | Restart | Status |
|---------|-------|------|---------|--------|
| Nginx | `sudo systemctl start nginx` | `sudo systemctl stop nginx` | `sudo systemctl restart nginx` | `systemctl status nginx` |
| PHP-FPM | `sudo systemctl start php8.3-fpm` | `sudo systemctl stop php8.3-fpm` | `sudo systemctl restart php8.3-fpm` | `systemctl status php8.3-fpm` |
| Redis | `sudo systemctl start redis` | `sudo systemctl stop redis` | `sudo systemctl restart redis` | `systemctl status redis` |
| MySQL | `sudo systemctl start mysql` | `sudo systemctl stop mysql` | `sudo systemctl restart mysql` | `systemctl status mysql` |

## 🎯 Most Common Issues & Solutions

### Issue: Nginx won't start
```bash
sudo killall apache2
sudo nginx -t
sudo systemctl start nginx
```

### Issue: 502 Bad Gateway
```bash
sudo systemctl restart php8.3-fpm
sudo systemctl restart nginx
```

### Issue: Laravel cache problems
```bash
php artisan cache:clear
php artisan config:clear
redis-cli FLUSHDB
```

### Issue: Permission denied
```bash
sudo chown -R www-data:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache
```

---

**💡 Pro Tip:** Bookmark this page for quick access during emergencies! 🚀
