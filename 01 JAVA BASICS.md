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

### Complexity

* Time: O(n) — single pass over each string (plus potential map overhead if non-ASCII)
* Space: O(1) for fixed-size array (O(k) where k is alphabet size); O(u) for Unicode map (u = unique codepoints)

---

## Notes & Best Practices

* **Normalization:** Remove spaces and normalize case (`toLowerCase()`) when you want to treat `"Debit Card"` and `"Bad Credit"` as anagrams.
* **Character set:** For English/ASCII, using an `int[128]` or `int[26]` (if only letters a–z) is fastest. For full Unicode support use code points and a `Map<Integer,Integer>`.
* **Early exits:** Always check length equality after normalization; if lengths differ they cannot be anagrams.
* **Security:** Avoid using `replaceAll` with user input in some security-sensitive contexts without validation (though common here).

---

## Unit Test Examples (JUnit 5)

```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class AnagramTest {
    @Test
    public void testSortApproach() {
        assertTrue(AnagramSort.areAnagrams("listen", "silent"));
        assertFalse(AnagramSort.areAnagrams("hello", "bello"));
    }

    @Test
    public void testCountApproach() {
        assertTrue(AnagramCount.areAnagrams("Debit Card", "Bad Credit"));
        assertFalse(AnagramCount.areAnagrams("apple", "pale"));
        assertTrue(AnagramCount.areAnagrams("résumé", "sérumé"));
    }
}
```

---
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




