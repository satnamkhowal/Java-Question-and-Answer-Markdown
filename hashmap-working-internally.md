# Internal Working of HashMap in Java
<pre>
In this article, we will discuss the Internal Working of HashMap in Java and how hashmap’s get and put method works internally. As we know that HashMap is a key and value collection in java. The HashMap stores the data in key and value format. It provides the basic implementation of the Map interface of Java. We can put a value with using a key and also we can access this value using that key.
</pre>

<pre>
HashMap uses a technique called Hashing. Hashing is a technique to convert an object into an integer. The hashing is done by using the method hashCode(). This method is very important to maintain the performance of the HashMap.
</pre>

### Internal Structure of HashMap
<p>A HashMap is an array of the node and these nodes are like a linked list’s node as the following:</p>
<ul>
<li>int hash</li>
<li>K key</li>
<li>V value</li>
<li>Node next</li>
</ul>

### Hashing Implementation
<p>As we have discussed that Hashing is processed to convert a long string into a small string to represent the same string. In other words, the Hashing is a technique to convert an object into an integer number and this number is known as the hashcode of the object and you can generate this hashcode by using hashcode() method. Let’s see the following implementation of the hashcode() method for the class Key:</p>

```Java
    package com.gscs;

/**
 * @author Satnam Singh
 *
 */
public class Key {
	
	final int data = 112;
	private String key;
	
	public Key(String key) {
		super();
		this.key = key;
	}
	
	//index = hashCode(key) & (n-1).
	
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + data;
		result = prime * result + ((key == null) ? 0 : (key.charAt(0)+"").hashCode());
		System.out.println("hashCode for key: "+ key + " = " + result);
		System.out.println("Index "+ (result & 15));
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Key other = (Key) obj;
		if (data != other.data)
			return false;
		if (key == null) {
			if (other.key != null)
				return false;
		} else if (!key.equals(other.key))
			return false;
		return true;
	}
}

```
<p>
    In the above code, I am taking a class Key and override hashCode() method to show different scenarios. Here, overridden hashCode() method return a calculated hashcode. The hashCode() method is used to get the hashCode of an object. The hashCode() method of object class returns the memory reference of the object in integer form. But In HashMap, hashCode() is used to calculate the bucket and therefore calculate the index.
<br/>
We have also overridden equals() method in the above code, equals method is used to check that 2 objects are equal or not. HashMap uses equals() to compare the key whether they are equal or not.
</p>

### Buckets
<p>
    As we have said, HashMap stores elements in the array. Internally it uses array data structure. A bucket is an item of that array. This whole array considers as the buckets. This bucket is used to store nodes. It can store one or more than two nodes. Each node has a link to the next nodes if they store into the same bucket. In that case, a link list structure is used to connect the nodes.
</p>
<span>Let’s see the following relationship between bucket and capacity is as follows:</span>

```
    capacity = number of buckets * load factor
```


If the hash code of two items is same then both items will be stored into the same bucket. That means node storage in the bucket depends on the hashCode() method. The better your hashCode() method is, the better your buckets will be utilized.



## Index Calculation in Hashmap

HashCode is used to decide the bucket in the HashMap array, so using the HashCode we create an index. What happens, if Hashcode of the key may be very large or may be in the range of integer and if we create arrays for such a range. That may be a reason of the OutOfMemoryException. That is why it uses an index to minimize the size of an array. An index is created by using the following calculation.

```
    index = hashCode(key) & (n-1).
```
In the above formula, n is a number of buckets or the size of an array. In our example, I will consider n as default size that is 16.

Let’s see the how does HashMap work internally.

## Internal Working of HashMap in Java
**Step 1:** Create an empty HashMap as the following

```
    Map map = new HashMap();
```

The default size of HashMap is taken as 16 as the following empty array with size 16.

![bucket Img](./assets/img/buckets.png)

You can see the above image initially there is no element in the array.

**Step 2:** Inserting first element Key-Value Pair as the below:

```
    map.put(new Key("Dinesh"), "Dinesh");
```

This step will be executed as the following:

1. First, it will calculate the hash code of Key {“Dinesh”}. As we have implemented hashCode() method for the Key class, hash code will be generated as 4501.
2. Calculate index by using a generated hash code, according to the index calculation formula, it will be 5.
3. Now it creates a node object as the following:

```
{
  int hash = 4501
  Key key = {"Dinesh"}
  Integer value = "Dinesh"
  Node next = null
}
```
4. This node will be placed at index 5. As of now, we are supposing there is no node present at this index because it is a very first element.

Let’s see the following diagram of the HashMap:

![bucket insertions image](./assets/img/buckets-insert-1.png)

**Step 3**: Adding another element Key-Value Pair as the below:

```
    map.put(new Key("Anamika"), "Anamika");
```
This step will be executed as the following:
1. First, it will calculate the hash code of Key {“Anamika”}. As we have implemented hashCode() method for the Key class, hash code will be generated as 4498.
2. Calculate index by using a generated hash code, according to the index calculation formula, it will be 2.
3. Now it creates a node object as the following:

```
{
  int hash = 4498
  Key key = {"Anamika"}
  Integer value = "Anamika"
  Node next = null
}
```
4. This node will be placed at index 2. As of now, we are supposing there is no node present at this index because it is a very first element.
   
Let’s see the following diagram of the HashMap:
![buckets-insert-2.png](./assets/img/buckets-insert-2.png)

**Step 4:** (Case of Collision) Adding another element Key-Value Pair as the below:

```
    map.put(new Key("Arnav"), "Arnav");
```
This step will be executed as the following:
1. First, it will calculate the hash code of Key {“Arnav”}. As we have implemented hashCode() method for the Key class, hash code will be generated as 4498.
2. Calculate index by using a generated hash code, according to the index calculation formula, it will be 2.
3. Now it creates a node object as the following:

```
{
    int hash = 4498
    Key key = {"Arnav"}
    Integer value = "Arnav"
    Node next = null
}
```
4. This node will be placed at index 2 if no other object is presented there.
5. But at this index 2, one node is already presented, so this is the case of a collision.
6. Now, it will check hashCode() and equals() method if both keys are same then it will replace the old value with current value.
7. If both keys are not the same then it will connect this node to the previous node object via the linked list and both are stored at index 2.
   
Let’s see the following diagram of the HashMap:

![buckets-insert-3.png](./assets/img/buckets-insert-3.png)

**Step 5:** Adding another element Key-Value Pair as the below:

```
    map.put(new Key("Rushika"), "Rushika");
```

This step will be executed as the following:
1. First, it will calculate the hash code of Key {“Rushika”}. As we have implemented hashCode() method for the Key class, hash code will be generated as 4515.
2. Calculate index by using a generated hash code, according to the index calculation formula, it will be 3.
3. Now it creates a node object as the following:
```
{
    int hash = 4515
    Key key = {"Rushika"}
    Integer value = "Rushika"
    Node next = null
}
```
4. This node will be placed at index 3. As of now, we are supposing there is no node present at this index because it is a very first element.
   
Let’s see the following diagram of the HashMap:
![buckets-insert-4.png](./assets/img/buckets-insert-4.png)

As we have seen how does HashMap’s put() method work internally? Let’s move to the next section and see how does HashMap’s get() method work internally.

## HashMap’s get() method work internally

Now we will fetch a value from the HashMap using the get() method. As the following, we are fetching the data for key {“Arnav”}:

```
    map.get(new Key("Arnav"));
```
This step will be executed as the following:

1. First, it will calculate the hash code of Key {“Arnav”}. As we have implemented hashCode() method for the Key class, hash code will be generated as 4498.
2. Calculate index by using a generated hash code, according to the index calculation formula, it will be 2.
3. Go to index 2 of an array and compare the first element’s key with given key. If both are equals then return the value, otherwise, check for next element if it exists.
4. In our case, it is not found as the first element and next of node object is not null.
5. If next of node is null then return null.
6. If next of node is not null traverse to the second element and repeat the process 3 until a key is not found or next is not null.