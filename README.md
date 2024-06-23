# ## Eat Safe, Love  README
nosql-challenge Module 12 Challenge
The UK Food Standards Agency evaluates various establishments across the United Kingdom, and gives them a food hygiene rating

# Introduction
Eat Safe, Love is a project that focuses on analyzing and managing food establishment data. This project utilizes a MongoDB database to store information about food establishments, including their names, types, addresses, ratings, and more. The data is provided in a JSON format and can be imported into MongoDB using the provided instructions.

#Part 1: Database and Jupyter Notebook Set Up
To set up the database and Jupyter Notebook, follow these steps:

Import the data provided in the establishments.json file into MongoDB. Use the following command in your Terminal:
css
Copy code
mongoimport --type json -d uk_food -c establishments --drop --jsonArray --file /path/to/establishments.json
Import dependencies in your Jupyter Notebook:
python
Copy code
from pymongo import MongoClient
from pprint import pprint
Create an instance of MongoClient and connect to the uk_food database:
python
Copy code
mongo = MongoClient(port=27017)
db = mongo['uk_food']
Confirm that the uk_food database was created:
python
Copy code
print(db.list_collection_names())
Assign the establishments collection to a variable and review a document in the collection:
python
Copy code
establishments = db['establishments']
pprint(establishments.find_one())

Here's the link to Jupyte Notebook: 
https://github.com/pandarik/nosql-challenge/blob/main/NoSQL_setup_starter.ipynb


## Part 2: Update the Database
In this part, you will add a new restaurant to the database and perform some updates. Follow these steps:

Create a dictionary for the new restaurant data and insert it into the establishments collection:
python
Copy code
new_restaurant = { ... }  # New restaurant data
establishments.insert_one(new_restaurant)
Find the BusinessTypeID for "Restaurant/Cafe/Canteen" and update the new restaurant with the correct BusinessTypeID:
python
Copy code
result = establishments.find_one({"BusinessType": "Restaurant/Cafe/Canteen"}, {"BusinessTypeID": 1})
establishments.update_one({"BusinessName": "Penang Flavours"}, {"$set": {"BusinessTypeID": result["BusinessTypeID"]}})

Here's the link to Jupyte Notebook: 
https://github.com/pandarik/nosql-challenge/blob/main/NoSQL_setup_starter.ipynb



## Part 3: Exploratory Analysis
In this part, you will perform some exploratory analysis on the data. Here are a few examples of the analysis you can perform:

Find establishments with a hygiene score equal to 20:

python
Copy code
query = {"scores.Hygiene": 20}
num_documents = establishments.count_documents(query)
pprint(establishments.find_one(query))
df = pd.DataFrame(list(establishments.find(query)))
Find establishments in London with a RatingValue greater than or equal to 4:

python
Copy code
query = {"LocalAuthorityName": "London", "RatingValue": {"$gte": "4"}}
num_documents = establishments.count_documents(query)
pprint(establishments.find_one(query))
df = pd.DataFrame(list(establishments.find(query)))
Find the top 5 establishments with a RatingValue of 5, sorted by lowest hygiene score, nearest to the new restaurant "Penang Flavours":

python
Copy code
degree_search = 0.01
latitude = 51.490142
longitude = 0.148488
query = {"RatingValue": "5", "geocode.latitude": {"$gte": str(latitude - degree_search), "$lte": str(latitude + degree_search)}, "geocode.longitude": {"$gte": str(longitude - degree_search), "$lte": str(longitude + degree_search)}}
sort = [("scores.Hygiene", 1)]
limit = 5
results = list(establishments.find(query).sort(sort).limit(limit))
Count the number of establishments in each Local Authority area with a hygiene score of 0:

python
Copy code
pipeline = [{"$match": {"scores.Hygiene": 0}}, {"$group": {"_id": "$LocalAuthorityName", "count": {"$sum": 1}}}, {"$sort": {"count": -1}}]
results = list(establishments.aggregate(pipeline))
df = pd.DataFrame(results)

Here's the link to Jupyte Notebook: 
https://github.com/pandarik/nosql-challenge/blob/main/NoSQL_analysis_starter.ipynb


## Conclusion
Eat Safe, Love provides a comprehensive way to manage and analyze food establishment data. By following the instructions provided, you can set up the database, perform updates, and conduct exploratory analysis to gain insights from the data.

# Acknowledgments
Leveraged Google, ChatGPT, and Copoilet as/where needed to develop/validate/troubleshoot code/data/functions. The Python and data science community for their invaluable resources.
