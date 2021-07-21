## Manipulating arrays by use case

*Photo by  [Elisa Calvet B.](https://unsplash.com/@elisa_cb)*

## Intro

There are so many ways to create and manipulate arrays in JavaScript that it can be confusing at first. And even after years of practice, it can still be hard to remember which method to use, and when. This post aims to provide something like a "cheat sheet" with the most common array methods by use case, with concise examples. I truly believe that learning from examples is one of the best ways of learning, as long as you practice independently. Feel free to bookmark this post and come back anytime you need 😉

## 1. Populating

6 ways to create and populate this array: `[ 'fruit', 'fruit', 'fruit', 'fruit', 'fruit' ]`

```jsx
// 1. The obvious way 😇
const fruits = ['fruit', 'fruit', 'fruit', 'fruit', 'fruit']

// 2. The old fashion way 🥃
const fruits = new Array(5)
for (let i=0; i<test.length; i++) {
  fruits[i] = 'fruit'
}

// 3. Using Array.prototype.fill()
const fruits = new Array(5).fill('fruit')

// 4. Using Array.from()
const fruits = Array.from({length: 5}, () => 'fruit')

// 5. Using spreading
const fruits = [...new Array(5)].map(() => 'fruit')

// 6. Using concat
const fruits = ['fruit', 'fruit', 'fruit'].concat(['fruit', 'fruit'])
```

## 2. Transforming

Let's consider this array of objects:

```jsx
const fruits = [
  { name: "apple", quantity: 3 },
  { name: "orange", quantity: 5 },
  { name: "pear", quantity: 1 },
  { name: "banana", quantity: 0 },
]
```

We can build a new array **without mutating the original array** in various ways, depending on the needs:

```jsx
// 1. Transform with Array.prototype.map()
fruits.map(fruit => fruit.name)
// > [ 'apple', 'orange', 'pear', 'banana' ]

fruits.map((fruit, idx) => {
	// Map is particularly usefull to build strings for each element of an array
	let str = `#${idx + 1} ${fruit.name} (${fruit.quantity})`
	return str
})
// > ['#1 apple (3)', '#2 orange (5)', '#3 pear (1)', '#4 banana (3)']

 
// 2. Filter the array
fruits.filter(fruit => fruit.quantity === 0)
// > ['banana']
 
// 4. "Reduce" the array to a single value
fruits.reduce((total, fruit) => total + fruit.quantity, 0)
// > 9

// 5. Get a "slice" out of the array (without changing it)
fruits.slice(0, 2)
// > [ { name: 'apple', quantity: 3 }, { name: 'orange', quantity: 5 } ]

// The original array is still untouched
console.log(fruits)
// > [
//     { name: "apple", quantity: 3 },
//     { name: "orange", quantity: 5 },
//     { name: "pear", quantity: 1 },
//     { name: "banana", quantity: 0 },
//   ]
```

Or we can directly modify the original array:

```jsx
// 1. Remove the last item (and return it)
fruits.pop()
// > { name: 'banana', quantity: 3 }
console.log(fruits)
// > [
//     { name: 'apple', quantity: 3 },
//     { name: 'orange', quantity: 5 },
//     { name: 'pear', quantity: 1 }
//   ]

// 2. Add one or more item(s) at the end (return the new length)
fruits.push({name: 'kiwi', quantity: 2}, {name: 'strawberry', quantity: 14})
// > 5
console.log(fruits)
// > [
//     { name: 'apple', quantity: 3 },
//     { name: 'orange', quantity: 5 },
//     { name: 'pear', quantity: 1 },
//     { name: 'kiwi', quantity: 2 },
//     { name: 'strawberry', quantity: 14 }
//   ]

// 3. Remove the first item (and return it)
fruits.shift()
// > { name: 'apple', quantity: 3 },
console.log(fruits)
// > [
//     { name: 'orange', quantity: 5 },
//     { name: 'pear', quantity: 1 },
//     { name: 'kiwi', quantity: 2 },
//     { name: 'strawberry', quantity: 14 }
//   ]

// 4. Add an item at the begining (return the new length)
fruits.unshift({ name: 'cherry', quantity: 7 })
// > 5
console.log(fruits)
// > [
//     { name: 'cherry', quantity: 7 },
//     { name: 'orange', quantity: 5 },
//     { name: 'pear', quantity: 1 },
//     { name: 'kiwi', quantity: 2 },
//     { name: 'strawberry', quantity: 14 }
//   ]
```

The `Array.splice` method can also be used to change an array by removing and adding elements. It returns the removed elements (or an empty array if no elements were removed).

```jsx
// Array.prototype.splice(index[, deleteCount, element1, ..., elementN])

// 1. Remove elements 
let colors = ['green', 'yellow', 'blue', 'purple'];
colors.splice(0, 2)
// > [ 'green', 'yellow' ] 
console.log(colors)
// > [ 'blue', 'purple' ]

// ... without "deleteCount", all elements starting from "index" are removed
let colors = ['green', 'yellow', 'blue', 'purple'];
colors.splice(1)
// > [ 'yellow', 'blue', 'purple' ]
console.log(colors)
// > [ 'green' ]

// 2. Replace elements
let colors = ['green', 'yellow', 'blue', 'purple'];
colors.splice(2, 2, 'pink', 'orange')
// > [ 'blue', 'purple' ]
console.log(colors)
// > [ 'green', 'yellow', 'pink', 'orange' ]

// 3. Insert elements
let colors = ['green', 'yellow', 'blue', 'purple'];
colors.splice(2, 0, 'red', 'white')
// > []
console.log(colors)
// > [ 'green', 'yellow', 'red', 'white', 'blue', 'purple' ]

```

## 3. Making assertions

```jsx
// Let's get back to our fruits 🍊🍒🍌
const fruits = [
  { name: "apple", quantity: 3 },
  { name: "orange", quantity: 5 },
  { name: "pear", quantity: 1 },
  { name: "banana", quantity: 0 },
]

// 1. Array.prototype.some() => check if a least one fruit is out of stock
fruits.some(fruit => fruit.quantity === 0)
// > true

// 2. Array.prototype.every() => are every fruits out of stock ?
fruits.every(fruit => fruit.quantity === 0)
// > false

// 3. Array.prototype.includes()
const shoppingList = ['bread', 'milk', 'cofee', 'sugar']
shoppingList.includes('bread')
// > true
shoppingList.includes('orange')
// > false
```

## 4. Ordering

```jsx
// Array.sort change the original array, as well a returning 
// its new value (the sorted array)
const numbers = [4, 1, 8, 3, 5]
numbers.sort((a, b) => b - a)
// > [ 8, 5, 4, 3, 1 ]
numbers.sort((a, b) => a - b)
// > [ 1, 3, 4, 5, 8 ]

// If you don't want to mutate the original array, you can spread it before:
const numbers = [4, 1, 8, 3, 5]
const sortedNumbers = [...numbers].sort((a, b) => a - b)
// sortedNumbers = [ 1, 3, 4, 5, 8 ]
// numbers = [4, 1, 8, 3, 5]
```

## 5. Searching

```jsx
const fruits = ['apple', 'orange', 'banana', 'apple', 'kiwi']

// Array.prototype.indexOf()
fruits.indexOf('apple')
// > 0
fruits.indexOf('kiwi')
// > 4
fruits.indexOf('pear')
// > -1

// Array.prototype.lastIndexOf()
fruits.lastIndexOf('apple')
// > 3

// Array.prototype.find()
const numbers = [1, 32, 12, 8, 4, 17]
numbers.find(number => number > 10)
// > [32, 12, 17
```

## 6. Arrays & Strings

```jsx
// 1. Array.join 
const words = ['Hello', 'wonderful', 'world']
words.join(' ')
// > 'Hello wonderful world'

// 2. Array.split
const sentence = "Hello wonderful world"
sentence.split(' ')
// > [ 'Hello', 'wonderful', 'world' ]
```

## 🎁 Bonus: tips & tricks

### Chaining

Many of the methods mentionned above return an array, which allow us to chain them like so:

```jsx
const fruits = [
  { name: "apple", quantity: 3 },
  { name: "orange", quantity: 5 },
  { name: "pear", quantity: 1 },
  { name: "banana", quantity: 0 },
]
fruits
	.sort((fruit1, fruit2) => fruit2.quantity - fruit1.quantity)
	.filter(fruit => fruit.quantity > 0)
	.map(fruit => `${fruit.name} (${fruit.quantity})`)
	.join(', ')
// > 'orange (5), apple (3), pear (1)'
```

### Spreading

There are various use cases when spreading an array is really useful:

```jsx
// 1. Make a copy of an array
const arrayCopy = [...originalArray]

// 2. Add elements in the array
const arrayWithNewElements = [newItem, ...originArray, anotherItem] 

// 3. Merge arrays
const mergedArrays = [...array1, ...array2, ...array3]

// 4. Convert a Set into an Array
const arrayFromSet = [...new Set(elements)]

// 5. Populate an empty array
const populatedArray = [...new Array(10)].map((_, idx) => idx)
// > [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ]

// 6. Convert an array to an object
const fruits = ['apple', 'orange', 'banana', 'kiwi']
console.log({...fruits})
// > { '0': 'apple', '1': 'orange', '2': 'banana', '3': 'kiwi' }
```

### Remove duplicates

3 ways to remove duplicates from an array:

```jsx
const array = ['🐑',1,2,'🐑','🐑',3,4];

// OPTION 1 
// Convert to a set and back to an array with the spread operator
const distinct_array = [...new Set(array)];

// OPTION 2 
// Use filter
const distinct_array = array.filter((item, idx) => array.indexOf(item) === idx);

// OPTION 3
// Use reduce
const distinct_array = array.reduce((unique, item) =>
  unique.includes(item) ? unique : [...unique, item]
)
```

### Empty an array

```jsx
const fruits = ['apple', 'orange', 'banana', 'kiwi']
fruits.length = 0
console.log(fruits)
// > []
```

### Remove falsy values

```jsx
const values = [false, 12, 'test', null, true]
values.filter(Boolean)
// > [ 12, 'test', true ]
```

---

That's all for today's breakfast folks. If you liked this post, feel free to share it with your friends/colleagues and leave your thoughts in the comments! 

Have a fantastic day,

With 🧡, Yohann