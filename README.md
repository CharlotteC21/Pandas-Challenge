# Heroes of Pymoli - Pandas-Challenge

Congratulations! After a lot of hard work in the data wrangling mines, you've landed a job as Lead Analyst for an independent gaming company. You've been assigned the task of analyzing the data for their most recent fantasy game Heroes of Pymoli.

Like many others in its genre, the game is free-to-play, but players are encouraged to purchase optional items that enhance their playing experience. As a first task, the company would like you to generate a report that breaks down the game's purchasing data into meaningful insights.

My final report includes the following:


<B>Three observable trends based on the data:</B>
# Observable Trends
 - The most popular, reported gender demographic for video game players is far and away the Male demographic, at 84.03%. (Which, based on experience - is fitting,    but we definitely need more female gamers!)
 - The vast majority, at 44.79% of video game players - are in the age bracket of 20-24. The second most popular age group demographic is 15-19. 
 - According to the "Average Player Purchase" purchasing data, <10 year olds spent ~$4.54 per player, which is more than the most popular age demographic of 20-24    year olds. (Basically, this means that the parents of 10 year olds are giving their kiddos lots of in game purchase money! haha)

# Player Count:
* Total Number of Players

```player = len(purchase_data["SN"].value_counts())
player_count = pd.DataFrame([player], columns = ["Total Players"])
player_count
```

# Purchasing Analysis (Total)
* Number of Unique Items
* Average Purchase Price
* Total Number of Purchases
* Total Revenue

```
# Purchasing Analysis (Total)
UniqueItems = len(purchase_data["Item Name"].unique())
Averageprice = purchase_data["Price"].mean()
NumPurchases = len(purchase_data["Item Name"])
Revenue = purchase_data["Price"].sum()


Pur_Analysis_Total = pd.DataFrame({"Number of Unique Items": [UniqueItems],
                                           "Average Price": [Averageprice],
                                           "Number of Purchases": [NumPurchases],
                                           "Total Revenue": [Revenue]})


Pur_Analysis_Total["Average Price"] = Pur_Analysis_Total["Average Price"].map("${:.2f}".format)
Pur_Analysis_Total["Total Revenue"] = Pur_Analysis_Total["Total Revenue"].map("${:.2f}".format)
Pur_Analysis_Total = Pur_Analysis_Total[["Number of Unique Items", "Average Price", "Number of Purchases", "Total Revenue"]]


Pur_Analysis_Total
```
# Gender Demographics
* Percentage and Count of Male Players
* Percentage and Count of Female Players
* Percentage and Count of Other / Non-Disclosed

```
# Gender Demographics

gender_groups = purchase_data.groupby("Gender")

total_count_gender = gender_groups.nunique()["SN"]

#percentages
percentage_gender = total_count_gender/player*100

gender_demographics = pd.DataFrame({
    "Percentage of Players":percentage_gender,
    "Total Count": total_count_gender
})

gender_demographics.drop_duplicates()

#float $
pd.options.display.float_format = '{:,.2f}%'.format
gender_demographics
```

# Purchasing Analysis (Gender)
* The below each broken by gender
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Gender

# Age Demographics
* The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.)
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Age Group

```
# Purchasing Analysis (age)
# Combined with the Purchasing Analysis from above!
bins = [0, 9.9, 14.9, 19.9, 24.9, 29.9, 34.9, 39.9, 150]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

purchase_data["Age Groups"]=pd.cut(purchase_data["Age"], bins, labels=group_names)
pd.cut(purchase_data["Age"], bins, labels=group_names).head()

age_groupings=purchase_data.groupby("Age Groups")

total_age_count=age_groupings["SN"].nunique()

percentage_by_age= total_age_count / player*100

age_demographics= pd.DataFrame({
    "Percentage of Players": percentage_by_age,
    "Total Count": total_age_count
})

#float $
pd.options.display.float_format = '{:,.2f}%'.format

age_demographics
```

# Purchasing Analysis (age)
* The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.)
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Age

```
#using the data frame from previous exercise and use that the create the variables for this set
purchase_count_age = age_groupings["Purchase ID"].count()

average_purchase_price = age_groupings["Price"].mean()

total_purchase_value = age_groupings["Price"].sum()

average_purch_per = total_purchase_value/total_age_count

age_analysis = pd.DataFrame({
    "Purchase Count": purchase_count_age,
    "Average Purchase Price": average_purchase_price,
    "Total Purchase Value":total_purchase_value,
    "Average Player Purchase": average_purch_per})

#float $
pd.options.display.float_format = '${:,.2f}'.format

age_analysis 
```

# Top Spenders
* Identified the the top 5 spenders in the game by total purchase value, then list (in a table):
  * SN
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value

```
# Top Spenders 
top_spender_stats = purchase_data.groupby("SN")

purchase_count_sn = top_spender_stats["Purchase ID"].count()

avg_spender_purch = top_spender_stats["Price"].mean()

total_spender_purchases = top_spender_stats["Price"].sum()

top_spenders = pd.DataFrame({"Purchase Count": purchase_count_sn,
                             "Average Purchase Price": avg_spender_purch,
                             "Total Purchase Value":total_spender_purchases})

ascending_purch_values = top_spenders.sort_values(["Purchase Count","Total Purchase Value"], ascending = False)

#float $
pd.options.display.float_format = '${:,.2f}'.format

ascending_purch_values.head(3)
```
# Most Popular Items
* Identified the 5 most popular items by purchase count, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value
```
#isolate the Item ID, Item Name, and Item Price columns
items = purchase_data[["Item ID", "Item Name", "Price"]]

item_id_names = items.groupby(["Item ID", "Item Name"])

purchase_count = item_id_names["Price"].count()

total_purchase_value = item_id_names["Price"].sum()

item_price = total_purchase_value/purchase_count

most_popular_items = pd.DataFrame({
    "Purchase Count": purchase_count, 
    "Item Price": item_price,
    "Total Purchase Value": total_purchase_value
})

#sort largest values
asc_most_popular_items = most_popular_items.sort_values(["Purchase Count"], ascending = False)

#float $
pd.options.display.float_format = '${:,.2f}'.format

asc_most_popular_items.head(3)
```
# Most Profitable Items
* Identified the 5 most profitable items by total purchase value, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value
```
# Most Profitable Items

most_profitable = most_popular_items.sort_values(["Total Purchase Value"],ascending = False).head(3)

#float $
pd.options.display.float_format = '${:,.2f}'.format

most_profitable
```
