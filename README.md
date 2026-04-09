🧪 MongoDB Experiments
❤️ Charity Donation & Campaign Platform
 # EXPERIMENT – 1: CRUD Operations (Donor Management)
## Aim
To perform CRUD operations for managing donor details.

## Description
In a charity platform, donor information like ID, name, donation amount, and status is stored and managed.

 Query
```js
 use charityDB

// CREATE - Add donors
db.donors.insertOne({
  donorId: "DON101",
  name: "Arun Kumar",
  campaign: "Education Fund",
  amount: 5000,
  isActive: true
})

db.donors.insertMany([
  {
    donorId: "DON102",
    name: "Priya Sharma",
    campaign: "Health Care",
    amount: 8000,
    isActive: true
  },
  {
    donorId: "DON103",
    name: "Rahul Singh",
    campaign: "Food Drive",
    amount: 3000,
    isActive: false
  }
])

#// READ
db.donors.find()
db.donors.find({ amount: { $gt: 4000 } })

#// UPDATE
db.donors.updateOne(
  { donorId: "DON101" },
  { $set: { amount: 6000 } }
)

db.donors.updateMany(
  { isActive: true },
  { $set: { status: "Active" } }
)

// DELETE
db.donors.deleteOne({ donorId: "DON103" })
db.donors.deleteMany({ isActive: false })
```

### Output
Donor records inserted, retrieved, updated, and deleted successfully.
### Result
CRUD operations for donor management were successfully performed.

# EXPERIMENT – 2: Collection Creation (Platform Setup)
## Aim
To create and manage collections for the charity platform.

### Description
Collections such as donors, campaigns, and donations are required.

🔹 Query
```js
use charityDB

db.createCollection("donors")

show collections

db.donors.drop()
```
### Output
Collection created, displayed, and dropped successfully.

### Result
Platform collections were successfully managed.

# EXPERIMENT – 3: Donor Schema Validation
## Aim
To enforce required fields for donor data.

### Description
Ensures donors must provide ID, name, and donation amount.

##3 Query
```js
db.createCollection("donors", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["donorId", "name", "amount"],
      properties: {
        donorId: { bsonType: "string" },
        name: { bsonType: "string" },
        amount: { bsonType: "int" },
        isActive: { bsonType: "bool" }
      }
    }
  }
})
```
### Output
Invalid donor data is rejected.

### Result
Data integrity for donors was maintained.

# EXPERIMENT – 4: Campaign Validation (Advanced Rules)
## Aim
To apply validation rules for campaigns.

### Description
Campaigns must follow rules like valid ID, type, and funding limits.

### Query
```js
db.createCollection("campaigns", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["campaignId", "campaignName", "targetAmount"],
      properties: {
        campaignId: {
          bsonType: "string",
          pattern: "^CMP[0-9]{3}$"
        },
        campaignName: {
          enum: ["Education Fund", "Health Care", "Food Drive", "Disaster Relief"]
        },
        targetAmount: {
          bsonType: "number",
          minimum: 10000,
          maximum: 10000000
        }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
})
```
### Output
Valid campaigns accepted, invalid ones rejected.

### Result
Campaign validation rules were successfully applied.

# EXPERIMENT – 5: Donation Tracking Validation
## Aim
To validate donation records.

### Description
Tracks donations with donor ID, campaign ID, date, and status.

### Query
```js
db.createCollection("donations")

db.runCommand({
  collMod: "donations",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["donorId", "campaignId", "date", "status"],
      properties: {
        donorId: {
          bsonType: "string",
          pattern: "^DON[0-9]{3}$"
        },
        campaignId: {
          bsonType: "string",
          pattern: "^CMP[0-9]{3}$"
        },
        date: { bsonType: "string" },
        status: {
          enum: ["Completed", "Pending", "Failed"]
        }
      }
    }
  },
  validationLevel: "moderate"
})
```
### Output
Only valid donation records are stored.

### Result
Donation tracking validation was successfully implemented.

# EXPERIMENT – 6: View & Verify Validation Rules
## Aim
To view validation rules applied to campaigns collection.

### Query
```js
db.getCollectionInfos({ name: "campaigns" })
```
### Output
Displays validation schema including required fields, enum values, and patterns.

### Result
Validation rules were successfully verified.


👉 This project represents a Charity Donation & Campaign Platform:

🔹 Modules:
👤 Donors → Manage donor details

🎯 Campaigns → Fundraising campaigns

💰 Donations → Track contributions

🔹 Features:
Data validation ensures accuracy

Prevents invalid donations

Supports real-time campaign tracking



