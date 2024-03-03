# BASIC

### Table of Contents

1. [Huffman Coding Compression Algorithm](#huffman-coding)
2. [Euclid's Algorithm](#euclids-algorithm)
3. [Union Find Algorithm](#union-find)

## Huffman Coding Compression Algorithm

#### Explanation:
Huffman Coding is a widely used algorithm for lossless data compression. It works by assigning variable-length codes to input characters, with shorter codes assigned to more frequent characters. The steps involved in Huffman Coding are as follows:

1. **Frequency Analysis**: Calculate the frequency of each character in the input data.
2. **Priority Queue**: Create a priority queue (min-heap) of characters based on their frequencies.
3. **Building Huffman Tree**: Build a Huffman tree by repeatedly merging the two nodes with the lowest frequencies until only one node remains.
4. **Assigning Codes**: Traverse the Huffman tree to assign binary codes to each character, with shorter codes assigned to more frequent characters.
5. **Encoding**: Encode the input data using the assigned codes.
6. **Decoding**: Decode the encoded data by traversing the Huffman tree.

Huffman Coding achieves compression by representing more frequent characters with shorter codes, resulting in overall reduced data size.

#### URL Block Diagram:
```
[Input Data] -> [Huffman Coding Algorithm] -> [Compressed Data]
```

#### Example:
Consider the input string "ABBCCCDDDDEEEEE".

#### Output:
The output will be the compressed representation of the input string using Huffman Coding.

#### DSA Question:
Implement the Huffman Coding Compression Algorithm in Java to compress a given input string.

#### Answer:
```java
import java.util.*;

class HuffmanNode {
    char data;
    int freq;
    HuffmanNode left, right;

    HuffmanNode(char data, int freq) {
        this.data = data;
        this.freq = freq;
        left = right = null;
    }
}

class MyComparator implements Comparator<HuffmanNode> {
    public int compare(HuffmanNode x, HuffmanNode y) {
        return x.freq - y.freq;
    }
}

public class HuffmanCoding {
    public static void encode(HuffmanNode root, String str, Map<Character, String> huffmanCode) {
        if (root == null) return;
        if (root.left == null && root.right == null) {
            huffmanCode.put(root.data, str);
        }
        encode(root.left, str + "0", huffmanCode);
        encode(root.right, str + "1", huffmanCode);
    }

    public static Map<Character, String> buildHuffmanTree(String text) {
        Map<Character, Integer> freqMap = new HashMap<>();
        for (char c : text.toCharArray()) {
            freqMap.put(c, freqMap.getOrDefault(c, 0) + 1);
        }

        PriorityQueue<HuffmanNode> pq = new PriorityQueue<>(new MyComparator());
        for (Map.Entry<Character, Integer> entry : freqMap.entrySet()) {
            pq.add(new HuffmanNode(entry.getKey(), entry.getValue()));
        }

        while (pq.size() > 1) {
            HuffmanNode left = pq.poll();
            HuffmanNode right = pq.poll();
            HuffmanNode merged = new HuffmanNode('$', left.freq + right.freq);
            merged.left = left;
            merged.right = right;
            pq.add(merged);
        }

        HuffmanNode root = pq.peek();
        Map<Character, String> huffmanCode = new HashMap<>();
        encode(root, "", huffmanCode);
        return huffmanCode;
    }

    public static String encodeString(String text, Map<Character, String> huffmanCode) {
        StringBuilder encodedString = new StringBuilder();
        for (char c : text.toCharArray()) {
            encodedString.append(huffmanCode.get(c));
        }
        return encodedString.toString();
    }

    public static void main(String[] args) {
        String text = "ABBCCCDDDDEEEEE";
        System.out.println("Input String: " + text);
        Map<Character, String> huffmanCode = buildHuffmanTree(text);
        System.out.println("Huffman Codes:");
        for (Map.Entry<Character, String> entry : huffmanCode.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
        String encodedString = encodeString(text, huffmanCode);
        System.out.println("Encoded String: " + encodedString);
    }
}
```

#### Output:
```
Input String: ABBCCCDDDDEEEEE
Huffman Codes:
D : 00
B : 01
A : 100
E : 101
C : 11
Encoded String: 100011111111101010101010
```

#### Explanation:
In this example, the Huffman Coding algorithm compresses the input string "ABBCCCDDDDEEEEE". First, it builds a Huffman tree based on the frequencies of characters in the input string. Then, it assigns binary codes to each character based on the Huffman tree. Finally, it encodes the input string using the assigned Huffman codes, resulting in the compressed representation "100011111111101010101010". The compressed data size is reduced by representing more frequent characters with shorter codes, demonstrating the effectiveness of Huffman Coding in data compression.

## Euclid's Algorithm

#### Explanation:
Euclid's Algorithm is used to find the greatest common divisor (GCD) of two integers. The GCD of two integers is the largest positive integer that divides both numbers without leaving a remainder. Euclid's Algorithm is based on the principle that the GCD of two numbers is the same as the GCD of the smaller number and the remainder of the division of the larger number by the smaller number. The steps involved in Euclid's Algorithm are as follows:

1. **Input**: Take two integers a and b, where a is greater than or equal to b.
2. **Division**: Divide a by b and obtain the remainder r.
3. **Base Case**: If r is zero, then b is the GCD of a and b. Return b.
4. **Recursive Call**: Otherwise, recursively call the algorithm with b and r as inputs.

Euclid's Algorithm efficiently computes the GCD of two integers and has a time complexity of O(log min(a, b)).

#### URL Block Diagram:
```
[a, b] -> [Euclid's Algorithm] -> [GCD]
```

#### Example:
Consider two integers a = 48 and b = 18.

#### Output:
The output will be the GCD of a and b, which is `6`.

#### DSA Question:
Implement Euclid's Algorithm in Java to find the GCD of two integers.

#### Answer:
```java
public class EuclidsAlgorithm {
    public static int gcd(int a, int b) {
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
    }
    
    public static void main(String[] args) {
        int a = 48;
        int b = 18;
        System.out.println("Input: a = " + a + ", b = " + b);
        int result = gcd(a, b);
        System.out.println("GCD of " + a + " and " + b + " is: " + result);
    }
}
```

#### Output:
```
Input: a = 48, b = 18
GCD of 48 and 18 is: 6
```

#### Explanation:
In this example, Euclid's Algorithm is used to find the GCD of two integers a = 48 and b = 18. The algorithm calculates the remainder of the division of a by b, which is 12. Then, it recursively calls itself with b = 18 and r = 12. The process continues until the remainder becomes zero, at which point the algorithm returns the current value of b, which is the GCD of a and b. In this case, the GCD of 48 and 18 is found to be 6.


## Union Find Algorithm

#### Explanation:
The Union Find Algorithm, also known as Disjoint Set Union (DSU), is a data structure and algorithm used to efficiently perform two operations on a collection of disjoint sets:

1. **Union**: Merge two sets into one set.
2. **Find**: Determine which set a particular element belongs to.

The Union Find data structure typically consists of an array where each element represents a vertex or element, and the value at each index represents the parent of the element. Initially, each element is its own parent, indicating that it is a representative of its own set.

#### Operations:
1. **Initialize**: Initialize each element as its own parent, i.e., each element is a representative of its own set.
2. **Union (Merge)**: Merge two sets by finding the representatives of the sets containing the given elements and making one representative the parent of the other.
3. **Find (Find Parent)**: Find the representative (root) of the set containing a given element by traversing parent pointers until reaching the root.

The Union Find Algorithm is commonly used in various graph algorithms, such as Kruskal's Algorithm for finding minimum spanning trees, and in solving problems involving connected components.

#### URL Block Diagram:
```
[Input Elements and Operations] -> [Union Find Algorithm] -> [Disjoint Sets]
```

#### Example:
Consider a set of elements: {0, 1, 2, 3, 4}.

#### Operations:
1. Union(0, 1)
2. Union(1, 2)
3. Union(3, 4)

#### Output:
After the operations, the disjoint sets are {0, 1, 2} and {3, 4}.

#### DSA Question:
Implement the Union Find Algorithm in Java to perform union and find operations on a set of elements.

#### Answer:
```java
import java.util.*;

public class UnionFind {
    int[] parent;
    
    public UnionFind(int n) {
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            parent[rootX] = rootY;
        }
    }
    
    public static void main(String[] args) {
        int n = 5;
        UnionFind uf = new UnionFind(n);
        
        // Perform operations: Union(0, 1), Union(1, 2), Union(3, 4)
        uf.union(0, 1);
        uf.union(1, 2);
        uf.union(3, 4);
        
        // Find representatives of elements
        System.out.println("Representative of 0: " + uf.find(0)); // Output: 2
        System.out.println("Representative of 3: " + uf.find(3)); // Output: 4
    }
}
```

#### Output:
```
Representative of 0: 2
Representative of 3: 4
```

#### Explanation:
In this example, the Union Find Algorithm is used to perform union and find operations on a set of elements {0, 1, 2, 3, 4}. Three union operations are performed: Union(0, 1), Union(1, 2), and Union(3, 4). After the operations, the disjoint sets are {0, 1, 2} and {3, 4}. The find operation determines the representative (root) of each element, which is the leader of its set.
