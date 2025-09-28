# Deployment Guide

## 🚀 Production Deployment

### Prerequisites
- Node.js 18+ installed
- PostgreSQL database
- Domain name and SSL certificate
- Server (VPS, AWS, DigitalOcean, etc.)

### Environment Setup

1. **Create production environment file**
```bash
cp env.example .env.production
```

2. **Configure production variables**
```env
# Database
DATABASE_URL="postgresql://username:password@your-db-host:5432/influencer_platform_prod"

# JWT
JWT_SECRET="your-super-secure-production-jwt-secret"
JWT_EXPIRES_IN="7d"

# Server
PORT=3000
NODE_ENV="production"
FRONTEND_URL="https://yourdomain.com"

# Email (Production SMTP)
SMTP_HOST="smtp.your-provider.com"
SMTP_PORT=587
SMTP_USER="your-production-email"
SMTP_PASS="your-production-password"

# Stripe (Production keys)
STRIPE_SECRET_KEY="sk_live_..."
STRIPE_PUBLISHABLE_KEY="pk_live_..."
STRIPE_WEBHOOK_SECRET="whsec_..."

# Social Media APIs (Production)
INSTAGRAM_CLIENT_ID="your-production-instagram-id"
INSTAGRAM_CLIENT_SECRET="your-production-instagram-secret"
YOUTUBE_API_KEY="your-production-youtube-key"
TIKTOK_CLIENT_KEY="your-production-tiktok-key"
TIKTOK_CLIENT_SECRET="your-production-tiktok-secret"
```

### Database Setup

1. **Create production database**
```sql
CREATE DATABASE influencer_platform_prod;
CREATE USER platform_user WITH PASSWORD 'secure_password';
GRANT ALL PRIVILEGES ON DATABASE influencer_platform_prod TO platform_user;
```

2. **Run migrations**
```bash
npm run db:push
# or
npm run db:migrate
```

### Backend Deployment

1. **Build the application**
```bash
npm run build
```

2. **Start production server**
```bash
npm start
```

3. **Using PM2 (recommended)**
```bash
npm install -g pm2
pm2 start dist/index.js --name "influencer-platform"
pm2 startup
pm2 save
```

### Frontend Deployment

1. **Build frontend**
```bash
cd frontend
npm install
npm run build
```

2. **Deploy to Vercel/Netlify**
```bash
# Vercel
npx vercel --prod

# Netlify
npm run build
# Upload dist folder to Netlify
```

### Nginx Configuration

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name yourdomain.com www.yourdomain.com;

    ssl_certificate /path/to/your/certificate.crt;
    ssl_certificate_key /path/to/your/private.key;

    # API routes
    location /api {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }

    # WebSocket support
    location /socket.io {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Frontend
    location / {
        root /path/to/frontend/dist;
        try_files $uri $uri/ /index.html;
    }
}
```

### Docker Deployment

1. **Create Dockerfile**
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

2. **Create docker-compose.yml**
```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://user:password@db:5432/influencer_platform
    depends_on:
      - db
    volumes:
      - ./uploads:/app/uploads

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=influencer_platform
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - app

volumes:
  postgres_data:
```

3. **Deploy with Docker**
```bash
docker-compose up -d
```

### Security Checklist

- [ ] Change all default passwords
- [ ] Enable SSL/TLS certificates
- [ ] Configure firewall rules
- [ ] Set up rate limiting
- [ ] Enable CORS properly
- [ ] Configure secure headers
- [ ] Set up monitoring and logging
- [ ] Regular security updates
- [ ] Database backups
- [ ] Environment variable security

### Monitoring

1. **Health checks**
```bash
curl https://yourdomain.com/health
```

2. **Log monitoring**
```bash
pm2 logs influencer-platform
```

3. **Database monitoring**
```sql
SELECT * FROM pg_stat_activity;
```

### Backup Strategy

1. **Database backup**
```bash
pg_dump influencer_platform_prod > backup_$(date +%Y%m%d_%H%M%S).sql
```

2. **File backup**
```bash
tar -czf uploads_backup_$(date +%Y%m%d_%H%M%S).tar.gz uploads/
```

### Scaling Considerations

- Use Redis for session storage
- Implement database connection pooling
- Set up CDN for static assets
- Use load balancer for multiple instances
- Implement caching strategies
- Monitor performance metrics

### Troubleshooting

1. **Check logs**
```bash
pm2 logs influencer-platform
tail -f /var/log/nginx/error.log
```

2. **Database connection issues**
```bash
psql -h your-db-host -U username -d influencer_platform_prod
```

3. **Port conflicts**
```bash
netstat -tulpn | grep :3000
```

### Performance Optimization

- Enable gzip compression
- Use CDN for static assets
- Implement database indexing
- Optimize images
- Use caching layers
- Monitor and optimize queries
