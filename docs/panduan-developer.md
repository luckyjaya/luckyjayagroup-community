# Panduan Developer - Kontribusi Teknis

## 🎯 Overview

Dokumen ini khusus untuk developer yang tertarik berkontribusi dalam pengembangan teknis LuckyJaya Group applications.

> **Catatan**: Ini berbeda dengan [Panduan Kontribusi](panduan-kontribusi.md) yang ditujukan untuk end users.

---

## 🛠️ Tech Stack

### 📱 Mobile Applications
```
LTech Mobile & AsriJaya Mobile:
├── Frontend: Flutter 3.x
├── State Management: Riverpod/BLoC
├── Database: SQLite (local) + MySQL (remote)
├── API: REST + WebSocket
├── Authentication: JWT + Biometric
└── Deployment: Play Store + App Store
```

### 🖥️ Desktop Applications
```
LuckyJaya Desktop & ArisanManfaat:
├── Frontend: Delphi/Pascal
├── Database: MySQL 8.0+ / SQL Server 2019+
├── Reporting: Crystal Reports
├── Architecture: Multi-tier
└── Deployment: Windows Installer
```

### 🌐 Backend Services
```
ApiLtech:
├── Runtime: Node.js 18+
├── Framework: Express.js
├── Database: MySQL 8.0
├── ORM: Sequelize
├── Authentication: JWT + Passport
├── Documentation: Swagger/OpenAPI
├── Testing: Jest + Supertest
└── Deployment: Docker + PM2
```

---

## 🚀 Getting Started

### 📋 Prerequisites

#### Development Environment
```bash
# Node.js & npm
node --version  # v18+
npm --version   # v9+

# Flutter
flutter --version  # v3.x
dart --version     # v3.x

# Git
git --version

# Database
mysql --version  # v8.0+
```

#### Required Tools
- **IDE**: VS Code (recommended) atau Android Studio
- **Database**: MySQL Workbench atau phpMyAdmin
- **API Testing**: Postman atau Insomnia
- **Git Client**: Command line atau GUI client

### 🔧 Repository Setup

#### 1. Fork & Clone
```bash
# Fork repository di GitHub, kemudian:
git clone https://github.com/YOUR_USERNAME/luckyjayagroup.git
cd luckyjayagroup

# Add upstream remote
git remote add upstream https://github.com/zahrasiska/luckyjayagroup.git
```

#### 2. Environment Setup
```bash
# ApiLtech setup
cd apiltech
cp .env.example .env
npm install

# LTech Mobile setup  
cd ../ltechmobile
flutter pub get

# Database setup
mysql -u root -p < database/schemas/development.sql
```

#### 3. Development Database
```sql
-- Create development database
CREATE DATABASE luckyjaya_dev CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Import test data
SOURCE database/sample_data/development_data.sql;

-- Create test user
CREATE USER 'dev_user'@'localhost' IDENTIFIED BY 'dev_password';
GRANT ALL PRIVILEGES ON luckyjaya_dev.* TO 'dev_user'@'localhost';
FLUSH PRIVILEGES;
```

---

## 📝 Development Workflow

### 🌿 Git Branch Strategy

```
main
├── develop                 # Integration branch
│   ├── feature/new-feature # Feature development
│   ├── bugfix/issue-123   # Bug fixes
│   └── hotfix/critical-fix # Emergency fixes
└── release/v2.1.0         # Release preparation
```

#### Branch Naming Convention
- `feature/feature-name`: New features
- `bugfix/issue-number`: Bug fixes
- `hotfix/critical-issue`: Emergency fixes
- `refactor/component-name`: Code refactoring
- `docs/section-name`: Documentation updates

### 🔄 Commit Guidelines

#### Commit Message Format
```
type(scope): subject

body (optional)

footer (optional)
```

#### Examples
```bash
# Feature
feat(inventory): add barcode scanning functionality

# Bug fix
fix(auth): resolve login timeout issue

# Documentation
docs(api): update authentication endpoints

# Refactor
refactor(database): optimize query performance
```

#### Commit Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Code style (formatting)
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

---

## 🧪 Testing Strategy

### 📱 Mobile Testing (Flutter)

#### Unit Tests
```dart
// test/unit/inventory_test.dart
import 'package:flutter_test/flutter_test.dart';
import 'package:ltechmobile/models/inventory.dart';

void main() {
  group('Inventory Model Tests', () {
    test('should calculate total stock correctly', () {
      final inventory = Inventory(
        itemId: '123',
        warehouse1: 10,
        warehouse2: 15,
      );
      
      expect(inventory.totalStock, equals(25));
    });
  });
}
```

#### Widget Tests
```dart
// test/widget/inventory_widget_test.dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:ltechmobile/widgets/inventory_list.dart';

void main() {
  testWidgets('Inventory list shows items correctly', (WidgetTester tester) async {
    await tester.pumpWidget(MaterialApp(
      home: InventoryList(items: mockInventoryItems),
    ));
    
    expect(find.text('Test Item'), findsOneWidget);
  });
}
```

#### Integration Tests
```dart
// integration_test/app_test.dart
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';
import 'package:ltechmobile/main.dart' as app;

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();
  
  group('App Integration Tests', () {
    testWidgets('login flow works correctly', (WidgetTester tester) async {
      app.main();
      await tester.pumpAndSettle();
      
      // Test login flow
      await tester.enterText(find.byKey(Key('username')), 'testuser');
      await tester.enterText(find.byKey(Key('password')), 'testpass');
      await tester.tap(find.byKey(Key('loginButton')));
      await tester.pumpAndSettle();
      
      expect(find.text('Dashboard'), findsOneWidget);
    });
  });
}
```

### 🌐 Backend Testing (Node.js)

#### Unit Tests
```javascript
// tests/unit/inventory.test.js
const { Inventory } = require('../../models');
const InventoryService = require('../../services/inventory');

describe('Inventory Service', () => {
  test('should create inventory item correctly', async () => {
    const itemData = {
      name: 'Test Item',
      sku: 'TEST001',
      quantity: 100
    };
    
    const result = await InventoryService.createItem(itemData);
    
    expect(result.name).toBe('Test Item');
    expect(result.sku).toBe('TEST001');
  });
});
```

#### Integration Tests
```javascript
// tests/integration/api.test.js
const request = require('supertest');
const app = require('../../app');

describe('Inventory API', () => {
  test('GET /api/inventory should return inventory list', async () => {
    const response = await request(app)
      .get('/api/inventory')
      .set('Authorization', `Bearer ${testToken}`)
      .expect(200);
      
    expect(response.body.success).toBe(true);
    expect(Array.isArray(response.body.data)).toBe(true);
  });
  
  test('POST /api/inventory should create new item', async () => {
    const newItem = {
      name: 'Test Item',
      sku: 'TEST001',
      quantity: 100
    };
    
    const response = await request(app)
      .post('/api/inventory')
      .set('Authorization', `Bearer ${testToken}`)
      .send(newItem)
      .expect(201);
      
    expect(response.body.data.name).toBe('Test Item');
  });
});
```

### 🧪 Running Tests

```bash
# Flutter tests
flutter test                    # Unit & widget tests
flutter test integration_test/  # Integration tests

# Node.js tests
npm test                       # All tests
npm run test:unit             # Unit tests only
npm run test:integration      # Integration tests only
npm run test:coverage         # With coverage report
```

---

## 📚 API Development

### 🔗 REST API Standards

#### Endpoint Naming
```
GET    /api/inventory          # List items
GET    /api/inventory/:id      # Get single item
POST   /api/inventory          # Create item
PUT    /api/inventory/:id      # Update item (full)
PATCH  /api/inventory/:id      # Update item (partial)
DELETE /api/inventory/:id      # Delete item
```

#### Response Format
```javascript
// Success Response
{
  "success": true,
  "data": {
    "id": 123,
    "name": "Item Name",
    "sku": "ITEM001"
  },
  "meta": {
    "timestamp": "2025-01-01T00:00:00Z",
    "version": "2.1.0"
  }
}

// Error Response
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Data tidak valid",
    "details": {
      "name": ["Nama wajib diisi"],
      "sku": ["SKU sudah ada"]
    }
  },
  "meta": {
    "timestamp": "2025-01-01T00:00:00Z",
    "request_id": "req-123456"
  }
}
```

#### Status Codes
```
200 OK           # Successful GET, PUT, PATCH
201 Created      # Successful POST
204 No Content   # Successful DELETE
400 Bad Request  # Validation errors
401 Unauthorized # Authentication required
403 Forbidden    # Access denied
404 Not Found    # Resource not found
409 Conflict     # Resource conflict
422 Unprocessable Entity # Business logic error
500 Internal Server Error # Server error
```

### 📖 API Documentation

#### Swagger/OpenAPI
```yaml
# swagger.yaml example
/api/inventory:
  get:
    summary: Get inventory list
    tags: [Inventory]
    parameters:
      - name: page
        in: query
        schema:
          type: integer
          default: 1
      - name: limit
        in: query
        schema:
          type: integer
          default: 20
    responses:
      200:
        description: Successful response
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/InventoryListResponse'
```

#### JSDoc Comments
```javascript
/**
 * Create new inventory item
 * @route POST /api/inventory
 * @group Inventory - Inventory management
 * @param {InventoryCreateRequest.model} body.body.required - Item data
 * @returns {InventoryResponse.model} 201 - Created item
 * @returns {ErrorResponse.model} 400 - Validation error
 * @security JWT
 */
async function createInventoryItem(req, res) {
  // Implementation
}
```

---

## 🎨 UI/UX Guidelines

### 📱 Flutter Design System

#### Colors
```dart
// lib/theme/colors.dart
class AppColors {
  static const Color primary = Color(0xFF2196F3);
  static const Color secondary = Color(0xFF03DAC6);
  static const Color error = Color(0xFFB00020);
  static const Color surface = Color(0xFFFFFFFF);
  static const Color background = Color(0xFFF5F5F5);
}
```

#### Typography
```dart
// lib/theme/typography.dart
class AppTextStyles {
  static const TextStyle headline1 = TextStyle(
    fontSize: 32,
    fontWeight: FontWeight.bold,
    fontFamily: 'Roboto',
  );
  
  static const TextStyle body1 = TextStyle(
    fontSize: 16,
    fontWeight: FontWeight.normal,
    fontFamily: 'Roboto',
  );
}
```

#### Components
```dart
// lib/widgets/app_button.dart
class AppButton extends StatelessWidget {
  final String text;
  final VoidCallback onPressed;
  final bool isLoading;
  
  const AppButton({
    Key? key,
    required this.text,
    required this.onPressed,
    this.isLoading = false,
  }) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: isLoading ? null : onPressed,
      child: isLoading 
        ? CircularProgressIndicator()
        : Text(text),
    );
  }
}
```

### 🖥️ Desktop UI Guidelines

#### Delphi/Pascal Components
```pascal
// Common button component
procedure TFormBase.CreateStandardButton(
  Parent: TWinControl;
  Caption: string;
  OnClick: TNotifyEvent;
  var Button: TButton
);
begin
  Button := TButton.Create(Parent);
  Button.Parent := Parent;
  Button.Caption := Caption;
  Button.OnClick := OnClick;
  Button.Font.Size := 10;
  Button.Height := 32;
end;
```

---

## 🔒 Security Guidelines

### 🔐 Authentication & Authorization

#### JWT Implementation
```javascript
// middleware/auth.js
const jwt = require('jsonwebtoken');

function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({
      success: false,
      error: { message: 'Token tidak ditemukan' }
    });
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) {
      return res.status(403).json({
        success: false,
        error: { message: 'Token tidak valid' }
      });
    }
    req.user = user;
    next();
  });
}
```

#### Input Validation
```javascript
// validation/inventory.js
const Joi = require('joi');

const createInventorySchema = Joi.object({
  name: Joi.string().min(3).max(100).required(),
  sku: Joi.string().pattern(/^[A-Z0-9]{3,20}$/).required(),
  quantity: Joi.number().integer().min(0).required(),
  price: Joi.number().positive().required()
});

function validateCreateInventory(req, res, next) {
  const { error } = createInventorySchema.validate(req.body);
  if (error) {
    return res.status(400).json({
      success: false,
      error: {
        code: 'VALIDATION_ERROR',
        message: 'Data tidak valid',
        details: error.details
      }
    });
  }
  next();
}
```

#### SQL Injection Prevention
```javascript
// Using parameterized queries
const mysql = require('mysql2/promise');

async function getInventoryById(id) {
  const query = 'SELECT * FROM inventory WHERE id = ?';
  const [rows] = await db.execute(query, [id]);
  return rows[0];
}

// DON'T DO THIS (vulnerable to SQL injection)
// const query = `SELECT * FROM inventory WHERE id = ${id}`;
```

---

## 📊 Performance Guidelines

### 🚀 Database Optimization

#### Indexing Strategy
```sql
-- Primary indexes
CREATE INDEX idx_inventory_sku ON inventory(sku);
CREATE INDEX idx_transactions_date ON transactions(transaction_date);
CREATE INDEX idx_users_email ON users(email);

-- Composite indexes
CREATE INDEX idx_inventory_warehouse_category 
ON inventory(warehouse_id, category_id);

-- Analyze query performance
EXPLAIN SELECT * FROM inventory 
WHERE warehouse_id = 1 AND category_id = 2;
```

#### Query Optimization
```sql
-- Good: Use LIMIT for pagination
SELECT * FROM inventory 
ORDER BY created_at DESC 
LIMIT 20 OFFSET 0;

-- Good: Use EXISTS instead of IN for subqueries
SELECT * FROM products p
WHERE EXISTS (
  SELECT 1 FROM inventory i 
  WHERE i.product_id = p.id AND i.quantity > 0
);

-- Avoid: SELECT *
-- Use: SELECT specific columns
SELECT id, name, sku, quantity FROM inventory;
```

### 📱 Mobile Performance

#### Flutter Optimization
```dart
// Use const constructors
const Text('Static text');

// Optimize list rendering
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ItemWidget(item: items[index]);
  },
);

// Lazy loading images
Image.network(
  imageUrl,
  loadingBuilder: (context, child, loadingProgress) {
    if (loadingProgress == null) return child;
    return CircularProgressIndicator();
  },
);

// Efficient state management
class InventoryNotifier extends StateNotifier<List<Inventory>> {
  InventoryNotifier() : super([]);
  
  void addItem(Inventory item) {
    state = [...state, item]; // Create new list
  }
}
```

### 🌐 API Performance

#### Caching Strategy
```javascript
// Redis caching
const redis = require('redis');
const client = redis.createClient();

async function getInventory(warehouseId) {
  const cacheKey = `inventory:${warehouseId}`;
  
  // Try cache first
  const cached = await client.get(cacheKey);
  if (cached) {
    return JSON.parse(cached);
  }
  
  // Query database
  const data = await db.query(
    'SELECT * FROM inventory WHERE warehouse_id = ?',
    [warehouseId]
  );
  
  // Cache result
  await client.setex(cacheKey, 300, JSON.stringify(data)); // 5 minutes
  
  return data;
}
```

#### Pagination
```javascript
// Efficient pagination
async function getInventoryPaginated(page = 1, limit = 20) {
  const offset = (page - 1) * limit;
  
  const [data, totalCount] = await Promise.all([
    db.query(
      'SELECT * FROM inventory ORDER BY created_at DESC LIMIT ? OFFSET ?',
      [limit, offset]
    ),
    db.query('SELECT COUNT(*) as total FROM inventory')
  ]);
  
  return {
    data,
    pagination: {
      page,
      limit,
      total: totalCount[0].total,
      totalPages: Math.ceil(totalCount[0].total / limit)
    }
  };
}
```

---

## 🚀 Deployment

### 📦 Build Process

#### Flutter Build
```bash
# Android release build
flutter build apk --release
flutter build appbundle --release

# iOS release build
flutter build ios --release

# Web build
flutter build web --release
```

#### Node.js Build
```bash
# Install production dependencies only
npm ci --only=production

# Run tests
npm run test:all

# Build Docker image
docker build -t luckyjaya-api:latest .

# Push to registry
docker push registry.luckyjayagroup.com/api:latest
```

### 🐳 Docker Configuration

#### Dockerfile (Node.js)
```dockerfile
FROM node:18-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production && npm cache clean --force

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# Start application
CMD ["npm", "start"]
```

#### docker-compose.yml
```yaml
version: '3.8'

services:
  api:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=db
      - DB_USER=luckyjaya
      - DB_PASS=${DB_PASSWORD}
    depends_on:
      - db
      - redis
    
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
      MYSQL_DATABASE: luckyjaya
      MYSQL_USER: luckyjaya
      MYSQL_PASSWORD: ${DB_PASSWORD}
    volumes:
      - db_data:/var/lib/mysql
    
  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  db_data:
  redis_data:
```

---

## 📋 Code Review Checklist

### ✅ General
- [ ] Code follows project style guidelines
- [ ] All tests pass
- [ ] No sensitive data committed
- [ ] Documentation updated
- [ ] Performance considered

### ✅ Security
- [ ] Input validation implemented
- [ ] SQL injection prevented
- [ ] Authentication/authorization checked
- [ ] Sensitive data encrypted
- [ ] Error messages don't leak information

### ✅ Performance
- [ ] Database queries optimized
- [ ] Proper indexing used
- [ ] Caching implemented where needed
- [ ] Memory leaks prevented
- [ ] Resource usage efficient

### ✅ Mobile (Flutter)
- [ ] Responsive design
- [ ] Offline capability considered
- [ ] Battery usage optimized
- [ ] Accessibility features
- [ ] Platform-specific code justified

### ✅ API
- [ ] RESTful principles followed
- [ ] Error handling comprehensive
- [ ] Rate limiting considered
- [ ] API documentation updated
- [ ] Breaking changes flagged

---

## 🤝 Getting Help

### 💬 Communication Channels
- **Development Questions**: [GitHub Discussions](https://github.com/zahrasiska/luckyjayagroup-community/discussions)
- **Bug Reports**: [GitHub Issues](https://github.com/zahrasiska/luckyjayagroup-community/issues)
- **Code Review**: GitHub Pull Requests
- **Technical Support**: tech-support@luckyjayagroup.com

### 📚 Additional Resources
- [API Documentation](https://api.luckyjayagroup.com/docs)
- [Flutter Documentation](https://flutter.dev/docs)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [Database Design Guidelines](https://github.com/zahrasiska/luckyjayagroup/wiki/Database-Design)

### 👥 Mentoring Program
- Pair programming sessions dengan senior developers
- Code review mentoring
- Architecture design discussions
- Career development guidance

---

**Selamat berkontribusi! Mari bersama-sama membangun platform ERP terbaik untuk Indonesia! 🚀**
