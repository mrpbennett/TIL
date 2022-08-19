# Arrays

With arrays you will have to declear its type first like so: `int[]` this is an array of intergers.

```java
String[] someStrings = new String[5]
```

The above will hold an array of five strings.

```java
public static void main(String[] args) {
        // Arrays

        int[] numbers = new int[10];

        // adds 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 to numbers
        for (int i = 0; i < numbers.length; i++) {
            numbers[i] = i + 0;
        }

        System.out.println(Arrays.toString(numbers));


    }
```