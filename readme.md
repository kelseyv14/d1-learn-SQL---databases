//1 How many users are there?
sqlite> SELECT COUNT(*) FROM users;
COUNT(*)
50

//2 What are the 5 most expensive items?
sqlite> select * from items order by price;
60|Ergonomic Steel Car|Books & Outdoors|Enterprise-wide secondary firmware|9341
40|Sleek Wooden Hat|Music & Baby|Quality-focused heuristic info-mediaries|9390
100|Awesome Granite Pants|Toys & Books|Upgradable 24/7 access|9790
83|Small Wooden Computer|Health|Re-engineered fault-tolerant adapter|9859
25|Small Cotton Gloves|Automotive, Shoes & Beauty|Multi-layered modular service-desk|9984

//3 What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?
SELECT * FROM items where category like '%book%' ORDER BY Price;
76|Ergonomic Granite Chair|Books|De-engineered bi-directional portal|1496
vs.
SELECT * FROM items WHERE category LIKE '%book%';
95|Small Cotton Hat|Beauty & Books|Reduced regional instruction set|1727

//4 Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?

sqlite> select * from addresses where street like '%Zetta Hills%';
id|user_id|street|city|state|zip
43|40|6439 Zetta Hills|Willmouth|WY|15029
user_id: 40 
sqlite> select * from users where id=40;
id|first_name|last_name|email
answer: 40|Corrine|Little|rubie_kovacek@grimes.net

sqlite> select * from addresses where user_id=40;
id|user_id|street|city|state|zip
43|40|6439 Zetta Hills|Willmouth|WY|15029
44|40|54369 Wolff Forges|Lake Bryon|CA|31587
answer: yes

//5 Correct Virginie Mitchell's address to "New York, NY, 10108".
sqlite> update addresses set city='New York', state='NY', zip='10108' where user_id=39;
sqlite> select * from addresses where user_id=39;
id|user_id|street|city|state|zip
41|39|12263 Jake Crossing|New York|NY|10108
42|39|83221 Mafalda Canyon|New York|NY|10108

//6 How much would it cost to buy one of each tool?
sqlite> SELECT Price from items where category like '%tools%';
price
7476
2851
1107
798
3335
615
7606
1913
2768
3160
5437
5210
839
2377
985
I could add all of those up and get the sum but I feel like there is a function that will do that, I just couldn't find it. May have been close with this: sqlite> select count (price) from items where category like '%tools%';
count (price)
15

edit: select sum(price) from items where category like '%tools%';
46477

//7 How many total items did we sell?
sqlite> select sum(quantity) from orders;
2125

//8 How much was spent on books?
sqlite> select items.category from items where category like '%Books%';
select sum((items.price * orders.quantity) / 100) as total from orders join items on items.id = orders.item_id where items.category like '%Books%';
10799

//9 Simulate buying an item by inserting a User for yourself and an Order for that User.
sqlite> insert into users (id, first_name, last_name, email) values (4444, 'kelsey', 'vaught','kelsey.vaught92@gmail.com');

sqlite> insert into orders (id, user_id, item_id, quantity, created_at) values ('378', '4444', '44', '4', '2017-03-06 09:32:44.307668');
