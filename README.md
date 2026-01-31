A1.  Tablica dynamiczna

public class IntDynArray {
    static private int[] table;  
    private int nElems;  

    public IntDynArray(int maxSize) { 
        table = new int[maxSize]; 
        nElems = 0;             
    }

    //Add z relokacja
    public void add(int value) {    
        if (nElems >= table.length) {
            int[] locTable = new int[table.length * 2];
            for (int i = 0; i < table.length; i++)
                locTable[i] = table[i];
            // LUB: System.arraycopy(table, 0, locTable, 0, table.length);
            table = locTable;
        }

        table[nElems] = value;        
        nElems++;                      
    }

    public int get(int index) {
        return table[index];
    }

    public int size() { 
        return nElems;
    }

    public boolean remove(int index) {   
        if (nElems == 0 || index >= nElems || index < 0)
            return false;
        for (int j = index; j < nElems - 1; j++) {  
            table[j] = table[j + 1];
        }
        nElems--;                        
        return true;
    }

    public int find(int searchElem) {        
for (int j = 0; j < nElems; j++) {
            if (table[j] == searchElem) return j; 
        }
        return -1; // Elementu nie znaleziono
    }

    public void print() {
        for (int i = 0; i < nElems; i++) {
            System.out.print(get(i) + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        int maxSize = 2;

        IntDynArray array = new IntDynArray(maxSize);    
        array.add(11);
        array.add(33);
        array.add(22);
        array.add(44);

        array.print();

        array.remove(1); 
        array.print();
        array.remove(1);
        array.print();

        array.add(55);
        array.add(66);

        array.print();

        int elemIndex = array.find(55);
        System.out.println(elemIndex);

    }
}

A2.  Uporządkowana tablica dynamiczna

public class OrdIntDynArray {
    private int[] table; 
    private int nElems;  

    public OrdIntDynArray(int maxSize) { /
        table = new int[maxSize]; 
        nElems = 0;               
    }

    public void add(int value) {     
        if (nElems >= table.length) {
            int[] locTable = new int[table.length * 2];
            for (int i = 0; i < table.length; i++) locTable[i] = table[i];
            table = locTable;
        }

        int j;
        for (j = 0; j < nElems; j++)   
            if (table[j] > value) break;

        for (int k = nElems; k > j; k--)   
            table[k] = table[k - 1];

        table[j] = value;       
        nElems++;                     
    }

    public int get(int index) {
        return table[index];
    }

    public int size() { 
        return nElems;
    }

    public boolean remove(int index) {  
        if (nElems == 0 || index >= nElems || index < 0) return false;
        for (int j = index; j < nElems - 1; j++)   
        {
            table[j] = table[j + 1];
        }
        nElems--;                        
        return true;
    }

    public int find(int searchElem) { 
        int left = 0;           
        int right = nElems - 1; 
        int currIndex;  

        while (true) {
            currIndex = (left + right) / 2;
            if (table[currIndex] == searchElem) return currIndex; 
            else if (left > right) return -1;
            else {
                if (table[currIndex] < searchElem) {
                    left = currIndex + 1;
                } else {
                    right = currIndex - 1; 
                }
            }
        }
    }

    public void print() {
        for (int i = 0; i < nElems; i++) {
            System.out.print(get(i) + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        int maxSize = 2;

        OrdIntDynArray array = new OrdIntDynArray(maxSize);    

        array.add(21);
        array.add(4);
        array.add(23);
        array.add(14);

        array.print();

        int index = array.find(23);
        System.out.println(index);

    }
}

A3. Lista powiązana
ListElem:
public class ListElem {
    public int iData;
    public ListElem next;

    public ListElem(int iData) {
        this.iData = iData;
        next = null;
    }

    public String toString() {
        return Integer.toString(iData);
    }
}
LinkedList:
public class LinkedList {
    private ListElem first;  

    public LinkedList() {
        first = null;   
    }

    public boolean isEmpty() {   
        return (first == null);
    }

    public ListElem getFirst() {
        return first;
    }

    public void insertFirst(int value) {
        ListElem newElem = new ListElem(value);
        if (!isEmpty()) newElem.next = first;       
        first = newElem;            
    }

    public boolean find(int elem) {   
        if (isEmpty()) return false;

        ListElem current = first;  
        while (current.iData != elem) {
            if (current.next == null) return false;
            else current = current.next;
        }
        return true;
    }

    public ListElem deleteFirst() {  
        if (isEmpty()) {
            return null;
        }
        ListElem temp = first;
        first = first.next;
        return temp;
    }

    public ListElem delete(int elem) {  
        if (isEmpty()) return null;

        ListElem current = first;
        ListElem previous = null;

        while (current.iData != elem) {
            if (current.next == null) return null; 
            else {
                previous = current;     
                current = current.next;
            }
        }
        
        if (previous == null)  
            first = first.next; 
        else                 
            previous.next = current.next;   

        return current; 
    }

    public LinkedListIterator iterator() {
        return new LinkedListIterator(this);
    }


    public void print() {
        System.out.print("Lista (początek-->koniec): ");
        LinkedListIterator iterator = this.iterator();

        while (iterator.hasNext()) {  
            ListElem elem = iterator.next();
            System.out.print(elem.toString() + " ");
        }
        System.out.println();
    }

    public void print2() {
        System.out.print("Lista (początek-->koniec): ");
        ListElem current = first;   
        while (current != null) {    
            System.out.print(current.toString() + " ");
            current = current.next;  
        }
        System.out.println();
    }

    public static void main(String[] args) {
        LinkedList theList = new LinkedList();  

        theList.insertFirst(22);      
        theList.insertFirst(44);
        theList.insertFirst(66);
        theList.insertFirst(88);

        theList.print();             

        int liczba = 44;
        boolean wynik = theList.find(liczba);   
        if (wynik) System.out.println("Znaleziono element " + liczba);
        else System.out.println("Nie znaleziono elementu " + liczba);

        ListElem dElem = theList.delete(66);  
        if (dElem != null) System.out.println("Usunięto element o kluczu " + dElem.iData);
        else System.out.println("Nie można usunąć elementu");

        theList.print();   

        theList.print2();

        theList.deleteFirst();
        theList.print2();
    }
}


LinkedListIterator: 
package Lab7.A3;

public class LinkedListIterator {
    private ListElem currentElem;

    public LinkedListIterator(LinkedList linkedList) {
        currentElem = linkedList.getFirst();
    }

    public boolean hasNext() {
        if (currentElem == null) return false;
        return currentElem != null;
    }

    public ListElem next() {
        if (currentElem == null) return null;
        ListElem currElem = currentElem;
        currentElem = currentElem.next;
        return currElem;
    }
}

A4. Lista powiązana uporządkowana
ListElem:

public class ListElem {
    public int iData;            
    public ListElem next;         

    public ListElem(int iData) {
        this.iData = iData;   
        next = null; 
    }

    public String toString() {
        return Integer.toString(iData);
    }
}
SortedLinkedList:

import java.util.Random;

public class SortedLinkedList {
    private ListElem first;

    public SortedLinkedList() {
        first = null;
    }

    public boolean isEmpty() {
        return (first == null);
    }

    public void insertFirst(int value) {
        ListElem newElem = new ListElem(value);
        newElem.next = first;
        first = newElem;
    }

    public void insert(int value) {
        ListElem newElem = new ListElem(value);
        ListElem previous = null;
        ListElem current = first;

        while (current != null && newElem.iData > current.iData) {
            previous = current;
            current = current.next;
        }

        if (previous == null) first = newElem;
        else previous.next = newElem;
        newElem.next = current;
    }

    public ListElem find(int elem) {
        if (isEmpty()) return null;

        ListElem current = first;
        while (current.iData != elem) {
            if (current.next == null) return null;
            else
                current = current.next;
        }
        return current;
    }

    public ListElem deleteFirst() {
        if (isEmpty())
            return null;
        ListElem temp = first;
        first = first.next;
        return temp;
    }

    public ListElem delete(int elem) {
        if (isEmpty()) return null;

        ListElem current = first;

B1. Stos (ArrayList)
import java.util.ArrayList;

public class ArrayListStack {
    private ArrayList<Integer> intStack;

    public ArrayListStack() {
        intStack = new ArrayList<Integer>();
    }

    public void push(int elem) {
        intStack.add(elem);
    }

    public int pop() {
        int topElem = intStack.get(intStack.size() - 1);
        intStack.remove(intStack.size() - 1);
        return topElem;
    }

    public int peek() {
        return intStack.get(intStack.size() - 1);
    }

    public boolean isEmpty() {
        return intStack.isEmpty();
    }

    public static void main(String[] args) {
        ArrayListStack theStack = new ArrayListStack();
        theStack.push(20);
        theStack.push(40);
        theStack.push(60);
        theStack.push(80);
        theStack.push(81);

        while (!theStack.isEmpty()) {
            int value = theStack.pop();
            System.out.println(value + " ");
        }
        System.out.println("");
    }
}
B2. Stos (LinkedList)
package Lab9.B2;

import java.util.LinkedList;

public class LinkedListStack {
    private LinkedList<Integer> intStack;

    public LinkedListStack() {
        intStack = new LinkedList<Integer>();
    }

    public void push(int elem) {
        intStack.push(elem);
    }

    public int pop() {
        return intStack.pop();
    }

    public int peek() {
        return intStack.peek();
    }

    public boolean isEmpty() {
        return intStack.isEmpty();
    }

    public static void main(String[] args) {
        LinkedListStack theStack = new LinkedListStack();
        theStack.push(20);
        theStack.push(40);
        theStack.push(60);
        theStack.push(80);

        while (!theStack.isEmpty()) {
            long value = theStack.pop();
            System.out.print(value + "\n");
        }
        System.out.println("");
    }
}
B3. Kolejka (ArrayList)

import java.util.ArrayList;

public class ArrayListQueue {
    private ArrayList<Integer> intQueue;

    public ArrayListQueue() {
        intQueue = new ArrayList<Integer>();
    }

    public void insert(int elem) {
        intQueue.add(elem);
    }

    public int remove() {
        int firstElem = intQueue.get(0).intValue();
        intQueue.remove(0);
        return firstElem;
    }

    public int peek() {
        return intQueue.get(0).intValue();
    }

    public boolean isEmpty() {
        return intQueue.isEmpty();
    }

    public int size() {
        return intQueue.size();
    }

    public static void main(String[] args) {
        ArrayListQueue theQueue = new ArrayListQueue();

        System.out.println("Wstawiam: 10");
        theQueue.insert(10);
        System.out.println("Wstawiam: 20");
        theQueue.insert(20);
        System.out.println("Wstawiam: 30");
        theQueue.insert(30);
        System.out.println("Wstawiam: 40");
        theQueue.
B4. Kolejka (LinkedList)
import java.util.LinkedList;

public class LinkedListQueue {
    private LinkedList<Integer> intQueue;

    public LinkedListQueue() {
        intQueue = new LinkedList<Integer>();
    }

    public void insert(int elem) {
        intQueue.addLast(elem);
    }

    public int remove() {
        int firstElem = intQueue.get(0).intValue();
        intQueue.removeFirst();
        return firstElem;
    }

    public int peek() {
        return intQueue.peekFirst().intValue();
    }

    public boolean isEmpty() {
        return intQueue.isEmpty();
    }

    public int size() {
        return intQueue.size();
    }

    public static void main(String[] args) {
        LinkedListQueue theQueue = new LinkedListQueue();

        System.out.println("Wstawiam: 10");
        theQueue.insert(10);
        System.out.println("Wstawiam: 20");
        theQueue.insert(20);
        System.out.println("Wstawiam: 30");
        theQueue.insert(30);
        System.out.println("Wstawiam: 40");
        theQueue.insert(40);

        int elem = theQueue.remove();
        System.out.println(" Usuwam: " + elem);
        System.out.println(" Usuwam: " + theQueue.remove());
        System.out.println(" Usuwam: " + theQueue.remove());

        System.out.println("Wstawiam: 50");
        theQueue.insert(50);
        System.out.println("Wstawiam:
B5. Lista (ArrayList)
package Lab10.B5;

import java.util.ArrayList;

public class ArrayListList {

    private ArrayList<Object> oList;

    public ArrayListList() {          // konstruktor
        oList = new ArrayList<Object>();
    }

    public boolean addLast(Object elem) {  // dodawanie elementow
        return oList.add(elem);
    }

    public Object removeLast() { 
        int lastIndex = oList.size() - 1;
        return oList.remove(lastIndex);
    }

    public Object get(int index) { // pobieranie elementu o danym indeksie
        return oList.get(index);
    }

    public int find(Object elem) { // wyszukiwanie elementow
        return oList.indexOf(elem);
    }

    public int size() { // rozmiar listy
        return oList.size();
    }

    public void print() { 
        for (int i = 0; i < oList.size(); i++) {
            System.out.print(oList.get(i) + " ");
        }
        System.out.println("");
    }

    public static void main(String[] args) {
        ArrayListList theList = new ArrayListList();

        theList.addLast("elem0");
        theList.addLast("elem1");
        theList.addLast("elem2");
        theList.addLast("elem3");
        theList.addLast("elem4");

        theList.print();
        System.out.println("");

        System.out.println(theList.get(3));
        System.out.println("");

        theList.print();
        System.out.println("");

        System.out.println(theList.removeLast());
        System.out.println("");

        System.out.println(theList.find("elem4"));
        System.out.println("");

        theList.print();
        System.out.println("");
    }

}

B6. Lista (LinkedList)

import java.util.LinkedList;

public class LinkedListList {

    private LinkedList<Object> oList;

    public LinkedListList() {
        oList = new LinkedList<Object>();
    }

    public boolean isEmpty() {
        return oList.isEmpty();
    }

    public void insertFirst(Object elem) {
        oList.addFirst(elem);
    }

    public void insertLast(Object elem) {
        oList.addLast(elem);
    }

    public Object deleteFirst() {
        return oList.removeFirst();
    }

    public Object deleteLast() {
        return oList.removeLast();
    }

    public Object getFirst() {
        return oList.getFirst();
    }

    public Object getLast() {
        return oList.getLast();
    }

    public int size() {
        return oList.size();
    }

    public void print() {
        for (int i = 0; i < oList.size(); i++) {
            System.out.print(oList.get(i) + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        LinkedListList theList = new LinkedListList();

        theList.insertFirst(22);
        theList.insertFirst(44);
        theList.insertFirst(66);

        theList.insertLast(11);
        theList.insertLast(33);
        theList.insertLast(55);

        theList.print();

        theList.deleteFirst();
        theList.deleteFirst();

        theList.print();

        theList.deleteLast();

        theList.print();
    }
}
B7. Zbiór (ArrayList)
package Lab10.B7;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class ArrayListSet {
    private ArrayList<Integer> integerSet;

    public ArrayListSet() {
        integerSet = new ArrayList<Integer>();
    }

    public int size() {
        return integerSet.size();
    }

    public void insert(int elem) {
        if (!member(elem))
            integerSet.add(elem);
    }

    private int get(int index) {
        return integerSet.get(index).intValue();
    }

    public boolean member(int elem) {
        for (int i = 0; i < size(); i++)
            if (get(i) == elem)
                return true;
        return false;
    }

    public boolean delete(int elem) {
        int position = -1;
        for (int i = 0; i < size(); i++) {
            if (get(i) == elem) {
                position = i;
                break;
            }
        }
        if (position > -1) {
            integerSet.remove(position);
            return true;
        }
        return false;
    }

    public ArrayListSet union(ArrayListSet secondSet) {
        ArrayListSet unionSet = new ArrayListSet();

        for (int i = 0; i < size(); i++) {
            int locElem = get(i);
            unionSet.insert(locElem);
        }

        for (int i = 0; i < secondSet.size(); i++) {
            int locElem = secondSet.get(i);
            unionSet.insert(locElem);
        }
        return unionSet;
    }

    public ArrayListSet intersection(ArrayListSet secondSet) {
        ArrayListSet intersectionSet = new ArrayListSet();

        for (int i = 0; i < size(); i++) {
            int locElem = get(i);
            if (secondSet.member(locElem))
                intersectionSet.insert(locElem);
        }
        return intersectionSet;
    }

    public ArrayListSet difference(ArrayListSet secondSet) {
        ArrayListSet differenceSet = new ArrayListSet();

        for (int i = 0; i < size(); i++) {
            int locElem = get(i);
            if (!secondSet.member(locElem))
                differenceSet.insert(locElem);
        }
        return differenceSet;
    }

    public void print() {
        for (int i = 0; i < size(); i++) {
            int locElem = get(i);
            System.out.print(locElem + " ");
        }
        System.out.println();
    }

    public void saveToFile(String fileName) throws IOException {
        FileOutputStream fos = new FileOutputStream(fileName);
        PrintWriter pw = new PrintWriter(fos);

        pw.println(size());

        for (int i = 0; i < integerSet.size(); i++)
            pw.println(get(i));

        pw.close();
    }

    private int getIntFromFile(BufferedReader br) throws IOException {
        String line = br.readLine();
        StringTokenizer st = new StringTokenizer(line);
        String stringVal = st.nextToken();
        int intVal = 0;
        try {
            intVal = Integer.parseInt(stringVal);
        } catch (NumberFormatException e) {
            throw new IOException("Format error with integer value! " + e.getMessage());
        }
        return intVal;
    }

    public void loadFromFile(String fileName) throws IOException {
        File inFile = new File(fileName);

        if (!inFile.exists()) {
            throw new IOException("Cannot open file with set: " + fileName);
        }

        integerSet.clear();

        FileReader fr = new FileReader(inFile);
        BufferedReader br = new BufferedReader(fr);

        int s = getIntFromFile(br);

        for (int i = 0; i < s; i++) {
            int elem = getIntFromFile(br);
            integerSet.add(elem);
        }
        br.close();
    }

    public static void main(String[] args) {
        ArrayListSet theSetA = new ArrayListSet();
        theSetA.insert(60);
        theSetA.insert(20);
        theSetA.insert(40);

        theSetA.print();

        boolean test20 = theSetA.member(20);
        System.out.println("Wynik testu w zbiorze A dla 20: " + test20);

        theSetA.print();

        boolean test30 = theSetA.member(30);
        System.out.println("Wynik w zbiorze A pierwszego testu dla 30: " + test30);

        theSetA.insert(30);

        theSetA.print();

        test30 = theSetA.member(30);
        System.out.println("Wynik w zbiorze A drugiego testu dla 30: " + test30);

        theSetA.delete(30);

        theSetA.print();

        test30 = theSetA.member(30);
        System.out.println("Wynik w zbiorze A trzeciego testu dla 30: " + test30);

        theSetA.insert(10);
        theSetA.insert(90);

        System.out.println("--------------------------------------------");

        System.out.println("Zbior A:");
        theSetA.print();

        ArrayListSet theSetB = new ArrayListSet();
        theSetB.insert(40);
        theSetB.insert(70);
        theSetB.insert(60);
        theSetB.insert(80);

        System.out.println("Zbior B:");
        theSetB.print();

        ArrayListSet unionSet = theSetA.union(theSetB);
        System.out.println("Suma A i B:");
        unionSet.print();

        ArrayListSet intersectionSet = theSetA.intersection(theSetB);
        System.out.println("Iloczyn A i B:");
        intersectionSet.print();

        ArrayListSet differenceSet = theSetA.difference(theSetB);
        System.out.println("Roznica A-B:");
        differenceSet.print();

        System.out.println("--------------------------------------------------");

        try {
            System.out.println("Zbior przed zapisem:");
            unionSet.print();
            unionSet.saveToFile("src/suma.txt");
            ArrayListSet theSetSUM = new ArrayListSet();
            theSetSUM.loadFromFile("src/suma.txt");
            System.out.println("Zbior po odczycie:");
            theSetSUM.print();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
Babelkowe: 
public class BubbleSort {
    public static void bubbleSortDescending(int[] array) {
        int n = array.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (array[j] < array[j + 1]) {
                    int temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                }
            }
        }
    }

    public static void main(String[] args) {
        int[] numbers = {5, 6, 3, 2, 8, -4, -9, 9, 0, 1};

        bubbleSortDescending(numbers);

        for (int num : numbers) {
            System.out.print(num + " ");
        }
    }
}
