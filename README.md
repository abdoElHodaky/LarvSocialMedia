# 🚀 Laravel Social Media Platform

A comprehensive social media application built with Laravel 9, featuring real-time messaging, posts, comments, bookmarks, and friend connections.

## ✨ Features

### 🔐 **Authentication & User Management**
- User registration and login with Laravel Breeze
- Profile management and customization
- Secure authentication with Laravel Sanctum

### 📱 **Social Features**
- **Posts**: Create, edit, delete posts with rich content
- **Comments**: Interactive commenting system
- **Bookmarks**: Save and organize favorite posts
- **Friend System**: Send/accept friend requests and manage connections
- **Real-time Messaging**: Private conversations between users
- **Categories**: Organize content with categorization

### 🛠️ **Technical Features**
- **PWA Support**: Progressive Web App capabilities with `ladumor/laravel-pwa`
- **API Documentation**: Swagger/OpenAPI documentation with `mtrajano/laravel-swagger`
- **RESTful API**: Complete API endpoints for mobile/frontend integration
- **Database**: PostgreSQL with optimized queries
- **Frontend**: Blade templates with Tailwind CSS and Vite

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Laravel Social Media                     │
├─────────────────────────────────────────────────────────────┤
│  Frontend Layer                                             │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐          │
│  │ Blade Views │ │ Tailwind CSS│ │ Vite Assets │          │
│  └─────────────┘ └─────────────┘ └─────────────┘          │
├─────────────────────────────────────────────────────────────┤
│  Application Layer                                          │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐          │
│  │ Controllers │ │ Middleware  │ │ Policies    │          │
│  └─────────────┘ └─────────────┘ └─────────────┘          │
├─────────────────────────────────────────────────────────────┤
│  Business Logic Layer                                       │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐          │
│  │ Models      │ │ Services    │ │ Events      │          │
│  │ • User      │ │ • Messaging │ │ • Real-time │          │
│  │ • Post      │ │ • Friends   │ │ • Notifications│       │
│  │ • Message   │ │ • Posts     │ │             │          │
│  │ • Comment   │ │             │ │             │          │
│  └─────────────┘ └─────────────┘ └─────────────┘          │
├─────────────────────────────────────────────────────────────┤
│  Data Layer                                                 │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐          │
│  │ PostgreSQL  │ │ Migrations  │ │ Seeders     │          │
│  │ Database    │ │ & Schema    │ │ & Factories │          │
│  └─────────────┘ └─────────────┘ └─────────────┘          │
└─────────────────────────────────────────────────────────────┘
```

## 📋 Prerequisites

- **PHP**: ^8.0.2
- **Composer**: Latest version
- **Node.js**: ^16.0.0
- **PostgreSQL**: ^13.0
- **Web Server**: Apache/Nginx

## 🚀 Installation & Setup

### 1. **Clone Repository**
```bash
git clone https://github.com/abdoElHodaky/LarvSocialMedia.git
cd LarvSocialMedia
```

### 2. **Install Dependencies**
```bash
# Install PHP dependencies
composer update

# Install Node.js dependencies
npm install
```

### 3. **Environment Configuration**
```bash
# Copy environment file
cp .env.example .env

# Generate application key
php artisan key:generate
```

### 4. **Database Configuration**
Update your `.env` file with database credentials:
```env
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=laravelsocialmedia
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

### 5. **Database Setup**
```bash
# Run migrations
php artisan migrate

# Seed database with sample data
php artisan db:seed
```

### 6. **Build Assets**
```bash
# Development
npm run dev

# Production
npm run build
```

### 7. **Start Application**
```bash
# Start Laravel development server
php artisan serve

# The application will be available at http://localhost:8000
```

## 🔑 Default Credentials

After seeding the database, you can login with:
- **Email**: admin@example.com
- **Password**: admin

## 📚 API Documentation

The application includes comprehensive API documentation powered by Swagger. After starting the application, visit:
- **Swagger UI**: `/api/documentation`
- **OpenAPI JSON**: `/swagger.json`

## 🧪 Testing

```bash
# Run all tests
php artisan test

# Run specific test suite
php artisan test --testsuite=Feature
php artisan test --testsuite=Unit
```

## 📱 PWA Features

This application supports Progressive Web App features:
- **Offline Support**: Basic offline functionality
- **Install Prompt**: Users can install the app on their devices
- **Push Notifications**: Real-time notifications support

## 🔧 Development

### **Code Style**
```bash
# Format code with Laravel Pint
./vendor/bin/pint
```

### **Database Management**
```bash
# Create new migration
php artisan make:migration create_table_name

# Create new model with migration
php artisan make:model ModelName -m

# Create new seeder
php artisan make:seeder TableSeeder
```

### **Asset Compilation**
```bash
# Watch for changes (development)
npm run dev

# Build for production
npm run build
```

## 🚀 Deployment

### **Production Checklist**
- [ ] Set `APP_ENV=production` in `.env`
- [ ] Set `APP_DEBUG=false` in `.env`
- [ ] Configure production database
- [ ] Run `composer install --optimize-autoloader --no-dev`
- [ ] Run `php artisan config:cache`
- [ ] Run `php artisan route:cache`
- [ ] Run `php artisan view:cache`
- [ ] Set up proper web server configuration
- [ ] Configure SSL certificate
- [ ] Set up backup strategy

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

If you encounter any issues or have questions:
1. Check the [Issues](https://github.com/abdoElHodaky/LarvSocialMedia/issues) page
2. Create a new issue with detailed information
3. Contact the maintainers

---

**Built with ❤️ using Laravel 9 and modern web technologies**
