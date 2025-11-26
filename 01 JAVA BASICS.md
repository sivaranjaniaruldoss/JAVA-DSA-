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

## Quick Checklist for Using the Code

1. Choose **Sort** if you prefer simplicity and your strings are short.
2. Choose **Count** for large strings or performance-critical paths.
3. Use **Unicode fallback** when inputs may contain non-ASCII characters.
4. Normalize input (remove spaces, punctuation if needed) consistently.

---

