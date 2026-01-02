# Anagram Checker — Java Implementation & Explanation

This document explains two common approaches to check whether two strings are anagrams, provides Java code for each, explains complexity, and shows examples and tests. Copy this into a `.md` file.

---

## What is an anagram?

Two strings are **anagrams** if they contain the exact same characters with the same frequencies, but possibly in different orders.
Example: `"listen"` and `"silent"` are anagrams.

---

## Approach 1 — Sort-and-Compare

### Idea

Sort the characters of both strings and compare the sorted results. If they are identical, the strings are anagrams.

### Java implementation

```java
public class AnagramSort {
    public static boolean areAnagrams(String s1, String s2) {
        if (s1 == null || s2 == null) return false;
        s1 = s1.replaceAll("\\s+", "").toLowerCase();
        s2 = s2.replaceAll("\\s+", "").toLowerCase();

        if (s1.length() != s2.length()) return false;

        char[] a1 = s1.toCharArray();
        char[] a2 = s2.toCharArray();
        java.util.Arrays.sort(a1);
        java.util.Arrays.sort(a2);

        return java.util.Arrays.equals(a1, a2);
    }

    public static void main(String[] args) {
        System.out.println(areAnagrams("listen", "silent")); // true
        System.out.println(areAnagrams("triangle", "integral")); // true
        System.out.println(areAnagrams("apple", "pale")); // false
    }
}
```

### Complexity

* Time: O(n log n) due to sorting (n = length of string)
* Space: O(n) for the character arrays (in-place sort may reduce extra space slightly)

---

## Approach 2 — Character Count (Linear time)

### Idea

Count frequency of each character in the first string, decrement the counts using the second string. If all counts end up zero, they are anagrams. This is O(n) time.

> This implementation assumes ASCII or Unicode handling described below. For ASCII (or only lowercase letters) you can use a fixed-size array for speed.

### Java implementation (ASCII / general safe approach)

```java
public class AnagramCount {
    public static boolean areAnagrams(String s1, String s2) {
        if (s1 == null || s2 == null) return false;
        s1 = s1.replaceAll("\\s+", "").toLowerCase();
        s2 = s2.replaceAll("\\s+", "").toLowerCase();

        if (s1.length() != s2.length()) return false;

        // Use an int array sized for standard ASCII (128) or extended as needed.
        // For Unicode beyond BMP, a map approach is safer.
        int[] counts = new int[128];

        for (int i = 0; i < s1.length(); i++) {
            char c = s1.charAt(i);
            if (c < 128) counts[c]++;
            else return fallbackUnicodeCheck(s1, s2); // fallback if non-ASCII detected
        }

        for (int i = 0; i < s2.length(); i++) {
            char c = s2.charAt(i);
            if (c < 128) {
                counts[c]--;
                if (counts[c] < 0) return false; // early exit
            } else return fallbackUnicodeCheck(s1, s2);
        }

        // all counts should be zero
        for (int cnt : counts) {
            if (cnt != 0) return false;
        }
        return true;
    }

    // Fallback using HashMap for full Unicode safety (slower but correct)
    private static boolean fallbackUnicodeCheck(String s1, String s2) {
        java.util.Map<Integer, Integer> map = new java.util.HashMap<>();
        s1.codePoints().forEach(cp -> map.merge(cp, 1, Integer::sum));
        s2.codePoints().forEach(cp -> map.merge(cp, -1, Integer::sum));
        return map.values().stream().allMatch(v -> v == 0);
    }

    public static void main(String[] args) {
        System.out.println(areAnagrams("listen", "silent")); // true
        System.out.println(areAnagrams("Debit Card", "Bad Credit")); // true (ignores spaces & case)
        System.out.println(areAnagrams("résumé", "sérumé")); // true (uses fallback)
    }
}
```





# Problem: Sum of Suffix Numbers

## Description
Given a positive integer `n`, calculate the sum of all suffix numbers formed from its digits.  
A suffix number is defined as the last *k* digits of `n`, where `k` ranges from 1 up to the total number of digits in `n`.

### Example
- Input: `1653`  
- Suffixes: `3, 53, 653, 1653`  
- Sum = `3 + 53 + 653 + 1653 = 2362`  
- Output: `2362`

## Constraints

- Input is a positive integer.

## Solution (Java)
```java
public class SuffixSum {
    public static int sumOfSuffixes(int num) {
        int sum = num;
        int divisor = 10;
        while (divisor <= num) {
            sum += num % divisor;
            divisor *= 10;
        }
        return sum;
    }

    public static void main(String[] args) {
        int num = 1653;
        System.out.println(sumOfSuffixes(num)); 
    }
}
```

---
## while loop

## Problem 1: Print numbers from 1 to N

Input: `5`  
Output: `1 2 3 4 5`

```java
int n = 5;
int i = 1;

while (i <= n) {
    System.out.print(i + " ");
    i++;
}
````

---

## Problem 2: Print numbers from N to 1

Input: `5`
Output: `5 4 3 2 1`

```java
int n = 5;

while (n >= 1) {
    System.out.print(n + " ");
    n--;
}
```

---

## Problem 3: Count digits in a number

Input: `78452`
Output: `5`

```java
int n = 78452;
int count = 0;

while (n > 0) {
    count++;
    n = n / 10;
}

System.out.println(count);
```

---

## Problem 4: Sum of digits

Input: `345`
Output: `12`

```java
int n = 345;
int sum = 0;

while (n > 0) {
    sum = sum + n % 10;
    n = n / 10;
}

System.out.println(sum);
```

---

## Problem 5: Reverse a number

Input: `1234`
Output: `4321`

```java
int n = 1234;
int rev = 0;

while (n > 0) {
    rev = rev * 10 + n % 10;
    n = n / 10;
}

System.out.println(rev);
```

---

## Problem 6: Check palindrome number

Input: `121`
Output: `Palindrome`

```java
int n = 121;
int temp = n;
int rev = 0;

while (n > 0) {
    rev = rev * 10 + n % 10;
    n = n / 10;
}

if (rev == temp)
    System.out.println("Palindrome");
else
    System.out.println("Not Palindrome");
```

---

## Problem 7: Product of digits

Input: `234`
Output: `24`

```java
int n = 234;
int product = 1;

while (n > 0) {
    product = product * (n % 10);
    n = n / 10;
}

System.out.println(product);
```

---

## Problem 8: Count even digits

Input: `48291`
Output: `2`

```java
int n = 48291;
int count = 0;

while (n > 0) {
    int d = n % 10;
    if (d % 2 == 0)
        count++;
    n = n / 10;
}

System.out.println(count);
```

---

## Problem 9: Find first and last digit

Input: `6789`
Output: `First: 6 Last: 9`

```java
int n = 6789;
int last = n % 10;

while (n >= 10) {
    n = n / 10;
}

System.out.println("First: " + n + " Last: " + last);
```

---

## Problem 10: Print digits in correct order

Input: `456`
Output: `4 5 6`

```java
int n = 456;
int temp = n;
int div = 1;

while (temp > 9) {
    div = div * 10;
    temp = temp / 10;
}

while (div > 0) {
    System.out.print(n / div + " ");
    n = n % div;
    div = div / 10;
}
```


---



---

## Problem 1: Infinite Loop Trap
**Description:** The loop decreases `i` instead of increasing it, so the condition `i <= 5` is always true.  
```
for (int i = 1; i <= 5; i--)
Solution: 1, 0, -1, -2, -3, ...
```
## Problem 2: Post Increment Confusion
Description: The loop increments i twice — once in the loop header and once in the print statement.

```
for (int i = 1; i <= 3; i++) 
    System.out.print(i++ + "");
Solution: Prints 1 3 because the extra i++ skips values.
```
## Problem 3: Pre Increment in Condition
Description: The condition uses ++i, so i is incremented before checking.
```

for (int i = 0; ++i <= 3;)
    System.out.print(i + " ");
Solution: Prints 1 2 3.

```
## Problem 4: Double Increment
Description: i is incremented in both the loop header and inside the body.
```

for (int i = 1; i <= 5; i++) { 
    i++; 
}
Solution: Prints 1 3 5.
```
## Problem 5: Empty Condition Loop
Description: The loop has no initialization or increment in the header, only in the body.
```

int i = 0;
for (; i < 3;) 
    i++;
Solution: Runs until i reaches 3, effectively iterating over 0 1 2.
```
## Problem 6: Do while loop
Write a program using a do-while loop that repeatedly asks the user to enter a positive integer.

If the user enters a negative number, the program should stop.
While the user keeps entering positive numbers, the program should calculate the sum of all positive integers entered.Finally, print the total sum when the loop ends
```
import java.util.Scanner;

public class DoWhileSum {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int number;
        int sum = 0;

        
        do {
            System.out.print("Enter a number: ");
            number = sc.nextInt();

            if (number > 0) {
                sum += number; 
            }
        } while (number >= 0); 

        System.out.println("Total sum = " + sum);
        sc.close();
    }
}
```
## Problem 7 Do while loop 
Write a program using a do-while loop that asks the user to enter a number between 1 and 10.

If the user enters a number outside this range, the program should keep asking until a valid number is entered.

Once a valid number is entered, print:

"You entered a valid number: X" (where X is the number).
```
import java.util.Scanner;

public class DoWhileValidation {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int number;

        
        do {
            System.out.print("Enter a number between 1 and 10: ");
            number = sc.nextInt();

            if (number < 1 || number > 10) {
                System.out.println("Invalid! Try again.");
            }
        } while (number < 1 || number > 10);

        System.out.println("You entered a valid number: " + number);
        sc.close();
    }
}
```
## Right-Angled Triangle of Stars
```
public class Pattern1 {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
}
```
## Number Pyramid
```
public class Pattern2 {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print(j + " ");
            }
            System.out.println();
        }
    }
}
```
## Alphabet Triangle
```
public class Pattern3 {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 0; i < n; i++) {
            char ch = 'A';
            for (int j = 0; j <= i; j++) {
                System.out.print(ch + " ");
                ch++;
            }
            System.out.println();
        }
    }
}
```
## Inverted Pyramid
```
public class Pattern4 {
    public static void main(String[] args) {
        int n = 5;
        for (int i = n; i >= 1; i--) {
            for (int j = 1; j <= i; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
}
```
## Diamond pattern
```
public class DiamondPattern {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 1; i <= n; i++) {
            for (int j = i; j < n; j++) {
                System.out.print(" ");
            }
            for (int j = 1; j <= (2 * i - 1); j++) {
                System.out.print("*");
            }
            System.out.println();
        }
        for (int i = n - 1; i >= 1; i--) {
            for (int j = n; j > i; j--) {
                System.out.print(" ");
            }
            for (int j = 1; j <= (2 * i - 1); j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```
## RECURSION


## 1. Factorial of a Number
```
int fact(int n) {
    if (n == 0 || n == 1) return 1;   
    return n * fact(n - 1);           
}
```
## 2. Fibonacci Series
```
int fib(int n) {
    if (n <= 1) return n;            
    return fib(n - 1) + fib(n - 2);   
}
```
## 3. Sum of Digits
```
int sumDigits(int n) {
    if (n == 0) return 0;             
    return (n % 10) + sumDigits(n / 10);
}
```
## 4. Reverse a Number
```
int reverse(int n, int rev) {
    if (n == 0) return rev;           // base case
    return reverse(n / 10, rev * 10 + n % 10);
}
```
## 5.Check Palindrome Number
```
boolean isPalindrome(int n, int rev, int temp) {
    if (n == 0) return temp == rev;   // base case
    return isPalindrome(n / 10, rev * 10 + n % 10, temp);
}
```
## 6. Tower of Hanoi
```
void hanoi(int n, char from, char to, char aux) {
    if (n == 1) {
        System.out.println("Move disk 1 from " + from + " to " + to);
        return;
    }
    hanoi(n - 1, from, aux, to);
    System.out.println("Move disk " + n + " from " + from + " to " + to);
    hanoi(n - 1, aux, to, from);
}
```
## 7. Binary Tree Problems
```
int height(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(height(root.left), height(root.right));
}
```
## 8. Print All Subsets of a String
```
void subsets(String s, String current, int index) {
    if (index == s.length()) {
        System.out.println(current);
        return;
    }
   
    subsets(s, current + s.charAt(index), index + 1);
subsets(s, current, index + 1);
}
```
## 9. Generate All Permutations of a String
```
void permute(String s, String prefix) {
    if (s.length() == 0) {
        System.out.println(prefix);
        return;
    }
    for (int i = 0; i < s.length(); i++) {
        permute(s.substring(0, i) + s.substring(i + 1), prefix + s.charAt(i));
    }
}
```
## 10. Count Paths in a Grid 
```
int countPaths(int m, int n) {
    if (m == 1 || n == 1) return 1; 
    return countPaths(m - 1, n) + countPaths(m, n - 1);
}
```
## 11. N-Queens Problem
```
boolean solveNQueens(int board[][], int row, int n) {
    if (row >= n) return true;

    for (int col = 0; col < n; col++) {
        if (isSafe(board, row, col, n)) {
            board[row][col] = 1;
            if (solveNQueens(board, row + 1, n)) return true;
            board[row][col] = 0; 
        }
    }
    return false;
}
```
## 12. Subset Sum Problem
```
boolean subsetSum(int[] arr, int n, int sum) {
    if (sum == 0) return true;
    if (n == 0) return false;

    if (arr[n - 1] > sum) return subsetSum(arr, n - 1, sum);

    return subsetSum(arr, n - 1, sum) || subsetSum(arr, n - 1, sum - arr[n - 1]);
}

```
## 13. Armstrong Number
Check if a number is equal to the sum of cubes of its digits.
```

int n = 153, temp = n, sum = 0;
while (temp > 0) {
    int d = temp % 10;
    sum += d * d * d;
    temp /= 10;
}
System.out.println(sum == n ? "Armstrong" : "Not Armstrong");
```
## 14. Strong Number
```
Sum of factorials of digits equals the number.


int n = 145, temp = n, sum = 0;
while (temp > 0) {
    int d = temp % 10;
    int fact = 1;
    for (int i = 1; i <= d; i++) fact *= i;
    sum += fact;
    temp /= 10;
}
System.out.println(sum == n ? "Strong Number" : "Not Strong Number");
```
## 15. Perfect Number
```
Sum of divisors equals the number.

java
int n = 28, sum = 0;
for (int i = 1; i < n; i++) {
    if (n % i == 0) sum += i;
}
System.out.println(sum == n ? "Perfect Number" : "Not Perfect Number");
```
## 16.Prime Factors of a Number
```
java
int n = 84;
for (int i = 2; i <= n; i++) {
    while (n % i == 0) {
        System.out.print(i + " ");
        n /= i;
    }
}
```
## 17. Palindrome String
```
String s = "madam";
String rev = new StringBuilder(s).reverse().toString();
System.out.println(s.equals(rev) ? "Palindrome" : "Not Palindrome");
```
## 18. Anagram Check
```
String s1 = "listen", s2 = "silent";
char[] a1 = s1.toCharArray();
char[] a2 = s2.toCharArray();
java.util.Arrays.sort(a1);
java.util.Arrays.sort(a2);
System.out.println(java.util.Arrays.equals(a1, a2) ? "Anagram" : "Not Anagram");


```

## 1️ Find Largest Digit in a Number

**Problem:**  
Given an integer `n`, find the largest digit present in it.

**Example:**  
Input: `57249`  
Output: `9`

**Solution:**
```java
int n = 57249;
int max = 0;

while (n > 0) {
    int digit = n % 10;
    if (digit > max) {
        max = digit;
    }
    n = n / 10;
}

System.out.println(max);
```
## Problem 2:
```
Calculate base^exp using loops only.

Example:
Input: 2 5
Output: 32

Solution:

int base = 2;
int exp = 5;
int result = 1;

for (int i = 1; i <= exp; i++) {
    result = result * base;
}

System.out.println(result);
```
## binary search
iterative approach
```
public class BinarySearchIterative {
    public static int binarySearch(int[] arr, int target) {
        int low = 0, high = arr.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2; // avoids overflow
            if (arr[mid] == target) return mid;
            else if (arr[mid] < target) low = mid + 1;
            else high = mid - 1;
        }
        return -1; // not found
    }

    public static void main(String[] args) {
        int[] nums = {1, 3, 5, 7, 9, 11};
        System.out.println(binarySearch(nums, 7));  // Output: 3
        System.out.println(binarySearch(nums, 4));  // Output: -1
    }
}
```
Recursive Method
```
public class BinarySearchRecursive {
    public static int binarySearch(int[] arr, int low, int high, int target) {
        if (low > high) return -1;
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) 
            return binarySearch(arr, mid + 1, high, target);
        else 
            return binarySearch(arr, low, mid - 1, target);
    }

    public static void main(String[] args) {
        int[] nums = {2, 4, 6, 8, 10, 12};
        System.out.println(binarySearch(nums, 0, nums.length - 1, 10)); // Output: 4
        System.out.println(binarySearch(nums, 0, nums.length - 1, 5));  // Output: -1
    }
}
```
## Linear Search in an Array
```
public class LinearSearchArray {
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i; // return index if found
            }
        }
        return -1; // not found
    }

    public static void main(String[] args) {
        int[] nums = {10, 25, 30, 45, 50};
        int target = 30;

        int result = linearSearch(nums, target);
        if (result == -1) {
            System.out.println("Element not found");
        } else {
            System.out.println("Element found at index " + result);
        }
    }
}
```
##  Linear Search in a String Array
```
public class LinearSearchString {
    public static int linearSearch(String[] arr, String target) {
        target = target.toLowerCase();
        for (int i = 0; i < arr.length; i++) {
            if (arr[i].toLowerCase().equals(target)) {
                return i; // return index if found
            }
        }
        return -1; // not found
    }

    public static void main(String[] args) {
        String[] fruits = {"apple", "banana", "cherry", "date"};
        String target = "Cherry";

        int result = linearSearch(fruits, target);
        if (result == -1) {
            System.out.println("Element not found");
        } else {
            System.out.println("Element found at index " + result);
        }
    }
}
```
## Two Sum
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input has exactly one solution, and you may not use the same element twice.
Return the answer in any order.
```
import java.util.*;

public class TwoSum {
    public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
        return new int[] {}; // no solution
    }

    public static void main(String[] args) {
        int[] nums = {2, 7, 11, 15};
        int target = 9;
        int[] result = twoSum(nums, target);
        System.out.println(Arrays.toString(result)); // Output: [0, 1]
    }
}
```
## Prime Number Check
```
public class PrimeNumberCheck {
    public static boolean isPrime(int n) {
        if (n <= 1) return false;
        if (n <= 3) return true;
        if (n % 2 == 0 || n % 3 == 0) return false;
        for (int i = 5; i * i <= n; i += 6) {
            if (n % i == 0 || n % (i + 2) == 0) return false;
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println(isPrime(7));  
        System.out.println(isPrime(10)); 
        System.out.println(isPrime(1));  
    }
}
```
## Problem: Longest Palindromic Substring
```
Description
Given a string s, return the longest palindromic substring in s.
A palindrome is a string that reads the same forward and backward.
```
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) return "";
        int start = 0, end = 0;

        for (int i = 0; i < s.length(); i++) {
            int len1 = expandFromCenter(s, i, i);       // odd length
            int len2 = expandFromCenter(s, i, i + 1);   // even length
            int len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }

    private int expandFromCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return right - left - 1;
    }
}
