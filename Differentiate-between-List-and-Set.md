# Difference Between List and Set in Java

Difficulty Level : Medium
Last Updated : 28 Jan, 2021

The List interface allows storing the ordered collection. It is a child interface of Collection. It is an ordered collection of objects in which duplicate values are allowed to store. List preserves the insertion order, it allows positional access and insertion of elements.

Declaration:

```
public abstract interface List extends Collection
```
The set interface in the java.util package and extends Collection interface is an unordered collection of objects in which duplicate values cannot be stored. It is an interface that implements the maths set. This interface contains the methods inherited from the Collection interface and adds a feature that restricts to insert the duplicate elements. 

Declaration: The Set interface is declared as:



public interface Set extends Collection
### Example:
```
Input :  Add Elements = [1, 2, 3, 1]
Output:  Set = [1, 2, 3]
     List = [1, 2, 3, 1]

Input :  Add Elements = [a, b, d, b]
Output:  Set = [a, b, d]
     List = [a, b, d, b]
 ```
Below is the illustration of Set and List :

``` java
// Implementation of List and Set in Java
import java.io.*;
import java.util.*;
  
class GFG {
    public static void main(String[] args)
    {
        // List declaration
        List<Integer> l = new ArrayList<>();
        l.add(5);
        l.add(6);
        l.add(3);
        l.add(5);
        l.add(4);
  
        // Set declaration
        Set<Integer> s = new HashSet<>();
        s.add(5);
        s.add(6);
        s.add(3);
        s.add(5);
        s.add(4);
  
        // printing list
        System.out.println("List = " + l);
        // printing Set
        System.out.println("Set = " + s);
    }
}
```
```
Output
List = [5, 6, 3, 5, 4]
Set = [3, 4, 5, 6]
```

## Difference between List and Set:

## List	
1. The List is an ordered sequence.	
2. List allows duplicate elements	
3. Elements by their position can be accessed.	
4. Multiple null elements can be stored.	
5. List implementations are ArrayList, LinkedList, Vector, Stack

## Set
1. The Set is an unordered sequence.
2. Set doesn’t allow duplicate elements.
3. Position access to elements is not allowed.
4. Null element can store only once
5. Set implementations are HashSet, LinkedHashSet.