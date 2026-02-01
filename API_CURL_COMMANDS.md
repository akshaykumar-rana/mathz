# User API - cURL Commands

Base URL: `http://localhost:7005`

## 1. Register

```bash
curl -X POST http://localhost:7005/user/register \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "John",
    "lastName": "Doe",
    "userName": "johndoe123",
    "email": "john.doe@example.com",
    "password": "Password123!",
    "dob": "1990-01-01",
    "useReferCode": "REF123456"
  }'
```

**Response:**
- Returns `accessToken`, `refreshToken`, and user data
- User is automatically logged in after registration

---

## 2. Login

```bash
curl -X POST http://localhost:7005/user/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john.doe@example.com",
    "password": "Password123!",
    "device": 3,
    "pushToken": "fcm_token_here",
    "isForceLogin": false
  }'
```

**Response:**
- Returns `accessToken` and user data
- Sets cookies for `accessToken` and `refreshToken`

**Note:** 
- `device`: 1 = Android, 2 = iOS, 3 = Web
- `isForceLogin`: true to logout from other device and login

---

## 3. Forgot Password - Request OTP

```bash
curl -X POST http://localhost:7005/user/forgot-password \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john.doe@example.com"
  }'
```

**Response:**
- Returns success message: "OTP sent to your email."
- OTP is valid for 10 minutes
- Can only request once every 24 hours

---

## 4. Verify Forgot Password OTP

```bash
curl -X POST http://localhost:7005/user/verify-forgot-password-otp \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john.doe@example.com",
    "otp": "123456"
  }'
```

**Response:**
- Returns success message: "OTP verified successfully"
- OTP must match and not be expired (10 minutes validity)

---

## 5. Reset Password

```bash
curl -X POST http://localhost:7005/user/reset-password \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john.doe@example.com",
    "otp": "123456",
    "newPassword": "NewPassword123!",
    "confirmPassword": "NewPassword123!"
  }'
```

**Response:**
- Returns success message: "Password reset successfully"
- Password must match confirmPassword
- OTP must be valid and not expired
- Password requirements: min 6 chars, must contain uppercase, number, and special character

---

## Additional APIs

### Get Profile

```bash
curl -X GET "http://localhost:7005/user/get-profile?userId=USER_ID_HERE" \
  -H "Content-Type: application/json"
```

### Change Password

```bash
curl -X POST http://localhost:7005/user/change-password \
  -H "Content-Type: application/json" \
  -d '{
    "userId": "USER_ID_HERE",
    "oldPassword": "Password123!",
    "newPassword": "NewPassword123!",
    "confirmPassword": "NewPassword123!"
  }'
```

### Update Profile

```bash
curl -X PATCH http://localhost:7005/user/edit-profile \
  -H "Content-Type: application/json" \
  -d '{
    "userId": "USER_ID_HERE",
    "firstName": "John Updated",
    "lastName": "Doe Updated",
    "userName": "johndoe456",
    "dob": "1990-01-15"
  }'
```

### Get App Settings

```bash
curl -X GET http://localhost:7005/user/app-settings \
  -H "Content-Type: application/json"
```

### Logout

```bash
curl -X PATCH http://localhost:7005/user/logout \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "userId: USER_ID_HERE"
```

### Delete Account

```bash
curl -X POST http://localhost:7005/user/delete-account \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "userId: USER_ID_HERE" \
  -d '{
    "password": "Password123!"
  }'
```

---

## Environment Variables

Replace the following in the commands:
- `USER_ID_HERE` - User ID from login/register response
- `YOUR_ACCESS_TOKEN` - Access token from login/register response
- `john.doe@example.com` - Your email address
- `Password123!` - Your password
