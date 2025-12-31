# Travel Packages API Documentation

This document describes the API endpoints and data formats required for the Trip Holiday packages system.

## Overview

The packages.html page has been converted to load data dynamically from an API. This allows administrators to manage packages through a backend system without editing HTML/JavaScript code.

## API Endpoints

### Base URL
```
/api
```

### 1. Get All Packages
**Endpoint:** `GET /api/packages`

**Description:** Returns all travel packages and their itineraries

**Response Format:**
```json
{
  "packages": [
    {
      "id": "dubai",
      "title": "Dubai Luxury Escape",
      "description": "Experience the luxury of Dubai with 5-star hotels, desert safari, and Burj Khalifa",
      "image": "https://example.com/images/dubai.jpg",
      "price": 150000,
      "duration": 7,
      "durationText": "7 Days / 6 Nights",
      "type": "luxury",
      "destination": "international",
      "travel": "family",
      "popular": true,
      "popularBadge": "Most Popular",
      "tags": ["Luxury", "International"]
    }
  ],
  "itineraries": {
    "dubai": {
      "title": "Dubai Luxury Escape",
      "subtitle": "7 Days / 6 Nights",
      "days": [
        {
          "day": 1,
          "title": "Arrival in Dubai",
          "description": "Arrive at Dubai International Airport...",
          "highlights": ["Airport Transfer", "Hotel Check-in", "Welcome Drink"]
        }
      ]
    }
  }
}
```

## Data Model

### Package Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | string | Yes | Unique identifier for the package (used for itineraries) |
| title | string | Yes | Package name/title |
| description | string | Yes | Short description of the package |
| image | string | Yes | URL to package image (recommended: 800x600px) |
| price | number | Yes | Package price in INR |
| duration | number | Yes | Number of days |
| durationText | string | No | Custom duration text (defaults to "X Days / Y Nights") |
| type | string | Yes | Package type: "luxury", "premium", or "budget" |
| destination | string | Yes | Destination category: "india", "international", or "pilgrimage" |
| travel | string | Yes | Travel type: "family", "couple", "solo", "group", or "buddy" |
| popular | boolean | No | Whether package is popular (shows badge) |
| popularBadge | string | No | Custom popular badge text (defaults to "Most Popular") |
| tags | array | Yes | Array of tag strings to display (e.g., ["Luxury", "International"]) |

### Itinerary Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Yes | Itinerary title |
| subtitle | string | Yes | Itinerary subtitle (usually duration) |
| days | array | Yes | Array of day objects |

### Day Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| day | number | Yes | Day number (1, 2, 3, etc.) |
| title | string | Yes | Day title |
| description | string | Yes | Detailed description of the day's activities |
| highlights | array | Yes | Array of highlight strings |

## Filter Values

### Package Types
- `luxury` - Luxury packages
- `premium` - Premium packages
- `budget` - Budget-friendly packages

### Destinations
- `india` - Domestic India packages
- `international` - International packages
- `pilgrimage` - Pilgrimage/Spiritual packages

### Travel Types
- `family` - Family-friendly packages
- `couple` - Romantic couple packages
- `solo` - Solo traveler packages
- `group` - Group tour packages
- `buddy` - Friend/buddy trips

## Implementation Notes

### Fallback Data
The current implementation includes fallback data functions (`getFallbackPackages()` and `getFallbackItineraries()`) that will be used if the API is not available. This allows the site to function during development and provides a graceful degradation if the API fails.

### Error Handling
The page includes built-in error handling:
- Loading spinner while fetching data
- Error messages if API fails
- "No packages found" message when filters return no results
- Fallback to local data if API is unreachable

### CORS Configuration
Ensure your API server has proper CORS headers configured to allow requests from the frontend domain.

Example headers:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type
```

## Frontend Configuration

To change the API endpoint, update the API_CONFIG object in packages.html:

```javascript
const API_CONFIG = {
    baseURL: 'https://your-api-domain.com/api', // Change this
    endpoints: {
        packages: '/packages',
        itineraries: '/itineraries' // Currently bundled with packages
    }
};
```

## Testing

### Test the API endpoint:
```bash
curl https://your-api-domain.com/api/packages
```

### Expected Response:
Should return JSON with `packages` and `itineraries` objects as specified above.

## Admin Panel Integration

To create an admin panel for managing packages:

1. Create backend endpoints for CRUD operations:
   - `POST /api/admin/packages` - Create new package
   - `PUT /api/admin/packages/:id` - Update existing package
   - `DELETE /api/admin/packages/:id` - Delete package
   - `GET /api/admin/packages/:id` - Get single package for editing

2. Implement authentication/authorization for admin endpoints

3. Create an admin interface that:
   - Lists all packages
   - Allows adding/editing/deleting packages
   - Manages itineraries for each package
   - Uploads and manages package images

## Sample API Response

```json
{
  "packages": [
    {
      "id": "dubai",
      "title": "Dubai Luxury Escape",
      "description": "Experience the luxury of Dubai with 5-star hotels, desert safari, and Burj Khalifa",
      "image": "https://images.unsplash.com/photo-1512453979798-5ea266f8880c?w=800&h=600&fit=crop&auto=format",
      "price": 150000,
      "duration": 7,
      "durationText": "7 Days / 6 Nights",
      "type": "luxury",
      "destination": "international",
      "travel": "family",
      "popular": true,
      "popularBadge": "Most Popular",
      "tags": ["Luxury", "International"]
    },
    {
      "id": "goa",
      "title": "Goa Beach Paradise",
      "description": "Relax on pristine beaches, enjoy water sports, and explore Portuguese heritage",
      "image": "https://images.unsplash.com/photo-1512343879784-a960bf40e7f2?w=800&h=600&fit=crop&auto=format",
      "price": 37000,
      "duration": 5,
      "durationText": "5 Days / 4 Nights",
      "type": "premium",
      "destination": "india",
      "travel": "couple",
      "popular": false,
      "tags": ["Premium", "India"]
    }
  ],
  "itineraries": {
    "dubai": {
      "title": "Dubai Luxury Escape",
      "subtitle": "7 Days / 6 Nights",
      "days": [
        {
          "day": 1,
          "title": "Arrival in Dubai",
          "description": "Arrive at Dubai International Airport. Meet and greet by our representative. Transfer to your 5-star hotel. Check-in and relax. Evening at leisure to explore nearby areas.",
          "highlights": ["Airport Transfer", "Hotel Check-in", "Welcome Drink", "Leisure Time"]
        },
        {
          "day": 2,
          "title": "Dubai City Tour",
          "description": "Morning city tour covering Burj Khalifa, Dubai Mall, and Dubai Fountain. Visit the historic Al Fahidi district and Dubai Museum.",
          "highlights": ["Burj Khalifa", "Dubai Mall", "Dubai Museum", "Dhow Cruise"]
        }
      ]
    },
    "goa": {
      "title": "Goa Beach Paradise",
      "subtitle": "5 Days / 4 Nights",
      "days": [
        {
          "day": 1,
          "title": "Arrival in Goa",
          "description": "Arrive at Goa International Airport. Transfer to your beach resort.",
          "highlights": ["Airport Transfer", "Resort Check-in", "Beach Walk"]
        }
      ]
    }
  }
}
```

## Support

For questions or issues with the API integration, please contact the development team.
