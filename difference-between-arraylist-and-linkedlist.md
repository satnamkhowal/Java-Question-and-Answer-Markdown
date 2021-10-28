# Difference between ArrayList and LinkedList
ArrayList and LinkedList both implements List interface and maintains insertion order. Both are non synchronized classes.

However, there are many differences between ArrayList and LinkedList classes that are given below.

<table class="alt">
<tbody><tr><th>ArrayList</th><th>LinkedList</th></tr>
<tr><td>1) ArrayList internally uses a <strong>dynamic array</strong> to store the elements.</td><td>LinkedList internally uses a <strong>doubly linked list</strong> to store the elements.</td></tr>
<tr><td>2) Manipulation with ArrayList is <strong>slow</strong> because it internally uses an array. If any element is removed from the array, all the bits are shifted in memory.</td><td>Manipulation with LinkedList is <strong>faster</strong> than ArrayList because it uses a doubly linked list, so no bit shifting is required in memory.</td></tr>
<tr><td>3) An ArrayList class can <strong>act as a list</strong> only because it implements List only.</td><td>LinkedList class can <strong>act as a list and queue</strong> both because it implements List and Deque interfaces.</td></tr>
<tr><td>4) ArrayList is <strong>better for storing and accessing</strong> data.</td><td>LinkedList is <strong>better for manipulating</strong> data.</td></tr>
</tbody></table>

## Example of ArrayList and LinkedList in Java
Let's see a simple example where we are using ArrayList and LinkedList both.

```Java
import java.util.*;    
class TestArrayLinked{    
 public static void main(String args[]){    
     
  List<String> al=new ArrayList<String>();//creating arraylist    
  al.add("Ravi");//adding object in arraylist    
  al.add("Vijay");    
  al.add("Ravi");    
  al.add("Ajay");    
    
  List<String> al2=new LinkedList<String>();//creating linkedlist    
  al2.add("James");//adding object in linkedlist    
  al2.add("Serena");    
  al2.add("Swati");    
  al2.add("Junaid");    
    
  System.out.println("arraylist: "+al);  
  System.out.println("linkedlist: "+al2);  
 }    
}    

```
