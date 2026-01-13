# 🏗️ Laravel Social Media - System Architecture

## 📋 Overview

This document provides a comprehensive overview of the Laravel Social Media platform architecture, including system components, data flow, and technical implementation details.

## 🎯 System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                          Laravel Social Media Platform                         │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                Client Layer                                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │   Web App   │  │ Mobile App  │  │     PWA     │  │  API Client │          │
│  │  (Browser)  │  │ (iOS/Android│  │ (Offline)   │  │ (3rd Party) │          │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘          │
│         │                 │                 │                 │               │
│         └─────────────────┼─────────────────┼─────────────────┘               │
│                           │                 │                                 │
├───────────────────────────┼─────────────────┼─────────────────────────────────┤
│                      Load Balancer / Reverse Proxy                             │
│  ┌─────────────────────────────────────────────────────────────────────────┐  │
│  │                        Nginx / Apache                                   │  │
│  └─────────────────────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────────────────────┤
│                              Application Layer                                  │
│  ┌─────────────────────────────────────────────────────────────────────────┐  │
│  │                          Laravel 9 Framework                           │  │
│  │                                                                         │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐   │  │
│  │  │   Routes    │  │ Middleware  │  │ Controllers │  │  Policies   │   │  │
│  │  │ (web/api)   │  │ (Auth/CORS) │  │ (Business)  │  │(Authorization│   │  │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘   │  │
│  │                                                                         │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐   │  │
│  │  │   Models    │  │  Services   │  │   Events    │  │   Jobs      │   │  │
│  │  │ (Eloquent)  │  │ (Business)  │  │(Real-time)  │  │ (Queues)    │   │  │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘   │  │
│  └─────────────────────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────────────────────┤
│                               Data Layer                                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │ PostgreSQL  │  │    Redis    │  │ File Storage│  │   Search    │          │
│  │ (Primary DB)│  │ (Cache/Queue│  │ (Media/Docs)│  │ (Optional)  │          │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘          │
├─────────────────────────────────────────────────────────────────────────────────┤
│                            External Services                                    │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │    Email    │  │Push Notifications│ │   CDN     │  │  Analytics  │          │
│  │  (SMTP)     │  │ (Firebase)  │  │ (CloudFlare)│  │ (Optional)  │          │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘          │
└─────────────────────────────────────────────────────────────────────────────────┘
```

## 🔄 Data Flow Architecture

### 1. **User Request Flow**
```
User Action → Route → Middleware → Controller → Service → Model → Database
     ↓
Response ← View/JSON ← Controller ← Service ← Model ← Database Result
```

### 2. **Real-time Communication Flow**
```
User A → WebSocket/Pusher → Laravel Echo → Broadcasting → User B
                                ↓
                         Database Storage
```

## 📊 Database Schema Overview

### **Core Entities**

```sql
-- Users Table
users (id, name, email, password, avatar, bio, created_at, updated_at)

-- Posts Table  
posts (id, user_id, content, image, likes_count, comments_count, created_at)

-- Comments Table
comments (id, post_id, user_id, content, parent_id, created_at)

-- Messages Table
messages (id, conversation_id, sender_id, content, read_at, created_at)

-- Conversations Table
conversations (id, type, participants, last_message_at, created_at)

-- Friend Requests Table
friend_requests (id, sender_id, receiver_id, status, created_at)

-- Bookmarks Table
bookmarks (id, user_id, post_id, created_at)

-- Categories Table
categories (id, name, description, color, created_at)
```

### **Relationships**
- User → Posts (1:N)
- User → Comments (1:N)  
- User → Messages (1:N)
- User → Bookmarks (1:N)
- Post → Comments (1:N)
- Post → Categories (N:M)
- User → Friend Requests (1:N as sender/receiver)
- Conversation → Messages (1:N)

## 🔧 Component Architecture

### **Frontend Components**
```
resources/
├── views/
│   ├── layouts/
│   │   ├── app.blade.php          # Main layout
│   │   └── guest.blade.php        # Guest layout
│   ├── auth/                      # Authentication views
│   ├── posts/                     # Post management
│   ├── messages/                  # Messaging interface
│   └── profile/                   # User profiles
├── js/
│   ├── app.js                     # Main JavaScript entry
│   ├── components/                # Vue/Alpine components
│   └── echo.js                    # Real-time setup
└── css/
    └── app.css                    # Tailwind CSS
```

### **Backend Components**
```
app/
├── Http/
│   ├── Controllers/
│   │   ├── Auth/                  # Authentication
│   │   ├── PostController.php     # Post management
│   │   ├── MessageController.php  # Messaging
│   │   └── UserController.php     # User management
│   ├── Middleware/                # Request filtering
│   └── Requests/                  # Form validation
├── Models/                        # Eloquent models
├── Services/                      # Business logic
├── Events/                        # Event system
└── Jobs/                          # Background tasks
```

## 🚀 Deployment Architecture

### **Development Environment**
```
Local Machine
├── PHP 8.0+ (Laravel Artisan)
├── PostgreSQL (Local instance)
├── Node.js (Vite dev server)
└── Redis (Optional, for caching)
```

### **Production Environment**
```
Production Server
├── Web Server (Nginx/Apache)
├── PHP-FPM (Laravel application)
├── PostgreSQL (Production database)
├── Redis (Cache + Queue worker)
├── SSL Certificate (HTTPS)
└── Process Manager (Supervisor)
```

### **Containerized Deployment (Docker)**
```
Docker Compose
├── app (Laravel + PHP-FPM)
├── nginx (Web server)
├── postgres (Database)
├── redis (Cache/Queue)
└── worker (Queue processing)
```

## 🔐 Security Architecture

### **Authentication & Authorization**
- **Laravel Sanctum**: API token authentication
- **Laravel Breeze**: Web authentication scaffolding
- **Policies**: Resource-based authorization
- **Middleware**: Request filtering and validation

### **Data Protection**
- **CSRF Protection**: Cross-site request forgery prevention
- **SQL Injection Prevention**: Eloquent ORM parameterized queries
- **XSS Protection**: Input sanitization and output encoding
- **Password Hashing**: Bcrypt/Argon2 hashing

### **API Security**
- **Rate Limiting**: Request throttling
- **CORS Configuration**: Cross-origin resource sharing
- **Input Validation**: Request validation rules
- **API Versioning**: Backward compatibility

## 📱 Progressive Web App (PWA) Architecture

### **PWA Components**
```
PWA Features
├── Service Worker (Offline caching)
├── Web App Manifest (Install prompt)
├── Push Notifications (Real-time alerts)
└── Background Sync (Offline actions)
```

### **Offline Strategy**
- **Cache First**: Static assets (CSS, JS, images)
- **Network First**: Dynamic content (posts, messages)
- **Stale While Revalidate**: User profiles, settings

## 🔄 Real-time Features

### **Broadcasting System**
```
Real-time Events
├── New Message (Private channels)
├── Friend Request (User-specific)
├── Post Likes (Public channels)
└── Online Status (Presence channels)
```

### **WebSocket Integration**
- **Laravel Echo**: Client-side WebSocket handling
- **Pusher/Socket.io**: WebSocket server (configurable)
- **Redis**: Message broadcasting backend

## 📈 Performance Optimization

### **Caching Strategy**
- **Application Cache**: Configuration, routes, views
- **Database Cache**: Query result caching
- **Session Cache**: User session storage
- **CDN**: Static asset delivery

### **Database Optimization**
- **Indexing**: Optimized database indexes
- **Query Optimization**: Eager loading, query scopes
- **Connection Pooling**: Database connection management
- **Read Replicas**: Separate read/write databases (optional)

## 🧪 Testing Architecture

### **Testing Layers**
```
Testing Strategy
├── Unit Tests (Models, Services)
├── Feature Tests (Controllers, APIs)
├── Browser Tests (Laravel Dusk)
└── API Tests (Postman/Swagger)
```

### **Test Environment**
- **In-Memory Database**: SQLite for fast testing
- **Factory Pattern**: Test data generation
- **Mocking**: External service simulation
- **CI/CD Integration**: Automated testing pipeline

## 📊 Monitoring & Analytics

### **Application Monitoring**
- **Laravel Telescope**: Development debugging
- **Error Tracking**: Exception monitoring
- **Performance Monitoring**: Response time tracking
- **User Analytics**: Usage statistics (optional)

### **Infrastructure Monitoring**
- **Server Metrics**: CPU, memory, disk usage
- **Database Monitoring**: Query performance, connections
- **Cache Monitoring**: Hit rates, memory usage
- **Log Aggregation**: Centralized logging

---

This architecture provides a scalable, maintainable, and secure foundation for the Laravel Social Media platform, supporting both current features and future enhancements.
