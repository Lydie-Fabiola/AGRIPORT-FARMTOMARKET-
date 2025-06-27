# Buyer Authentication System - Testing Guide

## Overview
This document outlines the complete buyer authentication system implementation and provides testing instructions for all authentication features.

## 🚀 Features Implemented

### 1. **Backend API Endpoints**
- ✅ `POST /api/buyer/register/` - Buyer registration with auto-generated password
- ✅ `POST /api/buyer/login/` - Buyer login with token generation
- ✅ `POST /api/buyer/verify-email/` - Email verification
- ✅ `POST /api/buyer/request-password-reset/` - Password reset request
- ✅ `POST /api/buyer/reset-password/` - Password reset confirmation
- ✅ `GET/PUT /api/buyer/profile/` - Buyer profile management
- ✅ `GET /api/buyer/dashboard-data/` - Dashboard statistics

### 2. **Database Models**
- ✅ Enhanced `BuyerProfile` model with additional fields
- ✅ `EmailVerificationToken` model for email verification
- ✅ `PasswordResetToken` model for password reset
- ✅ Proper relationships and constraints

### 3. **Email Service Integration**
- ✅ HTML email templates for welcome, verification, and password reset
- ✅ Email utility functions for different notification types
- ✅ Automatic password generation and email delivery
- ✅ Professional email styling with Farm2Market branding

### 4. **Frontend Authentication**
- ✅ Enhanced buyer registration form with API integration
- ✅ Enhanced buyer login form with API integration
- ✅ Real-time form validation and error handling
- ✅ Loading states and user feedback
- ✅ Responsive design and error styling

### 5. **Session Management**
- ✅ `BuyerAuth` class for authentication state management
- ✅ Token-based authentication with localStorage
- ✅ Automatic session validation and redirect
- ✅ Logout functionality with cleanup
- ✅ Protected route handling

## 🧪 Testing Instructions

### Prerequisites
1. **Start Django Server**
   ```bash
   cd AGRIPORT
   python manage.py runserver
   ```

2. **Run Database Migrations**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

3. **Configure Email Settings** (Optional for testing)
   - For development: Uses console backend (emails print to console)
   - For production: Configure SMTP settings in `settings.py`

### Test Case 1: Buyer Registration
**Objective**: Test complete buyer registration flow

**Steps**:
1. Navigate to `signupbuyer.html`
2. Fill out the registration form:
   - First Name: "John"
   - Last Name: "Buyer"
   - Email: "john.buyer@example.com"
   - Phone: "237612345678"
   - Location: "Douala, Cameroon"
3. Click "Create Account"

**Expected Results**:
- ✅ Form validation passes
- ✅ API call to `/api/buyer/register/` succeeds
- ✅ Success message displays
- ✅ Welcome email sent (check console output)
- ✅ User redirected to login page after 3 seconds
- ✅ User account created in database with auto-generated password

### Test Case 2: Email Verification
**Objective**: Test email verification process

**Steps**:
1. Check console output for verification email
2. Copy the verification URL from the email
3. Navigate to the verification URL
4. Or test via API: `POST /api/buyer/verify-email/` with token

**Expected Results**:
- ✅ Email verification succeeds
- ✅ User account activated
- ✅ Success notification created

### Test Case 3: Buyer Login
**Objective**: Test buyer login with generated credentials

**Steps**:
1. Navigate to `loginbuyer.html`
2. Enter email and password from welcome email
3. Click "Sign In"

**Expected Results**:
- ✅ Form validation passes
- ✅ API call to `/api/buyer/login/` succeeds
- ✅ Authentication token stored in localStorage
- ✅ User data stored in localStorage
- ✅ Success message displays
- ✅ Redirected to buyer dashboard

### Test Case 4: Dashboard Access Control
**Objective**: Test authentication protection on dashboard

**Steps**:
1. Navigate directly to `buyerdashboard.html` without logging in
2. Try accessing dashboard after login
3. Test logout functionality

**Expected Results**:
- ✅ Unauthenticated users redirected to login
- ✅ Authenticated users can access dashboard
- ✅ User info displayed correctly on dashboard
- ✅ Logout clears session and redirects to login

### Test Case 5: Password Reset
**Objective**: Test password reset functionality

**Steps**:
1. Use API endpoint: `POST /api/buyer/request-password-reset/`
   ```json
   {
     "email": "john.buyer@example.com"
   }
   ```
2. Check console for reset email
3. Use reset token with: `POST /api/buyer/reset-password/`
   ```json
   {
     "token": "reset_token_here",
     "new_password": "newpassword123",
     "confirm_password": "newpassword123"
   }
   ```

**Expected Results**:
- ✅ Reset email sent successfully
- ✅ Password reset with valid token succeeds
- ✅ Can login with new password
- ✅ Old password no longer works

### Test Case 6: Form Validation
**Objective**: Test client-side and server-side validation

**Steps**:
1. Test registration form with invalid data:
   - Empty fields
   - Invalid email format
   - Invalid phone number
   - Duplicate email
2. Test login form with invalid credentials

**Expected Results**:
- ✅ Client-side validation prevents submission
- ✅ Server-side validation returns appropriate errors
- ✅ Error messages display correctly
- ✅ Form styling updates for invalid fields

### Test Case 7: Session Persistence
**Objective**: Test session management across page reloads

**Steps**:
1. Login successfully
2. Refresh the page
3. Navigate to different pages
4. Close and reopen browser

**Expected Results**:
- ✅ Session persists across page reloads
- ✅ User remains logged in during browser session
- ✅ Protected pages remain accessible
- ✅ User info displays consistently

## 🐛 Common Issues and Solutions

### Issue 1: CORS Errors
**Solution**: Ensure Django CORS settings allow frontend domain

### Issue 2: Email Not Sending
**Solution**: Check email backend configuration in `settings.py`

### Issue 3: Token Validation Fails
**Solution**: Verify token format and expiration in database

### Issue 4: Database Migration Errors
**Solution**: Delete migration files and recreate:
```bash
rm coreF2M/migrations/00*.py
python manage.py makemigrations
python manage.py migrate
```

## 📊 API Testing with Postman/curl

### Registration
```bash
curl -X POST http://localhost:8000/api/buyer/register/ \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "John",
    "last_name": "Buyer", 
    "email": "john@example.com",
    "phone_number": "237612345678",
    "location": "Douala"
  }'
```

### Login
```bash
curl -X POST http://localhost:8000/api/buyer/login/ \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "generated_password"
  }'
```

### Dashboard Data (Authenticated)
```bash
curl -X GET http://localhost:8000/api/buyer/dashboard-data/ \
  -H "Authorization: Bearer your_token_here"
```

## ✅ Test Completion Checklist

- [ ] All API endpoints respond correctly
- [ ] Database models created and functional
- [ ] Email templates render properly
- [ ] Frontend forms validate and submit
- [ ] Authentication flow works end-to-end
- [ ] Session management functions correctly
- [ ] Error handling works as expected
- [ ] User experience is smooth and intuitive

## 🎯 Next Steps

After completing authentication testing:
1. Integrate chat functionality for buyers
2. Connect buyer dashboard to real product data
3. Implement reservation system integration
4. Add buyer profile management features
5. Set up real-time notifications

---

**Note**: This authentication system provides a solid foundation for the Farm2Market buyer experience with security, usability, and scalability in mind.
