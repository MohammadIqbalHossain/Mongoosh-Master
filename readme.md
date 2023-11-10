# Documentaion of be a Mongoosh-Master. 

// db.test.findOne({ name: { firstName: "Candy" } })
// Fiels filtering with {}. 
// db.test.find({ gender: "Male"}, {email: 1, name: 1, phone: 1}); 

// Fiedls filtering with project.
// db.test.find({gender: "Female"}).project({name: 1, email: 1, phone: 1});

// Operator $eq Field is equal to a value.

// db.test.find({birthday : {$eq: "3/13/2022"}});

// Not equal $ne 
// db.test.find({ age: { $ne: 12 } });

//Getter than $gt. 
// db.test.find({ age: { $gt: 30 } })

// getter than or equal to. $gte 

// db.test.find({ age: { $gte: 30 } }).sort({ age: 1 });

// less than or equal to. $gte 


 From the test database if age fiels data is getter than or equal to 18 and less than or equal to 30, and gender field should be female,  `{age: 1, gender: 1, name: 1}` this field is called field filtering by this we want to see age, and gender fields data only not everything.

 `.sort({ gender: 1 }` this code desents by gender means  `18, 19, 20... son on`

```javaScript
 db.test.find({age : {$gte: 18, $lte: 30}, gender: "Female"}, {age: 1, gender: 1, name: 1}).sort({ gender: 1 });
```

//If an age matched from the array `$in` will filters out it.
//`$nin` - not in will make sure 17, 20, 21, 35 these are not in the array.
//`interests: {$in: ["Cooking", "Gaming"]}` interests field should have `cooking` or `gaming`.

```javaScript
db.test.find({
    gender: "Female",
    age: { $in: [23, 34, 19, 24, 22], $nin: [17, 20, 21, 35] },
    interests: {$in: ["Cooking", "Gaming"]} 
},
{ age: 1, gender: 1, name: 1, interests: 1 }).
sort({ gender: 1 });
```
