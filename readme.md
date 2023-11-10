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

//Implicit `$and:` When we want to filter data by just one field, with two condition.

```javaScript
db.test.find({
    gender: "Female",
    age: { $gte: 18, $lte: 35 }
}).project(
    { age: 1, gender: 1 }
).sort(
    { age: -1 } //Desending order. 
)
```

When we want to add same field twice, It'll show an `Error: 'Duplicate identifier "age"'.`  

To solve this problem we can use Explicit $end query method. 

With this method we  have to write a logical query first,
`$and` after in the` [third bracket]` we can have same field field multiple time.
using `$and` means every query must be satisfied to be filterd out.


```javaScript
db.test.find({
    $and: [
        { gender: "Female" },
        { age: { $gt: 18 } },
        { age: { $lt: 34 } }
    ]
}).project({
    age: 1,
    gender: 1
}).sort({ age: 1 })
```
When we have a requirement that if a query is satisfied we'll filter out that data. 
In this case, explicit `$or` would be a perfect query.

```javaScript
db.test.find({
    $or: [
        { "skills.name": "JAVASCRIPT" },
        { "skills.name": "PYTHON" }
    ]
}).project({
    skills: 1,
    "name.firstName": 1
}).sort({ "name.firstName": 1 });
```

But, in above same this code, we can write using `$in` query in less code.
instead of using logical` $or` query. 

```javaScript
db.test.find({
    "skills.name": {
        $in: ["JAVASCRIPT", "PYTHON"]
    }
}).project({
    skills: 1,
    name: 1
}).sort({
    skills: 1
})
```