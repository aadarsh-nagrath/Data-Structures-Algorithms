# ARRAY ALGORITHMS

# Table of Contents

- [Kadane's Algorithm](#kadanes-algorithm)
- [Floyd's Cycle Detection Algorithm](#floyds-cycle-detection-algorithm)
- [Knuth-Morris-Pratt (KMP) Algorithm](#knuth-morris-pratt-kmp-algorithm)
- [Quick Select Algorithm](#quick-select-algorithm)
- [Boyer-Moore Majority Vote Algorithm](#boyer-moore-majority-vote-algorithm)

## Kadane's Algorithm
[Back to Top](#table-of-contents)

#### Explanation:
Kadane's Algorithm is used to find the maximum subarray sum in an array. It efficiently handles cases with both positive and negative numbers. The algorithm iterates through the array, maintaining a running sum of consecutive elements. If the current element increases the sum, it continues to accumulate the sum. If the sum becomes negative at any point, it resets the sum to zero, effectively starting over for the subarray sum calculation. At each step, it compares the current sum with the maximum sum found so far and updates it if necessary.

#### URL Block Diagram:
```
[Array Input] -> [Kadane's Algorithm] -> [Maximum Subarray Sum]
```

#### Example:
Consider the array `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`.

#### Output:
For this array, the maximum subarray sum is `6`, which corresponds to the subarray `[4, -1, 2, 1]`.

#### DSA Question:
Given an array of integers, implement Kadane's Algorithm to find the maximum subarray sum.

#### Answer:
```java
public class KadanesAlgorithm {
    public static int maxSubArraySum(int[] nums) {
        int maxSum = Integer.MIN_VALUE;
        int currentSum = 0;
        
        for (int num : nums) {
            currentSum += num;
            if (currentSum > maxSum) {
                maxSum = currentSum;
            }
            if (currentSum < 0) {
                currentSum = 0;
            }
        }
        
        return maxSum;
    }
    
    public static void main(String[] args) {
        int[] arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
        System.out.println("Maximum Subarray Sum: " + maxSubArraySum(arr));
    }
}
```

#### Output:
```
Maximum Subarray Sum: 6
```

#### Explanation:
In this example, the array `[-2, 1, -3, 4, -1, 2, 1, -5, 4]` has a maximum subarray sum of `6`, which corresponds to the subarray `[4, -1, 2, 1]`. Kadane's Algorithm efficiently computes this maximum subarray sum by iterating through the array and maintaining the current sum. It resets the sum to zero when it encounters negative sums, ensuring that it always considers contiguous subarrays.

Let's move on to the next algorithm: Floyd's Cycle Detection Algorithm.

## Floyd's Cycle Detection Algorithm
[Back to Top](#table-of-contents)


#### Explanation:
Floyd's Cycle Detection Algorithm, also known as the Tortoise and Hare algorithm, is used to detect cycles in a linked list. It works by using two pointers, one moving at a slower pace (the tortoise) and another moving at a faster pace (the hare). At each iteration, the tortoise moves one step forward while the hare moves two steps forward. If there is a cycle in the linked list, eventually the hare will catch up to the tortoise. This indicates the presence of a cycle.

#### URL Block Diagram:
```
[Linked List] -> [Floyd's Cycle Detection Algorithm] -> [Cycle Detected or Not]
```

#### Example:
Consider the following linked list:

```
1 -> 2 -> 3 -> 4 -> 5 -> 2 (points back to 2)
```

#### Output:
The output will indicate that a cycle is present in the linked list.

#### DSA Question:
Write a Java function to detect cycles in a linked list using Floyd's Cycle Detection Algorithm.

#### Answer:
```java
class ListNode {
    int val;
    ListNode next;
    
    ListNode(int x) {
        val = x;
        next = null;
    }
}

public class CycleDetection {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        
        ListNode slow = head;
        ListNode fast = head.next;
        
        while (fast != null && fast.next != null) {
            if (slow == fast) {
                return true; // Cycle detected
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return false; // No cycle detected
    }
    
    public static void main(String[] args) {
        CycleDetection solution = new CycleDetection();
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        head.next.next.next = new ListNode(4);
        head.next.next.next.next = new ListNode(5);
        head.next.next.next.next.next = head.next; // Creating a cycle
        
        System.out.println("Cycle Detected: " + solution.hasCycle(head));
    }
}
```

#### Output:
```
Cycle Detected: true
```

#### Explanation:
In this example, the linked list has a cycle, as node `5` points back to node `2`. Floyd's Cycle Detection Algorithm efficiently detects this cycle by moving two pointers through the linked list, one at a faster pace than the other. If there's a cycle, the fast pointer eventually catches up to the slow pointer, indicating the presence of a cycle.
Next, let's discuss the Knuth-Morris-Pratt (KMP) Algorithm.

## Knuth-Morris-Pratt (KMP) Algorithm
[Back to Top](#table-of-contents)



#### Explanation:
The Knuth-Morris-Pratt (KMP) Algorithm is used for pattern searching in a given string. It efficiently finds occurrences of a pattern within a text by exploiting the information gathered during a preprocessing phase. The algorithm avoids unnecessary backtracking by utilizing a prefix function that helps determine potential matches. The key idea is to preprocess the pattern to create a partial match table, also known as the failure function or prefix function. This table helps the algorithm decide how far to shift the pattern when a mismatch occurs. By doing so, the algorithm improves the overall efficiency of the pattern searching process.

#### URL Block Diagram:
```
[Text, Pattern] -> [KMP Algorithm] -> [Occurrences of Pattern in Text]
```

#### Example:
Consider the text `"ABABDABACDABABCABAB"` and the pattern `"ABABCABAB"`.

#### Output:
The output will indicate the positions where the pattern `"ABABCABAB"` occurs in the text `"ABABDABACDABABCABAB"`. In this example, the pattern occurs at positions 10 and 15.

#### DSA Question:
Implement the Knuth-Morris-Pratt (KMP) Algorithm in Java to find occurrences of a pattern within a text.

#### Answer:
```java
public class KMPAlgorithm {
    public static int[] computeLPSArray(String pattern) {
        int[] lps = new int[pattern.length()];
        int len = 0; // Length of the previous longest prefix suffix
        
        int i = 1;
        while (i < pattern.length()) {
            if (pattern.charAt(i) == pattern.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }
        return lps;
    }
    
    public static void KMPSearch(String text, String pattern) {
        int[] lps = computeLPSArray(pattern);
        int i = 0; // Index for text
        int j = 0; // Index for pattern
        
        while (i < text.length()) {
            if (pattern.charAt(j) == text.charAt(i)) {
                i++;
                j++;
            }
            if (j == pattern.length()) {
                System.out.println("Pattern found at index " + (i - j));
                j = lps[j - 1];
            } else if (i < text.length() && pattern.charAt(j) != text.charAt(i)) {
                if (j != 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
        }
    }
    
    public static void main(String[] args) {
        String text = "ABABDABACDABABCABAB";
        String pattern = "ABABCABAB";
        System.out.println("Occurrences of Pattern in Text:");
        KMPSearch(text, pattern);
    }
}
```

#### Output:
```
Occurrences of Pattern in Text:
Pattern found at index 9
Pattern found at index 14
```

#### Explanation:
In this example, the pattern `"ABABCABAB"` occurs twice in the text `"ABABDABACDABABCABAB"`, at indices 9 and 14. The KMP Algorithm efficiently identifies these occurrences without unnecessary backtracking, thanks to the preprocessed prefix function. This function guides the algorithm to shift the pattern intelligently when a mismatch occurs, enhancing its overall performance.

Next, let's discuss the Quick Select algorithm.

## Quick Select Algorithm
[Back to Top](#table-of-contents)



#### Explanation:
Quick Select is a selection algorithm that efficiently finds the kth smallest (or largest) element in an unsorted array. It is a variation of the Quick Sort algorithm and operates by selecting a pivot element and partitioning the array into two subarrays. The key idea is to repeatedly partition the array until the pivot element is the kth smallest (or largest) element. Once the pivot element is found, the algorithm terminates, and the kth element is returned. Quick Select has an average-case time complexity of O(n) and is often used in algorithms that require finding the median or other order statistics.

#### URL Block Diagram:
```
[Unsorted Array, k] -> [Quick Select Algorithm] -> [kth Smallest/Largest Element]
```

#### Example:
Consider an array `[7, 10, 4, 3, 20, 15]` and k = 3.

#### Output:
The output will be the 3rd smallest element, which is `7`.

#### DSA Question:
Implement the Quick Select algorithm in Java to find the kth smallest element in an array.

#### Answer:
```java
public class QuickSelect {
    public static int quickSelect(int[] arr, int low, int high, int k) {
        if (k > 0 && k <= high - low + 1) {
            int pivotIndex = partition(arr, low, high);
            if (pivotIndex - low == k - 1) {
                return arr[pivotIndex];
            }
            if (pivotIndex - low > k - 1) {
                return quickSelect(arr, low, pivotIndex - 1, k);
            }
            return quickSelect(arr, pivotIndex + 1, high, k - (pivotIndex - low + 1));
        }
        return Integer.MAX_VALUE;
    }
    
    public static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;
        return i + 1;
    }
    
    public static void main(String[] args) {
        int[] arr = {7, 10, 4, 3, 20, 15};
        int k = 3;
        System.out.println("Array: " + Arrays.toString(arr));
        int kthSmallest = quickSelect(arr, 0, arr.length - 1, k);
        System.out.println("The " + k + "th smallest element is: " + kthSmallest);
    }
}
```

#### Output:
```
Array: [7, 10, 4, 3, 20, 15]
The 3th smallest element is: 7
```

#### Explanation:
In this example, the Quick Select algorithm finds the 3rd smallest element in the array `[7, 10, 4, 3, 20, 15]`. It partitions the array based on a pivot element and recursively searches for the kth smallest element in the appropriate subarray. Once the kth smallest element is found, it is returned as the output. Quick Select efficiently finds order statistics in an unsorted array and has an average-case time complexity of O(n).

Next, let's explore Boyer-Moore Majority Vote Algorithm.

## Boyer-Moore Majority Vote Algorithm
[Back to Top](#table-of-contents)



#### Explanation:
Boyer-Moore Majority Vote Algorithm is used to find the majority element in an array, i.e., the element that appears more than ⌊ n/2 ⌋ times, where n is the size of the array. It operates by canceling out pairs of elements that are different and keeping track of a potential majority candidate. The key idea is that if the majority element exists, it will still be the majority element after canceling out other elements. The algorithm consists of two phases:

1. **Voting Phase**: Iterate through the array and select a candidate element as the potential majority candidate. Increment its count for each occurrence and decrement the count for different elements.
2. **Verification Phase**: Check if the potential majority candidate is indeed the majority element by counting its occurrences in the array.

Boyer-Moore Majority Vote Algorithm has a time complexity of O(n) and is efficient for finding the majority element in large arrays.

#### URL Block Diagram:
```
[Unsorted Array] -> [Boyer-Moore Majority Vote Algorithm] -> [Majority Element]
```

#### Example:
Consider an array `[3, 3, 4, 2, 4, 4, 2, 4, 4]`.

#### Output:
The output will be the majority element `4`.

#### DSA Question:
Implement the Boyer-Moore Majority Vote Algorithm in Java to find the majority element in an array.

#### Answer:
```java
public class BoyerMooreMajorityVote {
    public static int majorityElement(int[] nums) {
        int candidate = 0;
        int count = 0;
        
        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }
        
        // Verification Phase
        count = 0;
        for (int num : nums) {
            if (num == candidate) {
                count++;
            }
        }
        
        if (count > nums.length / 2) {
            return candidate;
        }
        
        return -1; // No majority element found
    }
    
    public static void main(String[] args) {
        int[] arr = {3, 3, 4, 2, 4, 4, 2, 4, 4};
        System.out.println("Array: " + Arrays.toString(arr));
        int majority = majorityElement(arr);
        if (majority != -1) {
            System.out.println("The majority element is: " + majority);
        } else {
            System.out.println("No majority element found.");
        }
    }
}
```

#### Output:
```
Array: [3, 3, 4, 2, 4, 4, 2, 4, 4]
The majority element is: 4
```

#### Explanation:
In this example, the Boyer-Moore Majority Vote Algorithm finds the majority element in the array `[3, 3, 4, 2, 4, 4, 2, 4, 4]`, which is `4`. The algorithm efficiently identifies the majority candidate by canceling out pairs of different elements. After the voting phase, it verifies if the potential majority candidate indeed appears more than ⌊ n/2 ⌋ times in the array. If so, it returns the majority element; otherwise, it indicates that no majority element exists.
