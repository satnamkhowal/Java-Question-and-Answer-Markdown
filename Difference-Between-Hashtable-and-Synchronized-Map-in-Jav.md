# Difference Between Hashtable and Synchronized Map in Java
Last Updated : 22 Feb, 2021
The Hashtable class implements a hash table, which maps keys to values. Any non-null object can be used as a key or as a value. To successfully store and retrieve objects from a Hashtable, the objects used as keys must implement the hashCode method and also the equals method.  

## Features of Hashtable:

Attention reader! Don’t stop learning now. Get hold of all the important Java Foundation and Collections concepts with the Fundamentals of Java and Java Collections Course at a student-friendly price and become industry ready. To complete your preparation from learning a language to DS Algo and many more,  please refer Complete Interview Preparation Course.

1. It is kind of like HashMap but is synchronized.
2. Hashtable stores key/value pair in the hash table.
3. In Hashtable we specify an object that is used as a key, and the value we want to associate with that key. The key is then hashed, and the resulting hash code is used as the index at which the value is stored within the table.
4. The initial default capacity of Hashtable class is 11 whereas load Factor is 0.75.
5. HashMap doesn’t provide any Enumeration, while Hashtable provides not fail-fast Enumeration.
Example:

``` java
// Java program to demonstrate the
// usage of HashTable
  
import java.util.*;
class Hashtable1 {
    public static void main(String args[])
    {
        // Creating a HashTable
        Hashtable<Integer, String> mytable = new Hashtable<Integer, String>();
  
        // Adding elements to HashTable
        mytable.put(1, "James Bond");
        mytable.put(2, "Donald Trumph");
        mytable.put(3, "Joe Biden");
        mytable.put(4, "Mona Lisa");
  
        // Iterating through HashTable
        for (Map.Entry m : mytable.entrySet()) 
        {
            System.out.println(m.getKey() + " "
                               + m.getValue());
        }
    }
}
```

### Output
```
4 Mona Lisa
3 Joe Biden
2 Donald Trumph
1 James Bond
```
## SynchronizedHashMap

Synchronization at the Object level.
Every read/write operation needs to acquire lock.
Locking the whole collection is a performance overhead.
This essentially gives access to just one thread to the entire map & blocks all the other threads.
It may cause contention.
SynchronizedHashMap returns Iterator, which fails-fast on concurrent modification.
Example:

``` java
// Java program to demonstrate the
// usage of Synchronized HashMap
  
import java.util.Collections;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;
public class HashMapSyncExample {
    public static void main(String args[])
    {
        // Creating HashMap
        HashMap<Integer, String> hmap = new HashMap<Integer, String>();
        
        // Adding elements in HashMap
        hmap.put(2, "Anil");
        hmap.put(44, "Ajit");
        hmap.put(1, "Brad");
        hmap.put(4, "Sachin");
        hmap.put(88, "XYZ");
  
        Map map = Collections.synchronizedMap(hmap);
        Set set = map.entrySet();
        
        synchronized (map)
        {
            Iterator i = set.iterator();
            
            // Display elements
            while (i.hasNext())
            {
                Map.Entry me = (Map.Entry)i.next();
                System.out.print(me.getKey() + ": ");
                System.out.println(me.getValue());
            }
        }
    }
}
```

### Output
```
1: Brad
2: Anil
4: Sachin
88: XYZ
44: Ajit
```
## Hashtable vs SynchronizedHashMap
### Hashtable             
1. Hashtable doesn’t allow even a single null key and null values.
2. Iterators returned by Hashtable are fail-safe in nature. i.e they don’t throw ConcurrentModificationException if the map is modified after the creation of the iterator.
3. HashTable was there since JDK 1.1. From JDK 1.2, it has been made a part of Java Collection Framework.
4. HashTable is the legacy class. It is sometimes considered as due for deprecation. So, it is recommended that not to use HashTable in your applications.


### Synchronized HashMap                                         

1. Synchronized HashMap allows one null key and any number of null values.
2. Iterators returned by synchronized HashMap are fail-fast in nature. i.e they throw ConcurrentModificationException if the map is modified after the creation of iterator.
3. HashMap is introduced in JDK 1.2.
4. If you want a high level of data consistency, then only consider using synchronized HashMap.##