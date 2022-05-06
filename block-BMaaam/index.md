writeCode

Insert the data present in users.json into local mongodb database using `mongoimport` into a database called sample and collection named as users.

Write aggregation queries to perform following tasks.

1. Find all users who are active.
Ans- db.users.aggregate([{ $match: { isActive: true } }])
2. Find all users whose name includes `blake` case insensitive.
Ans -
3. Find all males.
Ans- db.users.aggregate([{ $match: { gender: "male" } }])
4. Find all active males.
Ans - db.users.aggregate([{ $match: { $and: [{ gender: "male" }, { isActive: true }] } }])
5. Find all active females whose age is >= 25.
Ans -db.users.aggregate([ {$match:{gender:"female",age:{$gte:25}}}])
6. Find all 40+ males with green eyecolor.
Ans - db.users.aggregate([ {$match:{gender:"male",age:{$gte:40},eyeColor:"blue"}}])
7. Find all blue eyed men working in 'USA'.
Ans - db.users.aggregate([ {$match: {eyeColor:'blue', 'company.location.country': 'USA'}}]).pretty()
8. Find all female working in Germany with green eyes and apple as favoriteFruit.
Ans - db.users.aggregate([{$match:{gender:'female','company.location.country' : 'Germany', favoriteFruit:'apple'}}]).pretty()
9. Count total male and females.
Ans -  db.users.aggregate([{$group:{_id :'$gender', count :{$sum :1}}}]).pretty()
10. Count all whose eyeColor is green.
Ans - db.users.aggregate([{$match :{eyeColor: "green"}},{$group:{_id :'$eyeColor', count :{$sum :1}}}]).pretty()
11. Count all 20+ females who have brown eyes.
Ans -  db.users.aggregate([{$match :{eyeColor: "brown",age:{$gt : 20}}},{$group:{_id :'$eyeColor', count :{$sum :1}}}]).pretty()
12. Count all occurences of all eyeColors.
    Something like:-

```
blue -> 30
brown -> 67
green -> 123
```
Ans -    db.users.aggregate([{$group :{ _id:'$eyeColor' , count:{$sum : 1}}}])

13. Count all females whose tags array include `amet` in it.
Ans  - db.users.aggregate([{$match : {gender : "female" , tags :"amet"}} ,{$group : {_id : '$gender' , count : {$sum : 1}}}])
14. Find the average age of entire collection
Ans -  db.users.aggregate([{$group : {_id :null , avg :{$avg : '$age'}}}])
15. Find the average age of males and females i.e. group them by gender.
Ans - db.users.aggregate([{$group :{_id : "$gender" , avg : {$avg : '$age'}}}]).pretty()
16. Find the user with maximum age.
Ans - db.users.aggregate([{$group : {_id : null , maxAge:{$max : '$age'} }}]).pretty()
17. Find the document with minimum age.
Ans - db.users.aggregate([{$group : {_id : null , minAge:{$min : '$age'} }}]).pretty()
18. Find the sum of ages of all males and females.
Ans - db.users.aggregate([{$group :{_id : "$gender" , ageSum : {$sum : "$age"}}}])
19. Group all males by their eyeColor.
Ans - db.users.aggregate([{$match : {gender : "male"}} ,{$group : {_id : "$eyeColor"}}])
20. group all 30+ females by their age.
Ans - db.users.aggregate([{$match :{gender : "female"}} ,{$group : {_id : "$age"}}]).pretty()
21. Group all 23+ males with blue eyes working in Germany.
Ans - db.users.aggregate([{$match : {gender: "male" , age : {$gt : 23} ,'company.location.country':"Germany", eyeColor : "blue"}} ,{$group : {_id : "$eyeColor" ,count : {$sum : 1}}}])
22. Group all by tag names i.e. use \$unwind since tags are array.
Ans - db.users.aggregate([{$unwind : '$tags'} , {$group :{_id : '$tags' , count:{$sum : 1}}}])
23. Group all males whose favoriteFruit is `banana` who have registered before 2015.
Ans - db.users.aggregate([ {$match: { gender:'male' favouriteFruit: 'banana' }}, {$group: {_id:'$favoriteFruit', count: {$sum:1}}} ]).pretty()
24. Group all females by their favoriteFruit.
Ans - db.users.aggregate([ {$match : {gender : "female"}},{$group : {_id : "$favoriteFruit"} , count :{$sum : 1}}])
25. Scan all the document to retrieve all eyeColors(use db.COLLECTION_NAME.distinct);
Ans - db.users.distinct('eyeColor')
26. Find all apple loving blue eyed female working in 'USA'. Sort them by their registration date in descending order.
Ans - db.users.aggregate([{ $match: { gender: 'female', favoriteFruit: 'apple', eyeColor: 'blue' 'company.location.country': 'USA' } },{ $sort: { registered: -1 } },]).pretty();
27. Find all 18+ inactive men and return only the fields specified below in the below provided format

```js
{
  name: "",
  email: '';
  identity: {
    eye: '',
    phone: '',
    location: ''
  }
}
```
Ans- db.users.aggregate([{$match: {gender: 'male',isActive: 'false', age:{$gt :18} }}, { $project: { name: 1, email: '$company:email', identity: {

    eye: '$eyeColor',
    phone: '$company.phone',
    location: '$company.location'
    } } }]).pretty()