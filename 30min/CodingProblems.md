# Coding Problems

## Write a function that reverses the order of words in a given sentence, but keeps the words themselves unchanged.

### Concepts: Easy. String manipulation, split(), String.join()

### Example:
Input:  `"Hello world this is Java"`
Output: `"Java is this world Hello"`

### Constraints:
- Words are separated by single spaces.
- No leading or trailing spaces.

### Example answers

#### Streams one-liner

```java
public static String reverseWords(String sentence) {
    return Arrays.stream(sentence.split(" "))
                 .collect(Collectors.collectingAndThen(Collectors.toList(), list -> {
                     Collections.reverse(list);
                     return String.join(" ", list);
                 }));
}
```

#### Manual Swap no Collection reverse, No Collections API
```java
public static String reverseWords(String sentence) {
    String[] words = sentence.split(" ");
    int left = 0, right = words.length - 1;
    while (left < right) {
        String temp = words[left];
        words[left++] = words[right];
        words[right--] = temp;
    }
    return String.join(" ", words);
}
```

# Find the Most Frequent Element

Given an array of integers, return the most frequently occurring element. If there are multiple, return any.

### Example:
Input:  `[1, 3, 3, 2, 1, 3, 2, 2, 2]`
Output: `2`

### Constraints:
- Solve in **O(n)** time.
- The array is non-empty.

### Example using HashMap

```java
public static int mostFrequent(int[] nums) {
    Map<Integer, Integer> freq = new HashMap<>();
    int maxCount = 0, result = nums[0];
    
    for (int num : nums) {
        int count = freq.getOrDefault(num, 0) + 1;
        freq.put(num, count);
        if (count > maxCount) {
            maxCount = count;
            result = num;
        }
    }
    return result;
}
```

# Find First Non-Repeating Character (Strings, HashMap)

### Concepts: Mid. HashMap, iteration, LinkedHashMap

### Example:
Input: `"aabbccdeeffg"`
Output: `'d'`

### Constraints:
- Consider case-sensitive (`'A'` ≠ `'a'`).
- The string contains only lowercase/uppercase English letters.

### Example answers

#### Using Streams
```java
public static char firstNonRepeatingChar(String s) {
    Map<Character, Integer> freq = new LinkedHashMap<>();
    for (char c : s.toCharArray()) freq.put(c, freq.getOrDefault(c, 0) + 1);
    return freq.entrySet().stream().filter(e -> e.getValue() == 1).map(Map.Entry::getKey).findFirst().orElse('_');
}
```

#### Without Streams
```java
public static char firstNonRepeatingChar(String s) {
    Map<Character, Integer> countMap = new LinkedHashMap<>();
    for (char c : s.toCharArray()) countMap.put(c, countMap.getOrDefault(c, 0) + 1);
    for (char c : s.toCharArray()) if (countMap.get(c) == 1) return c;
    return '_';
}
```