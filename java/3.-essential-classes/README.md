# 3. Essential Classes

## Exception

In Java, code that might throw certain exceptions must be enclosed by either of the following:

A try statement that catches the exception:

```java
try {

} catch (ExceptionType name) {

} catch (ExceptionType name) {

} finally {

}
```

A method that specifies that it can throw the exception:

```java
// add a throws clause to the method declaration
public void writeList() throws IOException {
}
```



