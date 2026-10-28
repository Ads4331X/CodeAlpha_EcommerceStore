# CodeAlpha Ecommerce Store - Jewelry E-Commerce Platform

A full-stack e-commerce platform built for selling gold, silver, and other jewelry products. Features a modern customer storefront, admin dashboard, and integrated payment gateways (eSewa & Khalti).

## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [API Documentation](#api-documentation)
- [Payment Integration](#payment-integration)
- [Deployment](#deployment)
- [License](#license)

## Overview

This is a complete jewelry e-commerce solution with dual interfaces:
- **Customer Storefront** - Browse products, manage cart/wishlist, place orders, and make payments
- **Admin Dashboard** - Manage products, orders, customers, rates, and site settings

The platform calculates jewelry prices dynamically based on metal weight, current metal rates, making charges, wastage percentage, and VAT.

## Tech Stack

### Backend
- **Framework**: [Express.js](https://expressjs.com/) 5.2.1
- **Database**: [MySQL](https://www.mysql.com/) with [Prisma ORM](https://www.prisma.io/) 5.22.0
- **Authentication**: JWT (JSON Web Tokens)
- **File Storage**: [Vercel Blob](https://vercel.com/storage/blob)
- **Payment Gateways**: eSewa, Khalti
- **Email**: [Nodemailer](https://nodemailer.com/)
- **Validation**: [express-validator](https://express-validator.github.io/)

### Frontend
- **Framework**: [React](https://react.dev/) 19.2.6 with [TypeScript](https://www.typescriptlang.org/)
- **Build Tool**: [Vite](https://vitejs.dev/) 8.0.12
- **UI Libraries**: [Material UI](https://mui.com/) 9.0.1, [Tailwind CSS](https://tailwindcss.com/) 4.3.0
- **Routing**: [React Router DOM](https://reactrouter.com/) 7.15.1
- **Carousel**: [Swiper.js](https://swiperjs.com/)

## Features

### Customer Features

#### Authentication & Account Management
- **Customer Registration** - Sign up with email and phone number
- **Email Verification (OTP)** - Secure account activation via OTP
- **Login/Logout** - JWT-based authentication
- **Password Recovery** - Forgot password with OTP verification
- **Change Password** - Update password from profile
- **Profile Management** - Update personal information
- **Address Management** - Add, edit, delete, and set default shipping addresses

#### Shopping Experience
- **Product Catalog** - Browse jewelry products with images
- **Product Details** - View weight, purity, metal type, making charges, and calculated price
- **Category Filtering** - Filter products by categories and subcategories
- **Product Search** - Search functionality across product catalog
- **Featured Products & Deals of Day** - Highlighted product sections
- **Product Reviews** - Read and write reviews with ratings
- **Wishlist** - Save products for later
- **Shopping Cart** - Add/remove/update quantities

#### Checkout & Orders
- **Multi-step Checkout** - Secure checkout with address selection
- **Multiple Payment Methods**:
  - Cash on Delivery (COD)
  - eSewa (Nepal's digital wallet)
  - Khalti (Nepal's digital wallet)
- **Order History** - View all past orders with status
- **Order Details** - Track order items, payment status, and delivery info
- **Order Tracking** - Monitor order status through the pipeline

#### Content & Communication
- **Home Page** - Hero section, featured products, metal rates display
- **About Us** - Company story and craftsmanship showcase
- **Contact Form** - Send inquiries to admin
- **Live Metal Rates** - Real-time gold and silver prices

### Admin Features

#### Dashboard & Analytics
- **Overview Dashboard** - Key metrics and statistics
- **Order Statistics** - Sales analytics and order trends
- **Recent Orders** - Quick view of latest orders

#### Product Management
- **Product CRUD** - Create, read, update, and delete products
- **Image Management** - Upload multiple product images to Vercel Blob
- **Category Management** - Manage product categories and subcategories
- **Pricing Configuration**:
  - Set metal type (Gold, Silver, Diamond, Platinum, Other)
  - Configure weight in grams
  - Set purity (e.g., 24K, 22K)
  - Define making charges (Fixed amount or Percentage)
  - Set wastage percentage
  - Configure VAT percentage (default 13%)
- **Stock Management** - Track inventory levels
- **Low Stock Alerts** - Automated notifications for low stock items
- **Featured/Deals** - Mark products as featured or deals of the day
- **Product Activation** - Enable/disable products

#### Order Management
- **All Orders View** - Comprehensive order listing
- **Order Status Updates** - Update through workflow: PENDING → CONFIRMED → PROCESSING → READY_FOR_DELIVERY → OUT_FOR_DELIVERY → DELIVERED
- **Order Cancellation/Refund** - Handle cancellations and refunds
- **Payment Status Tracking** - Monitor payment states
- **Order Details** - View customer info, items, pricing breakdown

#### User & Access Control
- **Admin Authentication** - Secure admin login
- **Role-based Access Control (RBAC)**:
  - `SUPER_ADMIN` - Full system access
  - `ADMIN` - Standard administrative access
  - `DELIVERY_STAFF` - Limited delivery-focused access
- **Admin Account Management** - Create, edit, activate/deactivate admin accounts
- **Change Password** - Admin password management
- **Password Recovery** - Admin forgot password flow

#### Site Configuration
- **Metal Rates Management** - Update gold and silver rates per gram (affects live pricing)
- **Site Settings** - Configure site-wide settings
- **Enquiry/Contact Management** - View and respond to customer inquiries
- **Review Moderation** - Manage product reviews

## Project Structure

```
CodeAlpha_EcommerceStore/
├── backend/                    # Express.js API server
│   ├── api/                    # Vercel serverless entry point
│   ├── config/                 # Database and configuration
│   │   └── prisma.js           # Prisma client instance
│   ├── middleware/             # Auth and custom middleware
│   │   └── authMiddleware.js   # JWT verification
│   ├── prisma/                 # Database schema and migrations
│   │   ├── schema.prisma       # Data models and relationships
│   │   └── migrations/         # Migration history
│   ├── routes/                 # API route handlers
│   │   ├── admin/              # Admin-specific routes
│   │   ├── customer/           # Customer-specific routes
│   │   ├── products/           # Product routes
│   │   ├── esewa.js            # eSewa payment routes
│   │   ├── khalti.js           # Khalti payment routes
│   │   └── inquery/            # Contact inquiry routes
│   ├── services/               # Business logic
│   │   ├── paymentPending.js   # Payment intent handling
│   │   └── emailService.js     # Email notifications
│   ├── utils/                  # Utility functions
│   ├── app.js                  # Express app configuration
│   ├── server.js               # Server entry point
│   └── uploadroute.js          # Image upload handling
│
└── frontend/                   # React + Vite application
    ├── src/
    │   ├── components/         # Reusable UI components
    │   │   ├── layout/         # Header, Footer, Layout
    │   │   ├── shared/         # Protected routes, utilities
    │   │   └── ui/             # MUI/Tailwind components
    │   ├── features/           # Feature-based modules
    │   │   ├── admin/          # Admin dashboard
    │   │   ├── auth/           # Authentication (admin/customer)
    │   │   ├── customer/       # Customer pages (cart, checkout, orders)
    │   │   ├── products/       # Product listing/details
    │   │   ├── home/           # Homepage components
    │   │   ├── about/          # About page
    │   │   └── contact/        # Contact page
    │   ├── services/           # API calls and utilities
    │   ├── utils/              # Helper functions
    │   └── App.tsx             # App routing configuration
    └── public/                # Static assets
```

## Getting Started

### Prerequisites
- Node.js (v18 or higher)
- MySQL database
- npm or yarn

### Backend Setup

1. Navigate to backend directory:
```bash
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables (see [Environment Variables](#environment-variables))

4. Run Prisma migrations:
```bash
npx prisma migrate deploy
```

5. Generate Prisma client:
```bash
npx prisma generate
```

6. Start development server:
```bash
npm run dev
```

The backend will run on `http://localhost:5000` by default.

### Frontend Setup

1. Navigate to frontend directory:
```bash
cd frontend
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables

4. Start development server:
```bash
npm run dev
```

The frontend will run on `http://localhost:5173` by default.

### Running Both Simultaneously

From the frontend directory, you can run both static and customer dev servers:
```bash
npm run dev:all
```

## Environment Variables

### Backend (.env)

```env
# Database
DATABASE_URL="mysql://user:password@host:port/database"

# Authentication
JWT_SECRET="your_jwt_secret_key"

# Frontend URL (for CORS and payment callbacks)
FRONTEND_URL="http://localhost:5173"

# Vercel Blob Storage
BLOB_READ_WRITE_TOKEN="your_vercel_blob_token"

# Email Configuration (Nodemailer)
EMAIL_HOST="smtp.example.com"
EMAIL_PORT=587
EMAIL_USER="your_email@example.com"
EMAIL_PASS="your_email_password"
EMAIL_FROM="noreply@example.com"

# Payment Gateways
ESEWA_SECRET="your_esewa_secret_key"
KHALTI_SECRET_KEY="your_khalti_secret_key"
```

### Frontend (.env)

```env
# Backend API URL
VITE_API_URL="http://localhost:5000"
```

## API Documentation

### Base URL
- Development: `http://localhost:5000/api`
- Production: Your deployed backend URL

### Key Endpoints

#### Authentication
- `POST /api/customer/auth/login` - Customer login
- `POST /api/customer/signup` - Customer registration
- `POST /api/admin/auth/login` - Admin login
- `POST /api/customer/signup/verify` - Verify OTP
- `POST /api/customer/forgot-password` - Request password reset

#### Products
- `GET /api/products` - List all products (with filters)
- `GET /api/products/:slug` - Get product details
- `GET /api/categories` - List categories

#### Cart & Orders
- `GET /api/customer/cart` - Get cart
- `POST /api/customer/cart` - Add to cart
- `POST /api/customer/orders` - Create order (COD)
- `GET /api/customer/orders` - Get user orders

#### Payments
- `POST /api/esewa/initiate` - Initiate eSewa payment
- `GET /api/esewa/verify` - eSewa payment verification
- `POST /api/khalti/initiate` - Initiate Khalti payment
- `GET /api/khalti/verify` - Khalti payment verification

#### Admin
- `GET /api/admin/products` - Manage products
- `GET /api/admin/orders` - Manage orders
- `GET /api/admin/metal-rates` - Manage metal rates
- `GET /api/admin/site-settings` - Manage site settings

## Payment Integration

### eSewa
- Uses eSewa ePay v2 API
- Test merchant ID: `EPAYTEST`
- Signature generated using HMAC-SHA256
- Flow: Initiate → Redirect to eSewa → Verify callback

### Khalti
- Uses Khalti Payment API
- Initiates payment and verifies via Khalti's verification endpoint
- Supports amount in paisa (1 NPR = 100 paisa)

### Payment Flow
The platform uses a `PendingPayment` model to handle payments securely:
1. Customer initiates checkout with cart items and address
2. Backend creates a `PendingPayment` record (validates stock, calculates pricing)
3. User is redirected to payment gateway
4. On success, gateway redirects back with payment data
5. Backend verifies signature/token and calls `finalizePendingPayment()` to create order
6. Stock is deducted and order is created atomically

## Pricing Calculation

Jewelry prices are calculated dynamically using the following formula:

```javascript
metalCost = currentRatePerGram × weightGrams
wastageAmt = metalCost × (wastagePercent / 100)
makingAmt = makingChargeType === 'FIXED' 
  ? makingCharge 
  : metalCost × (makingCharge / 100)
subtotal = metalCost + wastageAmt + makingAmt
vatAmt = subtotal × (vatPercent / 100)  // Default 13% in Nepal
finalPrice = subtotal + vatAmt
```

Current metal rates are fetched from the `MetalRate` table (managed by admin).

## Live Demo

- **Frontend**: [https://codealpha-ecommerce-store-zeta.vercel.app/](https://codealpha-ecommerce-store-zeta.vercel.app/)
- **Backend API**: [https://codealpha-ecommerce-api.vercel.app/](https://codealpha-ecommerce-api.vercel.app/)

## Deployment

### Backend (Vercel)
1. Create Vercel project with `backend` as root directory
2. Set environment variables in Vercel dashboard
3. Add MySQL database (via Vercel Marketplace or external provider)
4. Create Vercel Blob store for image uploads
5. Deploy - API served via `api/index.js`

### Frontend (Vercel)
1. Create Vercel project with `frontend` as root directory
2. Set `VITE_API_URL` to backend deployment URL
3. Deploy

### Database Migrations
After deployment, run migrations on production database:
```bash
cd backend
npx prisma migrate deploy
```

## License

This project is proprietary. All rights reserved.

## Contributing

This project was developed as part of CodeAlpha internship program.
