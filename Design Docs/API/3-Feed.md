## 3. Feed

Feed is a stream of fashionable news from users on "following" list.

### 3.1 Posting a feed

**Resource URL**: `POST /api/feed`

**Authority**: authenticated

**Description**

Posting a feed will also creating feed links in the user's account who is currently following the posting user.

**Request Body**

    {
      "caption": "something",
      "media": {
        "type": "Image",
        "url": "https://s3.com/sample.jpeg",
        "size": {
          "width": 1024,
          "height": 1024
        }
      },
      "tags": [
        {
          "product": "someProductExternalId",
          "price_override": 20.44,
          "locationInImage": {
            "x": 512,
            "y": 213
          }
        }
      ]
    }

**Response Body**

    {
      "id": "someExternalId"
    }

### 3.2 Retrieving a feed

**Resource URL**: `GET /api/feed/$feedExternalId`

**Authority**: authenticated

**Response Body (200)**

    {
      "caption": "something",
      "media": {
        "type": "Image",
        "url": "https://s3.com/sample.jpeg",
        "size": {
          "width": 1024,
          "height": 1024
        }
      },
      "tags": [
        {
          "product": "someProductExternalId",
          "price_override": 20.44,
          "locationInImage": {
            "x": 512,
            "y": 213
          }
        }
      ],
      "likes": {
        "count": 1,
        "data": [
          {
            "display_name": "Foo Bar",
            "id": "someUserExternalId"
          }
        ]
      },
      "comments": {
        "count": 1,
        "data": [
          {
            "text": "someComment",
            "author": {
              "display_name": "Foo Bar",
              "id": "someUserExternalId"
            },
            "timestamp": aUnixTimestamp
          }
        ]
      }
    }

**Possible Error Response**

- `404`: not found
- `401`: not authenticated

### 3.3 Post a like

**Resource URL**: `POST /api/feed/$feedExternalId/like`

**Authority**: authenticated

**Response**: `204`

### 3.4 Revoke a like

**Resource URL**: `DELETE /api/feed/$feedExternalId/like`

**Authority**: authenticated

**Response**: `204`

### 3.4 Post a comment

**Resource URL**: `POST /api/feed/$feedExternalId/comment`

**Authority**: authenticated

**Request Body**

    {
      "text": "someComment"
    }

**Response Body (200)**:

    {
      "hash": "hash of user externalId and timestamp",
      "timestamp": aUnixTimestamp
    }

**Error Response**

- `404`: Feed not found

### 3.5 Update a comment

**Resource URL**: `POST /api/feed/$feedExternalId/comment/$commentHash`

**Authority**: author of comment

**Request Body**

    {
      "text": "someComment"
    }

**Response**: `204`

**Error Response**

- `404`: Feed not found or hash not found

### 3.6 Revoke a comment

**Resource URL**: `DELETE /api/feed/$feedExternalId/comment/$commentHash`

**Authority**: author of comment or author or feed

**Response**: `204`

**Error Response**

- `401`: insufficient authorization
- `404`: Feed not found or hash not found

### 3.7 My Feed

**Resource URL**: `GET /api/feed?ref=referenceTimestamp&count=10`

**Authority**: authenticated

**Response**

    {
      "request": {
        "count": 10,
        "reference_time": aUnixTimestamp  
      }
      "result_count": 5,
      "data": []  //content similar to "Retrieving a feed"
    }
