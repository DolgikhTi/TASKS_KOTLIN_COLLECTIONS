Introduction:

This section was inspired by GS Collections Kata.
Kotlin can be easily mixed with Java code. Default collections in Kotlin are all Java collections under the hood. Learn about read-only and mutable views on Java collections.
The Kotlin standard library contains lots of extension functions that make working with collections more convenient. For example, operations that transform a collection into another one, starting with 'to': toSet or toList.
Implement the extension function Shop.getSetOfCustomers(). The class Shop and all related classes can be found in Shop.kt.
___________________________________________________________________________________________________________________________________________________________________
Sort:

Learn about collection ordering and the the difference between operations in-place on mutable collections and operations returning new collections.
Implement a function for returning the list of customers, sorted in descending order by the number of orders they have made. Use sortedDescending or sortedByDescending.

val strings = listOf("bbb", "a", "cc")

strings.sorted() ==
    listOf("a", "bbb", "cc")
    
strings.sortedBy { it.length } ==
    listOf("a", "cc", "bbb")
    
strings.sortedDescending() ==
    listOf("cc", "bbb", "a")
        
strings.sortedByDescending { it.length } ==
    listOf("bbb", "cc", "a")
___________________________________________________________________________________________________________________________________________________________________
Filter; map:

Learn about mapping and filtering a collection.
Implement the following extension functions using the map and filter functions:
Find all the different cities the customers are from
Find the customers living in a given city
val numbers = listOf(1, -1, 2)
numbers.filter { it > 0 } == listOf(1, 2)
numbers.map { it * it } == listOf(1, 1, 4)
___________________________________________________________________________________________________________________________________________________________________
All, Any, and other predicates:

Learn about testing predicates and retrieving elements by condition.
Implement the following functions using all, any, count, find:
checkAllCustomersAreFrom should return true if all customers are from a given city
hasCustomerFrom should check if there is at least one customer from a given city
countCustomersFrom should return the number of customers from a given city
findCustomerFrom should return a customer who lives in a given city, or null if there is none
val numbers = listOf(-1, 0, 2)
val isZero: (Int) -> Boolean = { it == 0 }
numbers.any(isZero) == true
numbers.all(isZero) == false
numbers.count(isZero) == 1
numbers.find { it > 0 } == 2
___________________________________________________________________________________________________________________________________________________________________
Associate:

Learn about association. Implement the following functions using associateBy, associateWith, and associate:
Build a map from the customer name to the customer
Build a map from the customer to their city
Build a map from the customer name to their city

val list = listOf("abc", "cdef")

list.associateBy { it.first() } == 
        mapOf('a' to "abc", 'c' to "cdef")
        
list.associateWith { it.length } == 
        mapOf("abc" to 3, "cdef" to 4)
        
list.associate { it.first() to it.length } == 
        mapOf('a' to 3, 'c' to 4)
___________________________________________________________________________________________________________________________________________________________________
Group By:

Learn about grouping. Use groupBy to implement the function to build a map that stores the customers living in a given city.

val result = 
    listOf("a", "b", "ba", "ccc", "ad")
        .groupBy { it.length }
result == mapOf(
    1 to listOf("a", "b"),
    2 to listOf("ba", "ad"),
    3 to listOf("ccc"))
___________________________________________________________________________________________________________________________________________________________________
Partition:

Learn about partitioning and the destructuring declaration syntax that is often used together with partition.
Then implement a function for returning customers who have more undelivered orders than delivered orders using partition.

val numbers = listOf(1, 3, -4, 2, -11)

val (positive, negative) =
    numbers.partition { it > 0 }
    
positive == listOf(1, 3, 2)
negative == listOf(-4, -11)
___________________________________________________________________________________________________________________________________________________________________
FlatMap:

Learn about flattening and implement two functions using flatMap:
The first should return all products the given customer has ordered
The second should return all products that at least one customer ordered

val result = listOf("abc", "12")
    .flatMap { it.toList() }
    
result == listOf('a', 'b', 'c', '1', '2')
___________________________________________________________________________________________________________________________________________________________________
Max min:

Learn about collection aggregate operations.
Implement two functions:
The first should return the customer who has placed the most amount of orders in this shop
The second should return the most expensive product that the given customer has ordered
The functions maxOrNull, minOrNull, maxByOrNull, and minByOrNull might be helpful.

listOf(1, 42, 4).maxOrNull() == 42

listOf("a", "ab").minByOrNull(String::length) == "a"

You can use callable references instead of lambdas. It can be especially helpful in call chains, where it occurs in different lambdas and has different types. Implement the getMostExpensiveProductBy function using callable references.
___________________________________________________________________________________________________________________________________________________________________
Sum:

Implement a function that calculates the total amount of money the customer has spent: the sum of the prices for all the products ordered by a given customer. Note that each product should be counted as many times as it was ordered.
Use sum on a collection of numbers or sumOf to convert the elements to numbers first and then sum them up.

listOf(1, 5, 3).sum() == 9

listOf("a", "b", "cc").sumOf { it.length } == 4
___________________________________________________________________________________________________________________________________________________________________
Fold:

Learn about fold and reduce and implement a function that returns the set of products that all the customers ordered using fold.
You can use the Customer.getOrderedProducts() defined in the previous task (copy its implementation).

listOf(1, 2, 3, 4)
    .fold(1) { partProduct, element ->
        element * partProduct
    } == 24
___________________________________________________________________________________________________________________________________________________________________
Compound tasks:

Implement two functions:
The first one should find the most expensive product among all the delivered products ordered by the customer. Use Order.isDelivered flag
The second one should count the number of times a product was ordered. Note that a customer may order the same product several times
Use the functions from the Kotlin standard library that were previously discussed.
You can use the Customer.getOrderedProducts() function defined in the previous task (copy its implementation).
___________________________________________________________________________________________________________________________________________________________________
Sequences:

Learn about sequences, they allow you to perform operations lazily rather than eagerly.
Copy the implementation from the previous task and modify it in a way that the operations on sequences are used.
___________________________________________________________________________________________________________________________________________________________________
Getting used to the new style:

We can rewrite and simplify the following code using lambdas and operations on collections. Fill in the gaps in doSomethingWithCollection, the simplified version of the doSomethingWithCollectionOldStyle function, so that its behavior stays the same and isn't modified in any way.

fun doSomethingWithCollectionOldStyle(
    collection: Collection<String>
): Collection<String>? {
    val groupsByLength = mutableMapOf<Int, MutableList<String>>()
    for (s in collection) {
        var strings: MutableList<String>? = groupsByLength[s.length]
        if (strings == null) {
            strings = mutableListOf()
            groupsByLength[s.length] = strings
        }
        strings.add(s)
    }
​
    var maximumSizeOfGroup = 0
    for (group in groupsByLength.values) {
        if (group.size > maximumSizeOfGroup) {
            maximumSizeOfGroup = group.size
        }
    }
​
    for (group in groupsByLength.values) {
        if (group.size == maximumSizeOfGroup) {
            return group
        }
    }
    return null
}
