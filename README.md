# INF1-OP CheatSheet

## Eclipse's AutoComplete Suggestions
Use `Ctrl` + `Space` to see which functions are available, together with the signature and description of
each function.

> __Beware:__
>
> Pressing `Ctrl` + `Space` more than once will cycle through other options which you DO NOT WANT.
>
> Press `Esc` to close the suggestions, and try again if that happens.

You can also use AutoComplete Suggestions to get the description of the functions you've already used. To
do so, simply put the caret at the end of functions' name and before the parenthesis, and press `Ctrl` + `Space`:

```java
public class MyClass {
	public static void main(String[] args) {
		System.out.println|("Hello, World!");
		                  ^
		                  The caret should be here when you press Ctrl + Space
	}
}
```

> __Beware:__
>
> If there is text towards the right of the cursor, the AutoComplete Suggestions will NOT take them into consideration.


## Printing and String Formatting

### Printing Arrays

```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		int[] xs = new int[5];
		
		System.out.println(Arrays.toString(xs));  // [0, 0, 0, 0, 0]
	}
}
```

### Creating Formatted Strings
You can use `String.format()` to create formatted strings, when you are asked to write a `toString()` method for a custom class for instance. Compare the following two:

```java
public class Video {
	String title;
	boolean checkedOut = false;
	
	public String toString() {
		// Manually
		return "Video[title=\"" + this.title + "\", checkedOut=" + this.checkedOut + "]";
		
		// Using String.format
		return String.format("Video[title=\"%s\", checkedOut=%s]", this.title, this.checkedOut);
	}
}
```

Using `String.format()` makes the code more readable and less error-prone. `%s` in the first string (*i.e.* format) are called format specifiers, and are used as **placeholders**.

| Specifier | Description            | Use for                          |
|-----------|------------------------|----------------------------------|
| `%d`      | decimal integers       | `int`, `long`, `short`, `byte`   |
| `%f`      | floating-point numbers | `float`, `double`                |
| `%e`      | floating-point numbers (scientific notation) | `float`, `double` |
| `%c`      | single character       | `char`                           |
| `%s`      | string                 | `String`, `Object`(s), `boolean` |

> __Beware:__
>
> Specifiers are case-sensitive!


#### Formatting Floating-Point Numbers with Custom Precision

   You'll often want to format a floating-point number with a custom precision, such as 3 digits after the decimal point. In that case:
  
   ```java
   public class MyClass {
   	public static void main(String[] args) {
   		double pi = 3.14159265;
   		System.out.println(String.format("%.3f", pi));  // 3.142
   		System.out.println(String.format("%.3e", pi));  // 3.142e+00
   	}
   }
   ```
  
   Notice the `.3` within the `%f`, which tells us to print 3 digits after the decimal point.
 
   Also beware that the number is rounded to `3.142` (instead of `3.141`).

## Gotcha's!

### Comparing for Equality
Use `.equals()` method whenever it exists, because `==` checks for identity (*i.e.* two objects are identical) whereas `.equals()` checks for *equality*.

```java
public class MyClass {
	public static void main(String[] args) {
		System.out.println(new String("Hello, World!")   ==   "Hello, World!");   // false
		System.out.println(new String("Hello, World!").equals("Hello, World!"));  // true
	}
}
```

### Arrays are Mutable!
Whenever you pass an array as an argument to a function, the array itself can be **modified**.

```java
import java.util.Arrays;

public class MyClass {
	public static void main(String[] args) {
		int[] numbers = new int[6];
		System.out.println(Arrays.toString(numbers));  // [0, 0, 0, 0, 0, 0]
		modifyMyArray(numbers);
		System.out.println(Arrays.toString(numbers));  // [4, 8, 15, 16, 23, 42]
	}
	
	public static void modifyMyArray(int[] arr) {
		arr[0] =  4;
		arr[1] =  8;
		arr[2] = 15;
		arr[3] = 16;
		arr[4] = 23;
		arr[5] = 42;
	}
}
```

