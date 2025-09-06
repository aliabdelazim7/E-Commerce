# E-Commerce - Professional Full Stack Application

A professional, full-stack ecommerce application built with React, Node.js, Express, and SQLite. This application provides a complete shopping experience with user authentication, product management, shopping cart, and order processing. **Ready for production deployment!**

## Features

### 🛍️ Shopping Experience

- **Product Catalog**: Browse products with search and category filtering
- **Product Details**: Detailed product pages with images and descriptions
- **Shopping Cart**: Add/remove items, adjust quantities, and view cart total
- **Checkout Process**: Complete orders with user authentication

### 👤 User Management

- **User Registration**: Create new accounts with email validation
- **User Login**: Secure authentication with JWT tokens
- **Order History**: View past orders with detailed information
- **Profile Management**: User profile and order tracking

### 🎨 Modern UI/UX

- **Responsive Design**: Works perfectly on desktop, tablet, and mobile
- **Modern Interface**: Clean, intuitive design with smooth animations
- **Real-time Updates**: Cart updates and order status changes
- **Loading States**: Smooth loading indicators throughout the app

### 🔧 Backend Features

- **RESTful API**: Complete API for all ecommerce operations
- **Database Management**: SQLite database with proper relationships
- **Authentication**: JWT-based authentication with password hashing
- **Order Processing**: Complete order lifecycle management

## Tech Stack

### Frontend

- **React 18**: Modern React with hooks and functional components
- **React Router**: Client-side routing
- **Axios**: HTTP client for API communication
- **React Icons**: Beautiful icon library
- **CSS3**: Modern styling with flexbox and grid

### Backend

- **Node.js**: JavaScript runtime
- **Express.js**: Web framework
- **SQLite3**: Lightweight database
- **bcryptjs**: Password hashing
- **jsonwebtoken**: JWT authentication
- **CORS**: Cross-origin resource sharing

## Installation & Setup

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn package manager

### 1. Clone the Repository

```bash
git clone <repository-url>
cd e-commerce
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

### 3. Start the Application

#### Development Mode (Recommended)

```bash
# Start both frontend and backend simultaneously
npm run dev
```

#### Separate Development Servers

```bash
# Terminal 1: Start backend server
npm run server

# Terminal 2: Start frontend development server
npm run client
```

### 4. Access the Application

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000

## Project Structure

```
e-commerce/
├── server/                 # Backend code
│   ├── index.js           # Main server file
│   ├── database.js        # Database setup and initialization
│   └── routes/            # API routes
│       ├── auth.js        # Authentication routes
│       ├── products.js    # Product management routes
│       └── orders.js      # Order management routes
├── client/                # Frontend React application
│   ├── public/            # Static files
│   ├── src/               # React source code
│   │   ├── components/    # Reusable components
│   │   ├── context/       # React context providers
│   │   ├── pages/         # Page components
│   │   └── App.js         # Main App component
│   └── package.json       # Frontend dependencies
├── package.json           # Backend dependencies
└── README.md             # This file
```

## API Endpoints

### Authentication

- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `GET /api/auth/me` - Get current user

### Products

- `GET /api/products` - Get all products
- `GET /api/products/:id` - Get single product
- `GET /api/products/categories/list` - Get product categories
- `POST /api/products` - Create product (Admin)
- `PUT /api/products/:id` - Update product (Admin)
- `DELETE /api/products/:id` - Delete product (Admin)

### Orders

- `POST /api/orders` - Create new order
- `GET /api/orders/my-orders` - Get user orders
- `GET /api/orders/:id` - Get order details
- `PUT /api/orders/:id/status` - Update order status (Admin)

## Database Schema

### Users Table

- `id` (Primary Key)
- `email` (Unique)
- `password` (Hashed)
- `name`
- `role`
- `created_at`

### Products Table

- `id` (Primary Key)
- `name`
- `description`
- `price`
- `image_url`
- `category`
- `stock`
- `created_at`

### Orders Table

- `id` (Primary Key)
- `user_id` (Foreign Key)
- `total_amount`
- `status`
- `created_at`

### Order Items Table

- `id` (Primary Key)
- `order_id` (Foreign Key)
- `product_id` (Foreign Key)
- `quantity`
- `price`

## Usage Guide

### For Customers

1. **Browse Products**: Visit the home page or products page to see available items
2. **Search & Filter**: Use the search bar and category filters to find specific products
3. **Add to Cart**: Click the cart icon on product cards or use the "Add to Cart" button on product details
4. **Manage Cart**: View cart, adjust quantities, and remove items
5. **Checkout**: Login/register and complete your order
6. **Track Orders**: View order history and status in the "My Orders" section

### For Administrators

1. **Product Management**: Add, edit, or remove products through the API
2. **Order Management**: Update order status and view all orders
3. **User Management**: Monitor user accounts and activity

## Product Catalog

The application features a comprehensive product catalog with **65 products** across **6 categories**:

### **Product Categories**

- **Electronics** (12 products) - Smartphones, laptops, TVs, accessories
- **Clothing** (10 products) - T-shirts, jackets, shoes, handbags
- **Sports** (9 products) - Fitness equipment, sports gear
- **Home** (10 products) - Appliances, smart home devices
- **Books** (8 products) - Educational and entertainment books
- **Beauty** (8 products) - Skincare, makeup, personal care
- **Toys** (8 products) - Games, educational toys, electronics

### **Price Range**

- **Budget**: $14.99 - $49.99
- **Mid-Range**: $50.00 - $299.99
- **Premium**: $300.00 - $1,299.99

## Environment Variables

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

**Important:** Change the JWT_SECRET to a strong, unique value in production!

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

If you encounter any issues or have questions, please:

1. Check the console for error messages
2. Ensure all dependencies are installed
3. Verify the database is properly initialized
4. Check that both frontend and backend servers are running

## Security Features

- **Helmet.js**: Security headers protection
- **Rate Limiting**: 100 requests per 15 minutes per IP
- **Input Validation**: Comprehensive validation using express-validator
- **Password Hashing**: bcrypt with configurable rounds
- **JWT Authentication**: Secure token-based authentication
- **CORS Protection**: Configurable cross-origin resource sharing
- **SQL Injection Protection**: Parameterized queries
- **XSS Protection**: Input sanitization and output encoding

## Performance Optimizations

- **Database Indexes**: Optimized queries with proper indexing
- **Connection Pooling**: Efficient database connections
- **Error Handling**: Comprehensive error management
- **Logging System**: Structured logging for monitoring
- **Caching**: Static file serving optimization

## Production Deployment

### **Quick Production Setup**

1. **Navigate to dist folder**:

   ```bash
   cd dist
   ```

2. **Install production dependencies**:

   ```bash
   npm install
   ```

3. **Start production server**:

   ```bash
   node deploy.js
   ```

4. **Access your application**: http://localhost:5000

### **Production Features**

- ✅ **Optimized Build** - Minified and compressed assets
- ✅ **Security Headers** - Helmet.js protection
- ✅ **Rate Limiting** - DDoS protection
- ✅ **Error Handling** - Comprehensive error management
- ✅ **Database** - Pre-populated with 65 products

## Documentation

- **[API Documentation](API_DOCUMENTATION.md)**: Complete API reference
- **[Deployment Guide](DEPLOYMENT_GUIDE.md)**: Detailed production deployment instructions
- **[Final Project Review](FINAL_PROJECT_REVIEW.md)**: Complete project overview and handover guide

## Future Enhancements

- Payment gateway integration (Stripe, PayPal)
- Email notifications
- Admin dashboard
- Product reviews and ratings
- Wishlist functionality
- Advanced search and filtering
- Inventory management
- Shipping calculator
- Multi-language support
- Redis caching
- Docker containerization
- Microservices architecture
