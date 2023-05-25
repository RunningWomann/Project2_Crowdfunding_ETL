# Project2_Crowdfunding_ETL

![istockphoto-1361894912-612x612](https://github.com/cassiecontreras/Crowdfunding-ETL/assets/126130038/c0e74ca1-2f24-476c-9873-884599ef45a2)


### Team Members: Chun Zhao, Jing Xu, Jerrett Williams, Cassie Contreras

## Description

For the ETL mini project, you will work with a partner to practice building an ETL pipeline using Python, Pandas, and either Python dictionary methods or regular expressions to extract and transform the data. After you transform the data, you'll create four CSV files and use the CSV file data to create an ERD and a table schema. Finally, you’ll upload the CSV file data into a Postgres database.

## Resources
Project2 ETL Files

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

* Describe any process, steps, codes or graphs etc.
* Any modifications needed to be made to files/folders
* Step-by-step bullets
```
code blocks for commands
```

### Create the Contacts DataFrame

* Describe any process, steps, codes or graphs etc.
* Any modifications needed to be made to files/folders
* Step-by-step bullets
```
code blocks for commands
```
### Create the Crowdfunding Database

* Describe any process, steps, codes or graphs etc.
* Any modifications needed to be made to files/folders
* Step-by-step bullets
```
code blocks for commands
```

## Acknowledgments

Inspiration, code snippets, etc.
* [xxxxxx](https://github.com/matiassingers/awesome-readme)
