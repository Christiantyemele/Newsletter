# Rust Cameroon Community Website

A modern, production-ready website for the Rust Cameroon community built with React and Rust.

## Features

- 🏠 **Home Page**: Welcome section with community mission and recent posts
- 📖 **About Page**: Information about the Rust Cameroon community
- 📅 **Events Page**: Upcoming and past community events
- 📝 **Blog**: Articles and tutorials from the community
- 🔧 **Admin Panel**: Simple interface for managing posts
- 🚀 **Production Ready**: Optimized for deployment on AWS EC2
- 🔒 **SSL Support**: Automatic Let's Encrypt certificate management
- 📱 **Responsive**: Mobile-first design with TailwindCSS
- ♿ **Accessible**: Semantic HTML and keyboard navigation support
- 🔍 **SEO Optimized**: Meta tags and structured data

## Tech Stack

### Frontend
- **React 18** with TypeScript
- **React Router** for navigation
- **TailwindCSS** for styling
- **React Helmet** for SEO
- **Axios** for API calls
- **Context API** for state management

### Backend
- **Rust** with Axum web framework
- **JSON file storage** for posts (easily replaceable with database)
- **CORS enabled** for cross-origin requests
- **Structured logging** with tracing

### Deployment
- **Nginx** reverse proxy with SSL termination
- **Let's Encrypt** for free SSL certificates
- **Systemd** for service management
- **Docker** support for containerized deployment
- **AWS EC2** deployment scripts

## Quick Start

### Prerequisites

- Node.js 18+ and npm
- Rust 1.75+
- Git

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd rust-cameroon-website
   ```

2. **Setup Frontend**
   ```bash
   cd frontend
   npm install
   cp env.example .env
   # Edit .env with your configuration
   npm run dev
   ```

3. **Setup Backend**
   ```bash
   cd backend
   cargo run
   ```

4. **Access the application**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:8000
   - Admin Panel: http://localhost:5173/admin

## Configuration

### Environment Variables

Create a `.env` file in the frontend directory:

```env
# API Configuration
VITE_API_URL=http://localhost:8000/api

# Admin Configuration
VITE_ADMIN_PASSWORD=your-secure-password
```
## Deployment

### AWS EC2 Deployment

1. **Prepare your EC2 instance**
   - Launch Ubuntu 22.04 LTS instance
   - Configure security groups (ports 22, 80, 443)
   - Set up SSH key access

2. **Configure deployment**
   ```bash
   # Edit deployment script
   nano scripts/deploy.sh
   # Update EC2_HOST and DOMAIN variables
   ```

3. **Deploy**
   ```bash
   ./scripts/deploy.sh
   ```

4. **Setup SSL**
   ```bash
   # SSH into your server
   ssh ubuntu@your-ec2-ip
   
   # Run SSL setup
   sudo ./scripts/ssl-setup.sh your-domain.com
   ```

### Docker Deployment

1. **Build and run with Docker Compose**
   ```bash
   docker-compose -f docker-compose.prod.yml up -d
   ```

2. **Setup SSL certificates**
   ```bash
   # Install certbot on host
   sudo apt install certbot
   
   # Obtain certificate
   sudo certbot certonly --webroot -w ./frontend/dist -d your-domain.com
   ```

### Manual Deployment

1. **Build the application**
   ```bash
   # Frontend
   cd frontend && npm run build
   
   # Backend
   cd backend && cargo build --release
   ```

2. **Setup server**
   ```bash
   # Copy files to server
   scp -r frontend/dist user@server:/var/www/rust-cameroon/
   scp backend/target/release/rust-cameroon-api user@server:/opt/rust-cameroon/
   
   # Configure Nginx
   sudo cp nginx-https.conf /etc/nginx/sites-available/rust-cameroon
   sudo ln -s /etc/nginx/sites-available/rust-cameroon /etc/nginx/sites-enabled/
   sudo systemctl reload nginx
   ```

## Project Structure

```
rust-cameroon-website/
├── frontend/                 # React frontend
│   ├── src/
│   │   ├── components/       # Reusable components
│   │   │   ├── layout/       # Layout components
│   │   │   └── ui/          # UI components
│   │   ├── pages/           # Page components
│   │   ├── context/         # React context
│   │   ├── types/           # TypeScript types
│   │   └── utils/           # Utility functions
│   ├── public/              # Static assets
│   └── dist/                # Build output
├── backend/                 # Rust backend
│   ├── src/                 # Source code
│   └── posts.json           # Posts data
├── scripts/                 # Deployment scripts
│   ├── deploy.sh           # Main deployment script
│   ├── setup-server.sh     # Server setup
│   ├── ssl-setup.sh        # SSL certificate setup
│   └── start-services.sh   # Service management
├── nginx.conf              # Nginx configuration
├── docker-compose.prod.yml # Docker Compose for production
└── README.md               # This file
```

## Adding Content

### Creating Posts

1. **Via Admin Panel** (Recommended)
   - Go to `/admin`
   - Click "New Post"
   - Fill in the form and save

2. **Via JSON File**
   - Edit `backend/posts.json`
   - Add new post object with required fields
   - Restart the backend service

### Post Structure

```json
{
  "id": "unique-id",
  "title": "Post Title",
  "content": "<p>HTML content</p>",
  "excerpt": "Brief description",
  "author": "Author Name",
  "date": "2024-01-01",
  "tags": ["rust", "programming"],
  "image_url": "https://example.com/image.jpg",
  "slug": "post-title-slug"
}
```

## Security

- Admin panel is password-protected
- SSL/TLS encryption for all traffic
- Security headers configured in Nginx
- Fail2ban protection against brute force attacks
- Non-root user for backend service

## Performance

- Gzip compression enabled
- Static asset caching (1 year)
- Optimized React build
- Efficient Rust backend
- CDN-ready static assets

## Monitoring

- Systemd service management
- Log rotation configured
- Health checks for services
- Nginx access/error logs

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support and questions:
- GitHub Issues: [Create an issue](https://github.com/your-repo/issues)
- Discord: [Rust Cameroon Community](https://discord.gg/rust-cameroon)
- Email: admin@rust-cameroon.org

## Roadmap

- [ ] Database integration (PostgreSQL)
- [ ] User authentication system
- [ ] Comment system for posts
- [ ] Event management system
- [ ] Newsletter integration
- [ ] Multi-language support
- [ ] Dark mode toggle
- [ ] Progressive Web App features



Requesting a certificate for rustcameroon.com and www.rustcameroon.com

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/rustcameroon.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/rustcameroon.com/privkey.pem
This certificate expires on 2026-04-25.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.