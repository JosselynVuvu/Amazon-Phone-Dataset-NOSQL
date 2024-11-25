# Amazon-Phone-Dataset-NOSQL
Finding insights on Phones distributed on Amazon using NOSQL on MongoDB
# Dataset Link: https://www.kaggle.com/datasets/jakubkhalponiak/phones-2024

1. **AGGREGATION query:** Purpose of the query

What is the business case of the query

#Getting a general first overview of phones per brand to get an idea of the presence of each brands in the market share of our database

Query below

    { $match: { price_usd: { $gte: 0 } } },
    {
      $group: {
        _id: '$phone_brand',
        'Number of phone per brand': { $sum: 1 }
      }
    },
    { $sort: { 'Number of phone per brand': -1 } }**
    

```

1. **AGGREGATION query:** Purpose of the query

```sql

What is the business case of the query

#Now that we know the count of each phones per brand of our database, we are interested in the stores to see how many phones each stores owns
Query below

 { $match: { price_usd: { $gte: 0 } } },
    {
      $group: {
        _id: '$store',
        'Number of phone per store': { $sum: 1 }
      }
    },
    { $sort: { 'Number of phone per store': -1 } }**
```

1. **AGGREGATION query:** Purpose of the query

```sql

What is the business case of the query
# We want the number of phone available for each type of resolution and display the average price per resolution, 
# because we consider that resolution may be one of the most important characteristic for a phone

Query below

    {
      $project: {
        video: { $split: ['$video', ', '] },
        phone_brand: 1,
        price_usd: 1,
        store: 1,
        _id: 0
      }
    },
    { $unwind: { path: '$video' } },
    {
      $group: {
        _id: '$video',
        Average_price: { $avg: '$price_usd' }
      }
    },
    { $sort: { Average_price: -1 }**
    
    
    
```

1. **AGGREGATION query:** Purpose of the query

```sql

What is the business case of the query
# In the Query 2, we highlighted how many phones each store has, now we want to dig and see the most popular brands per store

Query below**

{
      $group: {
        _id: {
          store: '$store',
          brand: '$phone_brand'
        },
        'number of phones': { $sum: 1 }
      }
    },
    { $sort: { 'number of phones': -1 } },
    {
      $group: {
        _id: '$_id.store',
        top_brands: {
          $push: {
            brand: '$_id.brand',
            count: '$number of phones'
          }
        }
      }
    }
```

1. **OPERATORS query:** Purpose of the query

```sql

What is the business case of the query
# We want to know which phones costs more than 1000 USD and which ones are the most expensives.

Query below
    $match: { 
      price_usd: { "$gte": 1000 } 
    } 
  }
  { 
    $project: { 
      phone_brand: 1, 
      phone_model: 1, 
      price_usd: 1 
    } 
  }
  { 
    $sort: { 
      price_usd: -1 
    } 
  }**

```

1. **OPERATORS query:** Purpose of the query

```sql

What is the business case of the query
# We want to know which phones costs more than 1000 USD and which ones are the less expensives.

Query below
$match: { 
      price_usd: { "$lte": 1000 } 
    } 
  }
  { 
    $project: { 
      phone_brand: 1, 
      phone_model: 1, 
      price_usd: 1 
    } 
  }
  { 
    $sort: { 
      price_usd: 1 
    } 
  }**

```

1. **OPERATORS query:** Purpose of the query

```sql

What is the business case of the query
# For the next queries, we are searching basics informations, mostly the average price based on the different caracteristics of a phone
# We want to know the average price per ram

Query below

{
      $group: {
        _id: '$ram',
        AveragePrice: { $avg: '$price_usd' }
      }
    },
    { $sort: { AveragePrice: -1 } }**

```

1. **OPERATORS query:** Purpose of the query

```sql

What is the business case of the query
# We want to know the average price per storage

Query below
 {
      $group: {
        _id: '$storage',
        AveragePrice: { $avg: '$price_usd' }
      }
    },
    { $sort: { AveragePrice: -1 } }**
```

1. **OPERATORS query:** Purpose of the query

```sql

What is the business case of the query
# We want to know the average price per os

Query below**

 **{
      $group: {
        _id: '$os',
        AveragePrice: { $avg: '$price_usd' }
      }
    },
    { $sort: { AveragePrice: -1 } }**
```

1. **OPERATORS query:** Purpose of the query

```sql

What is the business case of the query
# We want to know the average price per os_type

Query below
{
      $group: {
        _id: '$os_type',
        AveragePrice: { $avg: '$price_usd' }
      }
    },
    { $project: { os_type: 1 }, {AveragePrice: 1} },
    { $sort: { AveragePrice: 1 } }**

```

1. **$EXPR query:** Purpose of the query

```sql

What is the business case of the query
# Identify which brands are making their own GPU to know which brands are dependant.

Query below

$expr: {
    $eq: [
        { $toLower: "$phone_brand" },
        { $toLower: "$gpu" }
    ]
},
{ $project: { phone_brand: 1 }, {gpu: 1} }**

```

1. **$EXPR query:** Purpose of the query

```sql

What is the business case of the query
# We know to know which phones were launched during 2024 to avoid any miscompliance with regulation previous than 2024.

Query below

{
    $match: {
      $expr: {
        $gte: ["$launch_date", new Date("2024-01-01")]
      }
    }
  },
  {
    $project: { 
      phone_brand: 1,
      phone_model: 1,
      launch_date: 1
    }
  },
  {
    $sort: { 
      launch_date: -1
    }
  }**
```

1. **ARRAYS query:** Purpose of the query

```sql

What is the business case of the query
# We want to know which models propose the most different colors

Query below

  {
  $project: {
    phone_brand: 1,
    phone_model: 1,
    number_of_colors: {
      $size: { $split: ["$colors", ", "] }
		    } 
  }
},
{
  $sort: { number_of_colors: -1 }
}**

```

1. **ARRAYS query:** Purpose of the query

```sql

What is the business case of the query
We want to know how which phones has the highest level of security when it comes to face recognizing such as FaceID or Iris Scanner

Query below

      $project: {
        features: {
          $split: ['$features_sensors', ', ']
        },
        phone_brand: 1,
        phone_model: 1
      }
    },
    {
      $match: {
        features: {
          $in: ['Face ID', 'Iris scanner']
        }
      }
    },
    { $sort: { phone_brand: -1 } }**
 

```

1. **ARRAYS query:** Purpose of the query

```sql

What is the business case of the query
# We want to know which phones has a lightning usb port but also a classic usb port at the same time, this type of phone can be a good option for people who are not so much into technology
Query below

      $project: {
        usb_connection: {
          $split: ['$usb', ', ']
        },
        phone_brand: 1,
        phone_model: 1
      }
    },
    {
      $match: {
        usb_connection: {
          $all: ['Lightning', 'USB 2.0']
        }
      }**
   

```

1. **ARRAYS query:** Purpose of the query

```sql

What is the business case of the query
# We want to know which phones propose 4K at 30 FPS and 8k at 30 fps for resolution

Query below

    {
      $project: {
        video: { $split: ['$video', ', '] },
        phone_brand: 1,
        phone_model: 1
      }
    },
    {
      $match: {
        video: { $all: ['8K@30fps', '4K@30fps'] }
      }
    },
    { $sort: { phone_brand: -1 } }**

```

**Link for the Chart Dashboard** 
https://charts.mongodb.com/charts-nosql-taygztg/public/dashboards/67360ced-9048-4bf1-8787-3573713c8012
```
