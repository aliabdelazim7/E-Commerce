# E-Commerce - Professional Full Stack Application
### Live Demo: https://simple-ecommerce-task45.netlify.app/

A professional, full-stack ecommerce application built with React, Node.js, Express, and SQLite. This application provides a complete shopping experience with user authentication, product management, shopping cart, and order processing. **Ready for production deployment!**

## Features

### 🛍️ Shopping Experience

- **Product Catalog**: Browse products with search and category filtering
- **Product Details**: Detailed product pages with images and descriptions
- **Shopping Cart**: Add, remove, and update items with real-time total calculation
- **Checkout Process**: Complete order processing with order confirmation
- **Order History**: View past orders and their status

### 👤 User Management

- **User Registration**: Create new accounts with email validation
- **User Login**: Secure authentication with JWT tokens
- **Profile Management**: Update user information and preferences
- **Order Tracking**: Track order status and delivery information

### 🛠️ Technical Features

- **Responsive Design**: Mobile-first design that works on all devices
- **Modern UI**: Clean, professional interface with smooth animations
- **Real-time Updates**: Dynamic cart updates and inventory management
- **Search & Filter**: Advanced product search and category filtering
- **Image Optimization**: Optimized product images for fast loading

## Installation & Setup

### Prerequisites

- Node.js (v14 or higher)
- npm (v6 or higher)

### 1. Clone the Repository

```bash
git clone https://github.com/aliabdelazim7/E-Commerce.git
cd E-Commerce
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

```bash
# Start both frontend and backend
npm run dev

# Or start them separately:
npm run server  # Backend only
npm run client  # Frontend only
```

### 4. Access the Application

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000/api

## Project Structure

```
E-Commerce/
├── server/                 # Backend code
│   ├── routes/            # API routes
│   ├── middleware/         # Custom middleware
│   ├── database.js        # Database setup
│   └── index.js           # Server entry point
├── client/                # Frontend code
│   ├── src/
│   │   ├── components/    # React components
│   │   ├── pages/         # Page components
│   │   ├── context/       # React Context
│   │   └── App.js         # Main App component
│   └── public/            # Static files
├── dist/                  # Production build
└── package.json           # Root package.json
```

## Product Catalog

The application features a comprehensive product catalog with **130 products** across **multiple categories**:

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
   node server.js
   ```

4. **Access your application**: http://localhost:5000

### **Production Features**

- ✅ **Optimized Build** - Minified and compressed assets
- ✅ **Security Headers** - Helmet.js protection
- ✅ **Rate Limiting** - DDoS protection
- ✅ **Error Handling** - Comprehensive error management
- ✅ **Database** - Pre-populated with 130 products

## Future Enhancements

- [ ] Payment gateway integration (Stripe, PayPal)
- [ ] Email notifications for orders
- [ ] Admin dashboard for product management
- [ ] Product reviews and ratings
- [ ] Wishlist functionality
- [ ] Multi-language support
- [ ] Advanced analytics and reporting
- [ ] Mobile app development

**Built with ❤️ using React, Node.js, Express, and SQLite**

