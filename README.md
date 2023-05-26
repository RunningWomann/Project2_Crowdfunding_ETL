# Project2_Crowdfunding_ETL

![istockphoto-1361894912-612x612](https://github.com/cassiecontreras/Crowdfunding-ETL/assets/126130038/c0e74ca1-2f24-476c-9873-884599ef45a2)


### Team Members: Chun Zhao, Jing Xu, Jerrett Williams, Cassie Contreras

### Solution is in ETL_Mini_Project_Final_Code.
We used a relational database, utilizing python in jupyter notebook and sql in pgadmin.

## Description

For the ETL mini project, you will work with a partner to practice building an ETL pipeline using Python, Pandas, and either Python dictionary methods or regular expressions to extract and transform the data. After you transform the data, you'll create four CSV files and use the CSV file data to create an ERD and a table schema. Finally, you’ll upload the CSV file data into a Postgres database.

## Data Sources
(CSV Files in Resources Folder)

campaign.csv

category.csv

contacts.csv

contacts.xlsx

crowdfunding.xlsx

subcategory.csv

## Subsections:
### Create the Category and Subcategory DataFrames

**Create a Category DataFrame that has the following columns:**

+ A "category_id" column that is numbered sequential form 1 to the length of the number of unique categories.
+ A "category" column that has only the categories.
Export the DataFrame as a category.csv CSV file.

**Create a SubCategory DataFrame that has the following columns:**

+ A "subcategory_id" column that is numbered sequential form 1 to the length of the number of unique subcategories.
+ A "subcategory" column that has only the subcategories.
Export the DataFrame as a subcategory.csv CSV file.

```
# Get the crowdfunding_info_df columns.
crowdfunding_info_df.columns
```
Index(['cf_id', 'contact_id', 'company_name', 'blurb', 'goal', 'pledged',
       'outcome', 'backers_count', 'country', 'currency', 'launched_at',
       'deadline', 'staff_pick', 'spotlight', 'category & sub-category'],
      dtype='object')

```      
# Assign the category and subcategory values to category and subcategory columns.
crowdfunding_info_df[["category","subcategory"]]  = crowdfunding_info_df["category & sub-category"].str.split('/', n=1, expand=True)
crowdfunding_info_df.head(5)
```  

```      
# Get the unique categories and subcategories in separate lists.
categories = crowdfunding_info_df["category"].unique()
subcategories = crowdfunding_info_df["subcategory"].unique()

print(categories)
print(subcategories)
```  
['food' 'music' 'technology' 'theater' 'film & video' 'publishing' 'games'
 'photography' 'journalism']
['food trucks' 'rock' 'web' 'plays' 'documentary' 'electric music' 'drama'
 'indie rock' 'wearables' 'nonfiction' 'animation' 'video games' 'shorts'
 'fiction' 'photography books' 'radio & podcasts' 'metal' 'jazz'
 'translations' 'television' 'mobile games' 'world music'
 'science fiction' 'audio']
 
 ```
# Get the number of distinct values in the categories and subcategories lists.
print(len(categories))
print(len(subcategories))
```
9
24

```
# Create numpy arrays from 1-9 for the categories and 1-24 for the subcategories.
category_ids = np.arange(1, 10)
subcategory_ids = np.arange(1, 25)

print(category_ids)
print(subcategory_ids)
```

[1 2 3 4 5 6 7 8 9]
[ 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24]

```
# Use a list comprehension to add "cat" to each category_id. 
cat_ids = ["cat" + str(cat_id) for cat_id in category_ids]
# Use a list comprehension to add "subcat" to each subcategory_id.    
scat_ids = ["subcat" + str(scat_id) for scat_id in subcategory_ids ]
    
print(cat_ids)
print(scat_ids)
```
['cat1', 'cat2', 'cat3', 'cat4', 'cat5', 'cat6', 'cat7', 'cat8', 'cat9']
['subcat1', 'subcat2', 'subcat3', 'subcat4', 'subcat5', 'subcat6', 'subcat7', 'subcat8', 'subcat9', 'subcat10', 'subcat11', 'subcat12', 'subcat13', 'subcat14', 'subcat15', 'subcat16', 'subcat17', 'subcat18', 'subcat19', 'subcat20', 'subcat21', 'subcat22', 'subcat23', 'subcat24']

```
# Create a category DataFrame with the category_id array as the category_id and categories list as the category name.
category_df = pd.DataFrame({
    "category_id": cat_ids,
    "category" : categories
})
# Create a category DataFrame with the subcategory_id array as the subcategory_id and subcategories list as the subcategory name. 
subcategory_df = pd.DataFrame({
    "subcategory_id": scat_ids,
    "subcategory" : subcategories
})

category_df

subcategory_df


# Export categories_df and subcategories_df as CSV files.
category_df.to_csv("Resources/category.csv", index=False)

subcategory_df.to_csv("Resources/subcategory.csv", index=False)

```


### Create the Campaign DataFrame

* Create a Campaign DataFrame that has the following columns:

+ The "cf_id" column.
+ The "contact_id" column.
+ The “company_name” column.
+ The "blurb" column is renamed as "description."
+ The "goal" column.
+ The "goal" column is converted to a float datatype.
+ The "pledged" column is converted to a float datatype.
+ The "backers_count" column.
+ The "country" column.
+ The "currency" column.
+ The "launched_at" column is renamed as "launch_date" and converted to a datetime format.
+ The "deadline" column is renamed as "end_date" and converted to a datetime format.
+ The "category_id" with the unique number matching the “category_id” from the category DataFrame.
+ The "subcategory_id" with the unique number matching the “subcategory_id” from the subcategory DataFrame.
+ And, create a column that contains the unique four-digit contact ID number from the contact.xlsx file.
+ Then export the DataFrame as a campaign.csv CSV file.

```bash
* Create a copy of the crowdfunding_info_df DataFrame name campaign_df. 
campaign_df = crowdfunding_info_df.copy()
campaign_df.head()
```
<img width="487" alt="image" src="https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126307180/d03f12d8-9eb2-40d6-b4b0-605c094b6c94">

```bash
* Rename the blurb, launched_at, and deadline columns.
campaign_df = campaign_df.rename(columns={'blurb':'description', 'launched_at': 'launch_date',  'deadline': 'end_date'})
campaign_df.head()
```
<img width="485" alt="image" src="https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126307180/566b5eb2-6555-451d-bbfc-959daef4c855">

```bash
* Convert the goal and pledged columns to a `float` data type.
campaign_df[['goal','pledged']] = campaign_df[['goal','pledged']].astype(float)
campaign_df.head()
```
<img width="481" alt="image" src="https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126307180/623884ca-cfb8-4e05-8c80-8340922daab9">

```bash
* Check the datatypes
campaign_df.dtypes
```
<img width="140" alt="image" src="https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126307180/19068e43-6900-4167-a8bb-3ec995220777">

```bash
* Format the launched_date and end_date columns to datetime format
from datetime import datetime as dt
campaign_df['launch_date'] = pd.to_datetime(campaign_df['launch_date']).dt.strftime('%Y-%m-%d') 
campaign_df['end_date'] = pd.to_datetime(campaign_df['end_date']).dt.strftime('%Y-%m-%d')
campaign_df.head()
```
<img width="485" alt="image" src="https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126307180/ff57e6f8-24a9-4148-8c88-c36ba7aa9d45">

```bash
* Merge the campaign_df with the category_df on the "category" column and the subcategory_df on the "subcategory" column.
campaign_merged_df = campaign_df.merge(category_df, on='category', how='left').merge(subcategory_df, on='subcategory', how='left')
campaign_merged_df.tail(10)
```
<img width="484" alt="image" src="https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126307180/35bd2a25-6f4b-4e6b-8596-9c65cb101d1b">

```bash
* Drop unwanted columns
campaign_cleaned = campaign_merged_df.drop(['staff_pick', 'spotlight', 'category & sub-category','category', 'subcategory'], axis=1)
campaign_cleaned.head()
```
<img width="484" alt="image" src="https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126307180/6ed2ecd9-417b-452b-a4b4-5fe3c2ecf896">

```bash
* Export the DataFrame as a CSV file. 
campaign_cleaned.to_csv('Resources/campaign.csv', index=False)
```
### Create the Contacts DataFrame


### Create the Crowdfunding Database
```
CREATE TABLE "campaign" (
    "cf_id" int   NOT NULL,
    "contact_id" int   NOT NULL,
    "company_name" varchar(100)   NOT NULL,
    "description" text   NOT NULL,
    "goal" numeric(10,2)   NOT NULL,
    "pledged" numeric(10,2)   NOT NULL,
    "outcome" varchar(50)   NOT NULL,
    "backers_count" int   NOT NULL,
    "country" varchar(10)   NOT NULL,
    "currency" varchar(10)   NOT NULL,
    "launch_date" date   NOT NULL,
    "end_date" date   NOT NULL,
    "category_id" varchar(10)   NOT NULL,
    "subcategory_id" varchar(10)   NOT NULL,
    CONSTRAINT "pk_campaign" PRIMARY KEY (
        "cf_id"
     )
);

CREATE TABLE "category" (
    "category_id" varchar(10)   NOT NULL,
    "category_name" varchar(50)   NOT NULL,
    CONSTRAINT "pk_category" PRIMARY KEY (
        "category_id"
     )
);

CREATE TABLE "subcategory" (
    "subcategory_id" varchar(10)   NOT NULL,
    "subcategory_name" varchar(50)   NOT NULL,
    CONSTRAINT "pk_subcategory" PRIMARY KEY (
        "subcategory_id"
     )
);

CREATE TABLE "contacts" (
    "contact_id" int   NOT NULL,
    "first_name" varchar(50)   NOT NULL,
    "last_name" varchar(50)   NOT NULL,
    "email" varchar(100)   NOT NULL,
    CONSTRAINT "pk_contacts" PRIMARY KEY (
        "contact_id"
     )
);


ALTER TABLE "campaign" ADD CONSTRAINT "fk_campaign_contact_id" FOREIGN KEY("contact_id")
REFERENCES "contacts" ("contact_id");

ALTER TABLE "campaign" ADD CONSTRAINT "fk_campaign_category_id" FOREIGN KEY("category_id")
REFERENCES "category" ("category_id");

ALTER TABLE "campaign" ADD CONSTRAINT "fk_campaign_subcategory_id" FOREIGN KEY("subcategory_id")
REFERENCES "subcategory" ("subcategory_id");

```

![campaign_sql_table](https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126130038/2e257359-359b-465b-b046-09d2c8133200)




![category_sql_table](https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126130038/d02a175f-3057-4c14-8b18-c038699a446b)



![contacts_sql_table](https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126130038/28d706da-03d1-465a-8324-9b731642b724)



![subcategory_sql_table](https://github.com/RunningWomann/Project2_Crowdfunding_ETL/assets/126130038/9f80784a-324c-4380-b1b9-773d821a14d6)





