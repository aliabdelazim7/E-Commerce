# Deployment Guide - Simple Ecommerce

## Prerequisites

### System Requirements

- Node.js (v14 or higher)
- npm or yarn package manager
- Git

### Production Environment

- Linux server (Ubuntu 20.04+ recommended)
- PM2 for process management
- Nginx for reverse proxy
- SSL certificate (Let's Encrypt recommended)

## Local Development Setup

### 1. Clone Repository

```bash
git clone <repository-url>
cd simple-ecommerce
```

### 2. Install Dependencies

```bash
# Install backend dependencies
npm install

# Install frontend dependencies
cd client
npm install
cd ..
```

### 3. Environment Configuration

Create a `.env` file in the root directory:

```env
NODE_ENV=development
PORT=5000
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
DB_PATH=./server/ecommerce.db
CORS_ORIGIN=http://localhost:3000
BCRYPT_ROUNDS=12
JWT_EXPIRES_IN=24h
MAX_FILE_SIZE=5242880
UPLOAD_PATH=./uploads
```

### 4. Start Development Servers

```bash
# Start both frontend and backend
npm run dev

# Or start separately:
# Terminal 1: Backend
npm run server

# Terminal 2: Frontend
npm run client
```

### 5. Access Application

- Frontend: http://localhost:3000
- Backend API: http://localhost:5000

## Production Deployment

### 1. Server Setup

#### Update System

```bash
sudo apt update && sudo apt upgrade -y
```

#### Install Node.js

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

#### Install PM2

```bash
sudo npm install -g pm2
```

#### Install Nginx

```bash
sudo apt install nginx -y
```

### 2. Application Deployment

#### Clone and Setup

```bash
# Clone repository
git clone <repository-url>
cd simple-ecommerce

# Install dependencies
npm install
cd client
npm install
npm run build
cd ..
```

#### Environment Configuration

Create production `.env` file:

```env
NODE_ENV=production
PORT=5000
JWT_SECRET=your-super-secret-production-jwt-key
DB_PATH=./server/ecommerce.db
CORS_ORIGIN=https://yourdomain.com
BCRYPT_ROUNDS=12
JWT_EXPIRES_IN=24h
MAX_FILE_SIZE=5242880
UPLOAD_PATH=./uploads
```

#### PM2 Configuration

Create `ecosystem.config.js`:

```javascript
module.exports = {
  apps: [
    {
      name: "simple-ecommerce",
      script: "server/index.js",
      instances: "max",
      exec_mode: "cluster",
      env: {
        NODE_ENV: "production",
        PORT: 5000,
      },
      error_file: "./logs/err.log",
      out_file: "./logs/out.log",
      log_file: "./logs/combined.log",
      time: true,
    },
  ],
};
```

#### Start Application

```bash
pm2 start ecosystem.config.js
pm2 save
pm2 startup
```

### 3. Nginx Configuration

#### Create Nginx Configuration

```bash
sudo nano /etc/nginx/sites-available/simple-ecommerce
```

Add the following configuration:

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    # Redirect HTTP to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name yourdomain.com www.yourdomain.com;

    # SSL Configuration
    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    # Security Headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
    add_header Content-Security-Policy "default-src 'self' http: https: data: blob: 'unsafe-inline'" always;

    # Gzip Compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_proxied expired no-cache no-store private must-revalidate auth;
    gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/xml+rss;

    # API Routes
    location /api/ {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }

    # Static Files
    location / {
        root /path/to/simple-ecommerce/client/build;
        try_files $uri $uri/ /index.html;
    }

    # File Upload Size
    client_max_body_size 10M;
}
```

#### Enable Site

```bash
sudo ln -s /etc/nginx/sites-available/simple-ecommerce /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### 4. SSL Certificate (Let's Encrypt)

#### Install Certbot

```bash
sudo apt install certbot python3-certbot-nginx -y
```

#### Obtain Certificate

```bash
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

#### Auto-renewal

```bash
sudo crontab -e
# Add: 0 12 * * * /usr/bin/certbot renew --quiet
```

### 5. Database Backup

#### Backup Script

Create `backup.sh`:

```bash
#!/bin/bash
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/backups"
DB_PATH="./server/ecommerce.db"

# Create backup directory
mkdir -p $BACKUP_DIR

# Backup database
cp $DB_PATH $BACKUP_DIR/ecommerce_$DATE.db

# Keep only last 7 days of backups
find $BACKUP_DIR -name "ecommerce_*.db" -mtime +7 -delete

echo "Backup completed: ecommerce_$DATE.db"
```

#### Schedule Backup

```bash
chmod +x backup.sh
crontab -e
# Add: 0 2 * * * /path/to/backup.sh
```

### 6. Monitoring and Logs

#### PM2 Monitoring

```bash
pm2 monit
pm2 logs
```

#### Nginx Logs

```bash
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```

### 7. Security Hardening

#### Firewall Configuration

```bash
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

#### Fail2Ban

```bash
sudo apt install fail2ban -y
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

### 8. Performance Optimization

#### Database Optimization

- Regular VACUUM operations
- Monitor query performance
- Add indexes as needed

#### Application Optimization

- Enable PM2 cluster mode
- Use Redis for session storage (optional)
- Implement caching strategies

### 9. Health Checks

#### Health Check Endpoint

Add to your application:

```javascript
app.get("/health", (req, res) => {
  res.status(200).json({
    status: "OK",
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
  });
});
```

#### Monitoring Script

Create `health-check.sh`:

```bash
#!/bin/bash
response=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:5000/health)
if [ $response != "200" ]; then
    echo "Health check failed"
    pm2 restart simple-ecommerce
fi
```

## Troubleshooting

### Common Issues

#### Application Won't Start

- Check Node.js version
- Verify all dependencies are installed
- Check environment variables
- Review PM2 logs

#### Database Issues

- Verify database file permissions
- Check disk space
- Review database logs

#### Nginx Issues

- Test configuration: `sudo nginx -t`
- Check error logs
- Verify SSL certificates

#### Performance Issues

- Monitor PM2 processes
- Check database performance
- Review Nginx access logs
- Monitor server resources

### Log Locations

- Application logs: `./logs/`
- PM2 logs: `~/.pm2/logs/`
- Nginx logs: `/var/log/nginx/`
- System logs: `/var/log/syslog`

## Maintenance

### Regular Tasks

- Update dependencies monthly
- Monitor security advisories
- Backup database daily
- Review logs weekly
- Update SSL certificates (auto-renewal)

### Scaling Considerations

- Use load balancer for multiple instances
- Implement Redis for session storage
- Use CDN for static assets
- Consider database clustering for high traffic
