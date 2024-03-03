# SORTING ALGORITHMS 

### Table of Contents

1. [Insertion Sort](#insertion-sort)
2. [Heap Sort](#heap-sort)
3. [Selection Sort](#selection-sort)
4. [Merge Sort](#merge-sort)
5. [Quick Sort](#quick-sort)
6. [Counting Sort](#counting-sort)


# SORTING
## Insertion Sort

#### Explanation:
Insertion Sort is a simple sorting algorithm that builds the final sorted array one item at a time. It iterates through the input array, starting from the second element, and repeatedly inserts each element into its proper position among the already sorted elements. At each iteration, the algorithm selects an element from the unsorted part of the array and compares it with the elements in the sorted part. It then shifts the larger elements one position to the right to make space for the selected element. This process continues until the entire array is sorted.

#### URL Block Diagram:
```
[Unsorted Array] -> [Insertion Sort Algorithm] -> [Sorted Array]
```

#### Example:
Consider an array `[5, 2, 4, 6, 1, 3]`.

#### Output:
The output will be the sorted array `[1, 2, 3, 4, 5, 6]`.

#### DSA Question:
Implement the Insertion Sort algorithm in Java to sort an array of integers in ascending order.

#### Answer:
```java
public class InsertionSort {
    public static void insertionSort(int[] arr) {
        int n = arr.length;
        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    }
    
    public static void printArray(int[] arr) {
        for (int num : arr) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        int[] arr = {5, 2, 4, 6, 1, 3};
        System.out.println("Original Array:");
        printArray(arr);
        insertionSort(arr);
        System.out.println("Sorted Array:");
        printArray(arr);
    }
}
```

#### Output:
```
Original Array:
5 2 4 6 1 3 
Sorted Array:
1 2 3 4 5 6 
```

#### Explanation:
In this example, the Insertion Sort algorithm sorts the array `[5, 2, 4, 6, 1, 3]` in ascending order. At each iteration, the algorithm selects an element from the unsorted part of the array and inserts it into its proper position among the sorted elements. This process continues until the entire array is sorted. Insertion Sort has a time complexity of O(n^2) but performs well on small arrays or nearly sorted arrays.

Let's continue with Heap Sort.

## Heap Sort

#### Explanation:
Heap Sort is a comparison-based sorting algorithm that uses a binary heap data structure to sort elements. It operates by first building a max heap (for sorting in ascending order) or a min heap (for sorting in descending order) from the input array. After constructing the heap, the algorithm repeatedly extracts the maximum (or minimum) element from the heap and places it at the end of the array. This process shrinks the heap size by one and continues until the heap is empty. The sorted array is then obtained by repeatedly extracting elements from the heap.

#### URL Block Diagram:
```
[Unsorted Array] -> [Heap Sort Algorithm] -> [Sorted Array]
```

#### Example:
Consider an array `[7, 2, 5, 1, 8, 3]`.

#### Output:
The output will be the sorted array `[1, 2, 3, 5, 7, 8]`.

#### DSA Question:
Implement the Heap Sort algorithm in Java to sort an array of integers in ascending order.

#### Answer:
```java
public class HeapSort {
    public static void heapSort(int[] arr) {
        int n = arr.length;
        
        // Build max heap
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }
        
        // Extract elements from heap one by one
        for (int i = n - 1; i > 0; i--) {
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;
            heapify(arr, i, 0);
        }
    }
    
    public static void heapify(int[] arr, int n, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        
        if (left < n && arr[left] > arr[largest]) {
            largest = left;
        }
        
        if (right < n && arr[right] > arr[largest]) {
            largest = right;
        }
        
        if (largest != i) {
            int temp = arr[i];
            arr[i] = arr[largest];
            arr[largest] = temp;
            heapify(arr, n, largest);
        }
    }
    
    public static void printArray(int[] arr) {
        for (int num : arr) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        int[] arr = {7, 2, 5, 1, 8, 3};
        System.out.println("Original Array:");
        printArray(arr);
        heapSort(arr);
        System.out.println("Sorted Array:");
        printArray(arr);
    }
}
```

#### Output:
```
Original Array:
7 2 5 1 8 3 
Sorted Array:
1 2 3 5 7 8 
```

#### Explanation:
In this example, the Heap Sort algorithm sorts the array `[7, 2, 5, 1, 8, 3]` in ascending order. It first builds a max heap from the array and then repeatedly extracts the maximum element from the heap, placing it at the end of the array. This process continues until the entire array is sorted. Heap Sort has a time complexity of O(n log n) and is an efficient sorting algorithm for large datasets.

Let's proceed with Selection Sort.

## Selection Sort

#### Explanation:
Selection Sort is a simple sorting algorithm that divides the input array into two parts: the sorted subarray and the unsorted subarray. It repeatedly selects the minimum element from the unsorted subarray and swaps it with the first unsorted element, thereby expanding the sorted subarray. This process continues until the entire array is sorted. Selection Sort is not very efficient on large datasets due to its time complexity, but it has the advantage of minimizing the number of swaps compared to other sorting algorithms.

#### URL Block Diagram:
```
[Unsorted Array] -> [Selection Sort Algorithm] -> [Sorted Array]
```

#### Example:
Consider an array `[64, 25, 12, 22, 11]`.

#### Output:
The output will be the sorted array `[11, 12, 22, 25, 64]`.

#### DSA Question:
Implement the Selection Sort algorithm in Java to sort an array of integers in ascending order.

#### Answer:
```java
public class SelectionSort {
    public static void selectionSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            int minIndex = i;
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            int temp = arr[minIndex];
            arr[minIndex] = arr[i];
            arr[i] = temp;
        }
    }
    
    public static void printArray(int[] arr) {
        for (int num : arr) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};
        System.out.println("Original Array:");
        printArray(arr);
        selectionSort(arr);
        System.out.println("Sorted Array:");
        printArray(arr);
    }
}
```

#### Output:
```
Original Array:
64 25 12 22 11 
Sorted Array:
11 12 22 25 64 
```

#### Explanation:
In this example, the Selection Sort algorithm sorts the array `[64, 25, 12, 22, 11]` in ascending order. It repeatedly selects the minimum element from the unsorted part of the array and swaps it with the first unsorted element. This process continues until the entire array is sorted. Selection Sort has a time complexity of O(n^2) and is suitable for small datasets or when the number of swaps is a concern.

Let's discuss Merge Sort.

## Merge Sort

#### Explanation:
Merge Sort is a divide-and-conquer sorting algorithm that divides the input array into two halves, recursively sorts each half, and then merges them to produce a sorted array. The key steps of the Merge Sort algorithm are:

1. **Divide**: The array is divided into two halves.
2. **Conquer**: Each half is recursively sorted.
3. **Merge**: The sorted halves are merged to produce the final sorted array.

The merging process involves comparing elements from the two sorted halves and placing them in the correct order in a temporary array. Merge Sort is efficient and stable, with a time complexity of O(n log n) in all cases.

#### URL Block Diagram:
```
[Unsorted Array] -> [Merge Sort Algorithm] -> [Sorted Array]
```

#### Example:
Consider an array `[38, 27, 43, 3, 9, 82, 10]`.

#### Output:
The output will be the sorted array `[3, 9, 10, 27, 38, 43, 82]`.

#### DSA Question:
Implement the Merge Sort algorithm in Java to sort an array of integers in ascending order.

#### Answer:
```java
public class MergeSort {
    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;
            mergeSort(arr, left, mid);
            mergeSort(arr, mid + 1, right);
            merge(arr, left, mid, right);
        }
    }
    
    public static void merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;
        
        int[] leftArr = new int[n1];
        int[] rightArr = new int[n2];
        
        for (int i = 0; i < n1; i++) {
            leftArr[i] = arr[left + i];
        }
        for (int j = 0; j < n2; j++) {
            rightArr[j] = arr[mid + 1 + j];
        }
        
        int i = 0, j = 0;
        int k = left;
        
        while (i < n1 && j < n2) {
            if (leftArr[i] <= rightArr[j]) {
                arr[k] = leftArr[i];
                i++;
            } else {
                arr[k] = rightArr[j];
                j++;
            }
            k++;
        }
        
        while (i < n1) {
            arr[k] = leftArr[i];
            i++;
            k++;
        }
        
        while (j < n2) {
            arr[k] = rightArr[j];
            j++;
            k++;
        }
    }
    
    public static void printArray(int[] arr) {
        for (int num : arr) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        int[] arr = {38, 27, 43, 3, 9, 82, 10};
        System.out.println("Original Array:");
        printArray(arr);
        mergeSort(arr, 0, arr.length - 1);
        System.out.println("Sorted Array:");
        printArray(arr);
    }
}
```

#### Output:
```
Original Array:
38 27 43 3 9 82 10 
Sorted Array:
3 9 10 27 38 43 82 
```

#### Explanation:
In this example, the Merge Sort algorithm sorts the array `[38, 27, 43, 3, 9, 82, 10]` in ascending order. It recursively divides the array into halves, sorts each half, and then merges them back together to produce the final sorted array. Merge Sort guarantees a time complexity of O(n log n) and is efficient for large datasets.

Let's move on to Quick Sort.

## Quick Sort

#### Explanation:
Quick Sort is a comparison-based sorting algorithm that uses a divide-and-conquer strategy to sort elements efficiently. It works by selecting a pivot element from the array and partitioning the other elements into two subarrays according to whether they are less than or greater than the pivot. The subarrays are then recursively sorted. The key steps of the Quick Sort algorithm are:

1. **Partitioning**: Choose a pivot element and partition the array into two subarrays such that elements less than the pivot are on the left, and elements greater than the pivot are on the right.
2. **Recursion**: Recursively apply Quick Sort to the left and right subarrays.
3. **Combining**: No combining step is required as the array is sorted in place.

Quick Sort is efficient and has an average-case time complexity of O(n log n) and a worst-case time complexity of O(n^2). However, it performs well in practice due to its good average-case behavior and low memory overhead.

#### URL Block Diagram:
```
[Unsorted Array] -> [Quick Sort Algorithm] -> [Sorted Array]
```

#### Example:
Consider an array `[10, 7, 8, 9, 1, 5]`.

#### Output:
The output will be the sorted array `[1, 5, 7, 8, 9, 10]`.

#### DSA Question:
Implement the Quick Sort algorithm in Java to sort an array of integers in ascending order.

#### Answer:
```java
public class QuickSort {
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pivotIndex = partition(arr, low, high);
            quickSort(arr, low, pivotIndex - 1);
            quickSort(arr, pivotIndex + 1, high);
        }
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
    
    public static void printArray(int[] arr) {
        for (int num : arr) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        int[] arr = {10, 7, 8, 9, 1, 5};
        System.out.println("Original Array:");
        printArray(arr);
        quickSort(arr, 0, arr.length - 1);
        System.out.println("Sorted Array:");
        printArray(arr);
    }
}
```

#### Output:
```
Original Array:
10 7 8 9 1 5 
Sorted Array:
1 5 7 8 9 10 
```

#### Explanation:
In this example, the Quick Sort algorithm sorts the array `[10, 7, 8, 9, 1, 5]` in ascending order. It chooses a pivot element (in this case, the last element), partitions the array into two subarrays, and recursively applies Quick Sort to each subarray. Quick Sort has an average-case time complexity of O(n log n) and performs well on large datasets, making it a popular choice for sorting.

The next algorithm on our list is Counting Sort.

## Counting Sort

#### Explanation:
Counting Sort is a non-comparison-based sorting algorithm that works by counting the occurrences of each unique element in the input array and using this information to determine the position of each element in the sorted output array. It operates by following these steps:

1. **Counting**: Count the occurrences of each unique element in the input array and store the counts in an auxiliary array.
2. **Accumulating**: Compute the cumulative sum of counts in the auxiliary array.
3. **Placing**: Place each element from the input array into its correct position in the sorted output array based on the cumulative counts.

Counting Sort assumes that the input elements are integers within a specific range. It has a time complexity of O(n + k), where n is the number of elements in the input array and k is the range of the input.

#### URL Block Diagram:
```
[Unsorted Array] -> [Counting Sort Algorithm] -> [Sorted Array]
```

#### Example:
Consider an array `[4, 2, 2, 8, 3, 3, 1]`.

#### Output:
The output will be the sorted array `[1, 2, 2, 3, 3, 4, 8]`.

#### DSA Question:
Implement the Counting Sort algorithm in Java to sort an array of integers in ascending order.

#### Answer:
```java
public class CountingSort {
    public static void countingSort(int[] arr) {
        int max = getMax(arr);
        int[] count = new int[max + 1];
        int[] output = new int[arr.length];
        
        // Count occurrences of each element
        for (int num : arr) {
            count[num]++;
        }
        
        // Compute cumulative sum of counts
        for (int i = 1; i <= max; i++) {
            count[i] += count[i - 1];
        }
        
        // Place elements in sorted order
        for (int i = arr.length - 1; i >= 0; i--) {
            output[count[arr[i]] - 1] = arr[i];
            count[arr[i]]--;
        }
        
        // Copy sorted elements back to original array
        for (int i = 0; i < arr.length; i++) {
            arr[i] = output[i];
        }
    }
    
    public static int getMax(int[] arr) {
        int max = arr[0];
        for (int num : arr) {
            if (num > max) {
                max = num;
            }
        }
        return max;
    }
    
    public static void printArray(int[] arr) {
        for (int num : arr) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        int[] arr = {4, 2, 2, 8, 3, 3, 1};
        System.out.println("Original Array:");
        printArray(arr);
        countingSort(arr);
        System.out.println("Sorted Array:");
        printArray(arr);
    }
}
```

#### Output:
```
Original Array:
4 2 2 8 3 3 1 
Sorted Array:
1 2 2 3 3 4 8 
```

#### Explanation:
In this example, the Counting Sort algorithm sorts the array `[4, 2, 2, 8, 3, 3, 1]` in ascending order. It counts the occurrences of each unique element, computes the cumulative sum of counts, and then places each element in its correct position in the sorted output array. Counting Sort is efficient for sorting integers within a specific range and has a time complexity of O(n + k), where n is the number of elements and k is the range of the input.

