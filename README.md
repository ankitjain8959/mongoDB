# MongoDB
MongoDB Documentation

# Aggregation Queries
> Count of documents
- Total count of documents
```
[
  {
    "$count": "totalDocuments"
  }
]
```

- Total count of documents for a customer
```
[
  {
    "$match": {
      "relatedParty.id": "123456"
    }
  },
  {
    "$count": "totalCount"
  }
]
```

> Document size
- Raw Document Size (BSON) - in byte
```
[
  {
    "$match": {
      "_id": "123456789012"
    }
  },
  {
    "$project": {
      "documentSize": {
        "$bsonSize": "$$ROOT"
      }
    }
  }
]
```

- Raw Document Size (BSON) - in KB
```
[
  {
    "$match": {
      "_id": "559461287499"
    }
  },
  {
    "$project": {
      "documentSizeKB": {
        "$divide": [
          { "$bsonSize": "$$ROOT" },
          1024
        ]
      }
    }
  }
]
```

- Largest document size (BSON)
```
[
  {
    "$project": {
      "_id": 1,
      "documentSize": {
        "$bsonSize": "$$ROOT"
      }
    }
  },
  {
    "$sort": {
      "documentSize": -1
    }
  },
  {
    "$limit": 1
  }
]
```

- Sum of total size of all documents
```
[
  {
    "$project": {
      "_id": 1,
      "documentSize": {
        "$bsonSize": "$$ROOT"
      }
    }
  },
  {
    "$group": {
      "_id": null,
      "totalSize": {
        "$sum": "$documentSize"
      }
    }
  }
]
```

> Fetch all customers
```
[
  {
    "$unwind": "$relatedParty"
  },
  {
    "$match": {
      "relatedParty.role": "Customer"
    }
  },
  {
    "$project": {
      "_id": 0,
      "relatedPartyId": "$relatedParty.id"
    }
  }
]
```


