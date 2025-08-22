```markdown
# UBER System Design (Simplified)

## Functional Requirements

### Core Requirements
1. Riders should be able to input a start location and a destination and get a fare estimate.  
2. Riders should be able to request a ride based on the estimated fare.  
3. Upon request, riders should be matched with a driver who is nearby and available.  
4. Drivers should be able to accept/decline a request and navigate to pickup/drop-off.  

### Below the Line (Out of Scope for Now)
1. Riders should be able to rate their ride and driver post-trip.  
2. Drivers should be able to rate passengers.  
3. Riders should be able to schedule rides in advance.  
4. Riders should be able to request different categories of rides (e.g., X, XL, Comfort).  

---

## Non-Functional Requirements

### Core Requirements
1. The system should prioritize **low latency matching** (< 1 minute to match or failure).  
2. The system should ensure **strong consistency** in ride matching to prevent any driver from being assigned multiple rides simultaneously.  
3. The system should handle **high throughput**, especially during peak hours or special events (e.g., 100k requests from the same location).  

### Below the Line (Out of Scope for Now)
1. The system should ensure the **security and privacy** of user and driver data, complying with regulations like GDPR.  
2. The system should be **resilient to failures**, with redundancy and failover mechanisms.  
3. The system should have **robust monitoring, logging, and alerting**.  
4. The system should allow **easy updates and maintenance** without significant downtime (CI/CD pipelines).  

---

## Defining the Core Entities

At this stage, we focus on the **primary entities** rather than specific columns/fields.

1. **Rider**  
   - Represents users requesting rides.  
   - Attributes: Name, contact info, preferred payment methods.  

2. **Driver**  
   - Represents users providing transportation.  
   - Attributes: Personal details, vehicle info (make, model, year), preferences, availability.  

3. **Fare**  
   - Represents an estimated fare for a ride.  
   - Attributes: Pickup, destination, estimated fare, ETA.  
   - Could also be merged into `Ride`, but kept separate here.  

4. **Ride**  
   - Represents a full ride lifecycle.  
   - Attributes: Rider, driver, vehicle details, route, fare, timestamps, state.  

5. **Location**  
   - Represents real-time driver location updates.  
   - Attributes: Latitude, longitude, last updated timestamp.  
   - Used for matching and ride tracking.  

---

## API / System Interface

### 1. Get Fare Estimate  
**Endpoint:**  
```

POST /fare

````

**Body:**  
```json
{
  "pickupLocation": "...",
  "destination": "..."
}
````

**Response:** `Fare` object with estimated fare and ETA.

---

### 2. Request Ride

**Endpoint:**

```
POST /rides
```

**Body:**

```json
{
  "fareId": "..."
}
```

**Response:** `Ride` object with ride details and pending driver assignment.

---

### 3. Update Driver Location

**Endpoint:**

```
POST /drivers/location
```

**Body:**

```json
{
  "lat": "...",
  "long": "..."
}
```

**Notes:**

* `driverId` is taken from session cookie or JWT (not request body).

---

### 4. Accept / Deny Ride Request

**Endpoint:**

```
PATCH /rides/:rideId
```

**Body:**

```json
{
  "status": "accept" | "deny"
}
```

**Response:** Updated `Ride` object with driver assignment or rejection.

---

### 5. Update Ride Status

**Endpoint:**

```
PATCH /ride/driver/update
```

**Body:**

```json
{
  "rideId": "...",
  "status": "en_route" | "picked_up" | "completed" | "cancelled"
}
```

---

## Next Steps

* Define detailed schema for entities (tables/fields).
* Add ride-matching algorithm design.
* Extend APIs for out-of-scope features (ratings, categories, scheduling).

```
<img width="1014" height="734" alt="Screenshot 2025-08-22 at 8 48 03 PM" src="https://github.com/user-attachments/assets/2d40f1f7-db12-470b-a34f-8b181f0655e2" />
![Uploading Screenshot 2025-08-22 at 8.46.07 PM.png…]()

<img width="819" height="665" alt="Screenshot 2025-08-22 at 8 35 45 PM" src="https://github.com/user-attachments/assets/cda4ee1b-b484-4368-ae5b-4818d941c25f" />
