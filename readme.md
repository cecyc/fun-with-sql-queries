##Intro to SQL

Assignment for this week is to answer the following question about SQL, based on the provided file "store.sqlite3"

###Answers

####1) Numbers of users in the database.

I used the following code:

```
SELECT COUNT(*) FROM users;
```

There are **50 users** in the database.

####2) What are the 5 most expensive items?

I used the following code:

```
SELECT * FROM items ORDER BY price DESC;
```

Based on the output, I was able to see that the following items were the most expensive ones:

Item                   | Price
-----------------------|--------
Small Cotton Gloves    | 9984
Small Wooden Computer  | 9859
Awesome Granite Pants  | 9790
Sleek Wooden Hat       | 9390
Ergonimic Steel Car    | 9341


####3) What is the cheapest book?

I used the following code:

```
SELECT * FROM items WHERE category LIKE "book%";
```

This gives me a list of any categories that include "book," which includes "Books" and
"Books and Toys," etc. If I just say...

```
SELECT * FROM items WHERE category LIKE "book";
```
... I do not get anything because it is looking for exactly "book."

####4) Who lives at “6439 Zetta Hills, Willmouth, WY”? Do they have another address?

I used the following code to find the user ID in addresses

```
SELECT * FROM addresses WHERE street LIKE "6439 Zetta Hills";
```

The output was:

```
id          user_id     street            city        state       zip
----------  ----------  ----------------  ----------  ----------  ----------
43          40          6439 Zetta Hills  Willmouth   WY          15029
```

This told me the user I was looking for had the user_id of 40. Next, I had to look in the users table to find this user with the following code:

```
SELECT * FROM users WHERE id LIKE "40";
```

Which gave me the following output:

```
id          first_name  last_name
----------  ----------  ----------
40          Corrine     Little
```

Looking in addresses for her user_id yields that the user does, in fact, have a second address. This code...

```
SELECT * FROM addresses WHERE user_id LIKE "40";
```

...yields this output:

```
id          user_id     street            city        state       zip
----------  ----------  ----------------  ----------  ----------  ----------
43          40          6439 Zetta Hills  Willmouth   WY          15029
44          40          54369 Wolff Forg  Lake Bryon  CA          31587
```

####5) Correct Virginie Mitchell’s address to “New York, NY, 10108”.

First, I had to find the user's user_id:

```
SELECT * FROM users WHERE first_name LIKE "Virginie";
```

The output yielded that the user's ID was 39. I then cross-referenced the user ID in the addresses table:

```
SELECT * FROM addresses WHERE user_id LIKE "39";
```

I then found out there were two addresses associated with this user, so I updated the address that listed "NY" as the state. To update the user's city and zip, I then ran the following two queries:

```
UPDATE addresses SET city = "New York" WHERE id = "41";

UPDATE addresses SET zip = "10108" WHERE id = "41";
```

####6) How much would it cost to buy one of each tool?

To get this answer, I used the SUM method:

```
SELECT SUM(price) FROM items WHERE category LIKE "tools";
```

The answer I got after running that query is **7383**.

####7) How many items did we sell?
To get this answer, I used the SUM method once again, this time, calling it on quantity.

```
SELECT SUM(quantity) FROM orders;
```

The answer I got after running that query is **2125**.

####8) How much was spent on books?
This was by far the trickest of the queries, in which I had to use the JOIN method, while performing a multiplication and a sum. The code I used is as follows:

```
SELECT SUM(quantity * items.price) FROM orders JOIN items ON orders.item_id = items.id WHERE category LIKE "books";
```

This code only selects "Books" and does not select any category that is Books and something else (like "Books and Toys"). Based on this output, the answer is **420,566**.

However, if we are to sum all items that contain the keyword "Books" in its category, you can run the following query:

```
SELECT SUM(quantity * items.price) FROM orders JOIN items ON orders.item_id = items.id WHERE category LIKE "book%";
SUM(quantity * items.price)
```

Based on that query which contains the category "Books" and categories that include "books" but are not just books, we get the following answer: **503,761**.

####9) Simulate buying an item by inserting a User for yourself and an Order for that User.

In order to simulate buying an item, I first created a user with the following query:

```
INSERT INTO users VALUES (51, "Cecy", "Correa", "cecycorrea@gmail.com");
```
Once I created a new user, I added a new order for that user:

```
INSERT INTO orders VALUES (378, "51", "1", "1", CURRENT_TIMESTAMP);
```

That's it! We are now SQL wizards!


