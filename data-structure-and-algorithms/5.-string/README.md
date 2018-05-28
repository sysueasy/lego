# 5. String

The plan of this chapter is as follows: After addressing basic properties of strings, we revisit in Sections 5.1 and 5.2 the **sorting** and **searching** APIs from Chapters 2 and 3. Algorithms that exploit special properties of string keys are faster and more flexible than the algorithms that we considered earlier. In Section 5.3 we consider algorithms for **substring** search, including a famous algorithm due to **Knuth, Morris, and Pratt**. In Section 5.4 we introduce **regular expressions**, the basis of the pattern-matching problem, a generalization of substring search, and a quintessential search tool known as **grep**. These classic algorithms are based on the related conceptual devices known as **formal languages** and **finite automata**. Section 5.5 is devoted to a central application: **data compression**, where we try to reduce the size of a string as much as possible.

## Recap

A Java **String** is a sequence of characters. For many decades, programmers restricted attention to characters encoded in 7-bit **ASCII** or 8-bit extended ASCII, but many modern applications call for 16-bit **Unicode**.

```java
char c = s.charAt(3)
int length = s.length()
String sub = s.substring(7, 11)
```

