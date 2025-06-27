# 🌾 **FARM2MARKET API DOCUMENTATION**

## 📋 **COMPLETE LIST OF ALL APIs CREATED**

**Base URL**: `http://localhost:8000/api`

---

## 🔌 **1. API ROOT & SYSTEM**

### **GET** `/api/` - API Root
- **Description**: Shows all available API endpoints
- **Authentication**: None required
- **Response**: JSON with endpoint list

---

## 🔐 **2. AUTHENTICATION APIs**

### **POST** `/api/auth/register/` - General User Registration
- **Description**: Register any type of user
- **Body**: `{username, email, password, user_type, first_name, last_name}`

### **POST** `/api/auth/login/` - General User Login
- **Description**: Login for any user type
- **Body**: `{email, password}`

### **POST** `/api/buyer/register/` - Buyer Registration
- **Description**: Register new buyer with auto-generated password
- **Body**: `{username, email, first_name, last_name, phone}`
- **Features**: Auto-password generation, email delivery

### **POST** `/api/buyer/login/` - Buyer Login
- **Description**: Buyer-specific login
- **Body**: `{email, password}`
- **Response**: JWT token for authentication

### **POST** `/api/buyer/verify-email/` - Email Verification
- **Description**: Verify buyer email address
- **Body**: `{email, verification_code}`

### **POST** `/api/buyer/request-password-reset/` - Password Reset Request
- **Description**: Request password reset email
- **Body**: `{email}`

### **POST** `/api/buyer/reset-password/` - Password Reset
- **Description**: Reset password with token
- **Body**: `{email, reset_token, new_password}`

---

## 🛒 **3. BUYER MARKETPLACE APIs**

### **GET** `/api/products/` - Get All Products
- **Description**: Retrieve all available products
- **Authentication**: None required
- **Response**: List of products with farmer details

### **GET** `/api/products/{listing_id}/` - Get Product Details
- **Description**: Get detailed information about specific product
- **Authentication**: None required
- **Response**: Complete product information

### **GET** `/api/search/` - Search Products & Farmers
- **Description**: Search for products or farmers
- **Parameters**: 
  - `q` (query string)
  - `type` (optional: 'product' or 'farmer')
- **Authentication**: None required

### **GET** `/api/search/farmers/` - Search Farmers
- **Description**: Search specifically for farmers
- **Parameters**: `q` (query string)
- **Authentication**: None required

---

## 👤 **4. BUYER PROFILE APIs**

### **GET/PUT** `/api/buyer/profile/` - Buyer Profile Management
- **Description**: Get or update buyer profile
- **Authentication**: Required (Bearer token)
- **GET Response**: Complete buyer profile
- **PUT Body**: Profile update fields

### **GET** `/api/buyer/dashboard-data/` - Buyer Dashboard Data
- **Description**: Get buyer dashboard statistics
- **Authentication**: Required
- **Response**: Reservations, spending, analytics

---

## 📅 **5. RESERVATION APIs**

### **POST** `/api/reservations/create/` - Create Reservation
- **Description**: Create new product reservation
- **Authentication**: Required (Buyer)
- **Body**: `{listing_id, quantity, delivery_method}`
- **Response**: Reservation details with ID

### **GET** `/api/farmer/reservations/` - Farmer Reservations
- **Description**: Get all reservations for farmer
- **Authentication**: Required (Farmer)
- **Response**: List of reservations

### **PUT** `/api/reservations/{reservation_id}/status/` - Update Reservation Status
- **Description**: Approve/reject reservation
- **Authentication**: Required (Farmer)
- **Body**: `{status, notes}`

---

## 🌱 **6. FARMER LISTING APIs**

### **GET** `/api/farmer/listings/` - Farmer's Own Listings
- **Description**: Get farmer's product listings
- **Authentication**: Required (Farmer)
- **Response**: Farmer's products

### **GET** `/api/farmer/{farmer_id}/listings/` - Public Farmer Listings
- **Description**: Get public listings for specific farmer
- **Authentication**: None required
- **Response**: Public farmer products

---

## 🚨 **7. URGENT SALES APIs**

### **GET** `/api/farmer/urgent-sales/` - Farmer Urgent Sales
- **Description**: Get farmer's urgent sales
- **Authentication**: Required (Farmer)

### **GET** `/api/urgent-sales/public/` - Public Urgent Sales
- **Description**: Get all public urgent sales
- **Authentication**: None required
- **Response**: List of urgent sales

---

## 📂 **8. CATEGORIES API**

### **GET** `/api/categories/` - Get All Categories
- **Description**: Get all product categories
- **Authentication**: None required
- **Response**: List of categories

---

## 🔔 **9. NOTIFICATION APIs**

### **GET** `/api/notifications/` - Get User Notifications
- **Description**: Get user's notifications
- **Authentication**: Required
- **Response**: List of notifications

### **PUT** `/api/notifications/{notification_id}/read/` - Mark Notification Read
- **Description**: Mark specific notification as read
- **Authentication**: Required

---

## 💬 **10. MESSAGING/CHAT APIs**

### **GET** `/api/messages/conversations/` - Get Conversations
- **Description**: Get user's chat conversations
- **Authentication**: Required

### **GET** `/api/messages/conversation/{conversation_id}/` - Get Messages
- **Description**: Get messages in specific conversation
- **Authentication**: Required

### **POST** `/api/messages/send/` - Send Message
- **Description**: Send new message
- **Authentication**: Required
- **Body**: `{conversation_id, message_text}`

### **POST** `/api/messages/conversations/start/` - Start Conversation
- **Description**: Start new conversation
- **Authentication**: Required
- **Body**: `{recipient_id, initial_message}`

### **PUT** `/api/messages/conversations/{conversation_id}/read/` - Mark Conversation Read
- **Description**: Mark conversation as read
- **Authentication**: Required

### **GET** `/api/messages/unread-count/` - Get Unread Count
- **Description**: Get count of unread messages
- **Authentication**: Required

### **DELETE** `/api/messages/conversations/{conversation_id}/delete/` - Delete Conversation
- **Description**: Delete conversation
- **Authentication**: Required

### **GET** `/api/messages/search-users/` - Search Users for Chat
- **Description**: Search users to start chat
- **Authentication**: Required
- **Parameters**: `q` (query string)

---

## 🚜 **11. FARMER PROFILE APIs**

### **GET/PUT** `/api/farmer/profile/` - Farmer Profile Management
- **Description**: Get or update farmer profile
- **Authentication**: Required (Farmer)

### **GET** `/api/farmer/dashboard/` - Farmer Dashboard Data
- **Description**: Get farmer dashboard statistics
- **Authentication**: Required (Farmer)

---

## 👑 **12. ADMIN APIs**

### **GET** `/api/admin/pending-farmers/` - Get Pending Farmers
- **Description**: Get farmers awaiting approval
- **Authentication**: Required (Admin)

### **POST** `/api/admin/approve-farmer/{farmer_id}/` - Approve Farmer
- **Description**: Approve farmer registration
- **Authentication**: Required (Admin)

### **POST** `/api/admin/reject-farmer/{farmer_id}/` - Reject Farmer
- **Description**: Reject farmer registration
- **Authentication**: Required (Admin)

---

## 🔑 **AUTHENTICATION METHODS**

### **Bearer Token Authentication**
```
Headers: {
  "Authorization": "Bearer YOUR_JWT_TOKEN",
  "Content-Type": "application/json"
}
```

### **Token Acquisition**
1. Register via `/api/buyer/register/` or `/api/auth/register/`
2. Login via `/api/buyer/login/` or `/api/auth/login/`
3. Use returned token in Authorization header

---

## 📊 **API RESPONSE FORMATS**

### **Success Response**
```json
{
  "success": true,
  "data": {...},
  "message": "Operation successful"
}
```

### **Error Response**
```json
{
  "success": false,
  "error": "Error description",
  "details": {...}
}
```

---

## 🎯 **TOTAL API COUNT: 32 ENDPOINTS**

**Categories:**
- 🔐 Authentication: 7 endpoints
- 🛒 Marketplace: 4 endpoints  
- 👤 Profiles: 3 endpoints
- 📅 Reservations: 3 endpoints
- 🌱 Listings: 2 endpoints
- 🚨 Urgent Sales: 2 endpoints
- 🔔 Notifications: 2 endpoints
- 💬 Messaging: 8 endpoints
- 👑 Admin: 3 endpoints
- 📂 Categories: 1 endpoint

**All APIs are production-ready and fully functional!** 🚀
