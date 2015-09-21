## 1. User
A user can be either a customer, a salesperson or an IT internal user. All users are granted `USER` role. Customers are implicitly granted `CUSTOMER` role. Salespersons are implicitly granted `SALESPERSON` role. Internal users are implicitly granted `ADMIN` role.

### 1.1 Customer creation

**Resource URL**: `POST /api/user?type=customer`

**Description**

Anyone with a valid referral code can create a customer account representing himself. Once registered, customer will need to verify the email using the confirmation link sent to his email to unlock his account.

**Authority**: none

**Request Body**

    {
      "email": "foo@bar.com",
      "password": "someSecret",
      "referral_code": "foo",
      "source": "IT",
      "name": {
        "first": "Foo",
        "last": "Bar"
      }
    }

**Successful Response Body (200)**

    {
      "email": "foo@bar.com",
      "created": 1442865931,
      "external_id": "some-uuid"
    }

**Possible Error Conditions**

- `400`: Request does not meet requirements
- `403`: Invalid referral code
- `409`: User already exists (by email)
- `500`: Internal error

### 1.2 Salesperson creation

**Resource URL**: `POST /api/user?type=salesperson`

**Authority**: person who owns the referral code requires `SALESPERSON` role or `ADMIN` role

**Description**

Verified salesperson can register using a referral code issued by another salesperson or an admin. They are automatically verified.

**Request Body**

    {
      "email": "foo@bar.com",
      "password": "someSecret",
      "referral_code": "foo",
      "name": {
        "first": "Foo",
        "last": "Bar"
      },
      "phone": "somePhoneNumber",
      "brand_id": "someBrandExternalId"
    }

**Successful Response Body (200)**

    {
      "email": "foo@bar.com",
      "created": 1442865931,
      "external_id": "some-uuid",
      "api_key": "some-api-key"
    }

**Possible Error Conditions**

- `400`: Request does not meet requirements
- `401`: Insufficient authorization
- `403`: Invalid referral code
- `409`: User already exists (by email)
- `500`: Internal error

### 1.3 Internal user creation

**Resource URL**: `POST /api/user?type=internal`

**Authority**: person who owns the referral code requires `ADMIN` role

**Request Body**

    {
      "email": "foo@bar.com",
      "password": "someSecret",
      "referral_code": "foo",
      "name": {
        "first": "Foo",
        "last": "Bar"
      }
    }

**Successful Response Body (200)**

    {
      "email": "foo@bar.com",
      "created": 1442865931,
      "external_id": "some-uuid",
      "api_key": "some-api-key"
    }

**Possible Error Conditions**

- `400`: Request does not meet requirements
- `401`: Insufficient authorization
- `403`: Invalid referral code
- `409`: User already exists (by email)
- `500`: Internal error

### 1.4 User Authentication

**Resource URL**: `POST /api/user/authentication`

**Authority**: none

**Request Body**

    {
      "email": "foo@bar.com",
      "password": "someSecret"
    }

**Response Body (200)**

    {
      "email": "foo@bar.com",
      "api_key": "some-api-key"
    }

**Possible Error Conditions**

- `400`: Request does not meet requirements
- `401`: Authentication failed
- `500`: Internal error

### 1.5 User (Email) Verification

**Resource URL**: `GET /verification?token=$someTokenFromEmail`

**Description**

This is not a API endpoint, but still included here as it's an integral part of customer registration.

### 1.6 Get account information

**Resource URL**: `GET /api/user/me`

**Authority**: authenticated

**Response Body (200)**

    {
      "email": "foo@bar.com",
      "name": {
        "first": "Foo",
        "last": "Bar"
      },
      "referral_code": "myReferralCode"
    }

**Error Response**

- `401`: Not authenticated
- `500`: Server error

### 1.7 Update account information

**Resource URL**: `PUT /api/user/me`

**Authority**: authenticated

**Request Body**

    {
      "name": {
        "first": "Foo",
        "last": "Bar"
      }
    }

**Response Codes**

- `204`: successful
- `401`: not authenticated
- `500`: Server error

### 1.8 Password Reset

**Resource URL**: `POST /api/user/reset`

**Authority**: none

**Description**

This will lookup the account by the email and send the user
a link to reset his password.

**Request Body**

    {
      "email": "foo@bar.com"
    }

**Response Code**

- `202`: Request accepted
