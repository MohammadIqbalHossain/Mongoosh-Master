### Documentaion of be a Mongoosh-Master. 

 db.test.findOne({ name: { firstName: "Candy" } })

// Fiedls filtering with {}. we want to watch only these filed of feild "malel" `{email: 1, name: 1, phone: 1}`

```javaScript
db.test.find({ gender: "Male"}, {email: 1, name: 1, phone: 1});
```
 
Fiedls filtering with project.

```javaScript
db.test.find({gender: "Female"}).project({name: 1, email: 1, phone: 1});
```
Operator `$eq`. find out if birthday field is equal to `"3/13/2022"` this value.

```javaScript
db.test.find({birthday : {$eq: "3/13/2022"}});
```

Not equal `$ne`. Find all age which are not equal to age 12. 
`$ne` will finter our all data which's age field value is not equal to 12 

```javaScript
db.test.find({ age: { $ne: 12 } });
```

Grater than `$gt`. Finds out all age fields value whcih are grater than 30.

```javaScript
db.test.find({ age: { $gt: 30 } });
```

greater than or equal to.` $gte`. Finds out all the age field value which are greater than or equal to 30

```js
 db.test.find({ age: { $gte: 30 } }).sort({ age: 1 });
```

// less than or equal to. `$lte`. Finds out all the age field value which are less than or equal to 30.

```js
 db.test.find({ age: { $lte: 30 } }).sort({ age: 1 });
```


 From the test database if age fiels data is getter than or equal to 18 and less than or equal to 30, and gender field should be female,  `{age: 1, gender: 1, name: 1}` this field is called field filtering by this we want to see age, and gender fields data only not everything.

 `.sort({ gender: 1 }` this code asends by gender means  `18, 19, 20... son on`

```javaScript
 db.test.find({age : {$gte: 18, $lte: 30}, gender: "Female"}, {age: 1, gender: 1, name: 1}).sort({ gender: 1 });
```

If an age matched from the array `$in` will filters out it.
`$nin` - not in will make sure 17, 20, 21, 35 these are not in the array.
`interests: {$in: ["Cooking", "Gaming"]}` interests field should have `cooking` or `gaming`.

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

But, above same this code, we can write using `$in` query in less code.
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

When <boolean> is true, 
`$exists`` matches the documents that contain the field, including documents where the field value is null.
If <boolean> is false, the query returns only the documents that do not contain the field.

With `$exsits` Element query operator we are checking if the field is exsits in the data or not,
It doesn't checks weather data is `null` or `undefined`. If it's there we can have the document.

```javaScript
db.test.find({ age: { $exists: false } }).project({ age: 1, name: 1 }).sort({ age: 1 })
```

The query `type` will match documents where the field value is given type. 
The types specified in the `type` element query can be either numeric or string aliases.

`type` will filter out weather any of the company field value is type null or given type.

```javaScript
db.test.find({company: {$type: "null"}})

```

when we want to filter out an array with an specified numbers of elements.
Suppose, we want to find out all array which are have 5 elements. 
In this case, we'll use `$size` array query operator.

```javaScript
db.test.find({ friends: { $size: 5 } }).project({ friends: 1 });
```


### `$all` array query operator.

Find if an array field value is matches.

```javaScript
db.test.find({ interests: "Writing" }).project({ interests: 1 })
```
Find out if two field value matches. 

```javaScript
db.test.find({ interests: ["Cooking", "Writing"] }).project({ interests: 1 });
```

Although, We know that "Cooking" and "Writing" value are there but we're having an empty array here.
because it's matches everything means index and value, mismatching in index and value will cause problem. 
here this will return some document.

```javaScript
db.test.find({ interests: ["Cooking", "Writing", "Reading"] }).project({ interests: 1 });//Index and vallues are matched. 
```

But our requirement is we've to find out if values in there. so, we'll use `$all` array query opoerator.

```javaScript
db.test.find({ interests: { $all: ["Cooking", "Writing"] } }).project({ interests: 1 }); //Now,it's working.
```

Find find if an array field value matches.

```js
db.test.find({ "skills.name": "JAVASCRIPT" }).project({ skills: 1 });
```


Find out if two array field value matches. 

```js
 db.test.find({ "skills.name": "JAVASCRIPT", "skills.level": "Intermidiate" }).project({ skills: 1 });
```
It's not working it's giving me all "level": "Expert" values also. 

My requirement is to find out if the level value is intermidate. let's try is another way.
```js
db.test.find({
    skills: {
        name: "JAVASCRIPT",
        level: "Intermidiate",
        // isLearning: false
    }
}).project({skills: 1})
```
 Returning an empty array. That means its's also checks if every field is matched and order of the field.
 when I add `isLearing: true` it's works. 



But our requirement is to find out if two field value or one field value matches,
No matter what is the order. 
To do so, we'll use `$elemMatch` match operator.

```js
db.test.find({
    skills: {
        $elemMatch: {
            name: "JAVASCRIPT",
            level: "Intermidiate"
        }
    }
}).project({ skills: 1 })
```
Yes, Now, it's giving me all documents where javascriptis intermidate value.


Update every this using `$set` operator. It'll will replace everything and adds new value.
It's used when updatinig primitive data type. When we need to replace a non-primitive data type we can use `$set` operator.

```javaScript
db.test.updateOne({ _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $set:
        {
            interests: "Gaming" //It'll change the whole array ["Gaming", "writing", "Cooking"] to "Gaming".
        }
    })
```

When our requiremnet is to not change but add something to it. we'll use `$addToSet`: operator. 
It won'take duplicate entry.

When we have add two value. we can use `$each` operator.
```javaScript
db.test.updateOne({ _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $addToSet: {
            interests: { $each: ["Travelling", "Driving"] }
       }
    })
```

Adding duplicate value. we'll use `$push`

```javaScript
db.test.updateOne({ _id: ObjectId("6406ad63fc13ae5a40000069") },
    {
        $push: {
            interests: { $each: ["Reading", "Writing", "Cooking"] }
        }
    })
```

To remove something from the data we'll use `$unset` operator. 
It'll remove the whole field. 

```javaScript
db.test.updateOne({ _id: ObjectId("6406ad63fc13ae5a40000069") },
    { $unset: { age: 80 } })

```

The $pop operator removes the first or last element of an array. Pass $pop
a value of - 1 to remove the first element of an array and 1 to remove the last element in an array.

When removing a value from an array, we can use `$pop` method, using `$pop`:

```javaScript
db.test.updateOne({ _id: ObjectId("6406ad63fc13ae5a40000069") }, {
    $pop: { friends: -1 }
});
```


The $pull operator removes from an existing array all instances of a value or values that match a specified condition.

Remove one or multiple values from an array.

Remove one.
```javaScript
db.test.updateOne({ _id: ObjectId("6406ad63fc13ae5a40000069") }, {
    $pull: { friends: "Mir Hussain" }
})
```

Remove multiple.
```javaScript
db.test.updateOne({_id: ObjectId("6406ad63fc13ae5a40000069")}, {
    $pullAll: {friends:[ "Abdur Rakib", "Najmus Sakib" ]}
})

```

More from `$set` method.

Set or update an objects data by `$set` method.

```javaScript
db.test.updateOne({ _id: ObjectId("6406ad63fc13ae5a40000069") }, {
    $set: {
        "address.city": "dhaka",
        "address.country": "Bangladesh",
        "address.postalCode": "0343"
    }
})
```


Set or update array of object data using `$set` method.

```javaScript
db.test.updateOne({ _id: ObjectId("6406ad63fc13ae5a40000069"), "education.degree": "Master of Science" },
    { $set: { "education.$.major": "Physics" } }
)
```

Set or update multiple data from an array of objects.

```js
db.test.updateOne({ _id: ObjectId("6406ad63fc13ae5a40000069"), "education.degree": "Master of Science" },
    { $set: { "education.$.major": "Physics", "education.year": 2005 } }
)
```