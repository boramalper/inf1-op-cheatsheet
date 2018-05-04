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

### Printing Lists

```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		List<Integer> xs = new LinkedList<Integer>(Arrays.asList(4, 8, 15, 16, 23, 42));
		System.out.println(Arrays.toString(xs.toArray()));  // [4, 8, 15, 16, 23, 42]
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

#### Printing Formatted Strings
Instead of using `System.out.println(String.format(...))` every single time, you can use `System.out.printf()` instead, takes the same arguments as `String.format()`, but prints them instead of returning a formatted string.

> __Beware:__
>
> You might want to manually add a new-line character `\n` at the end of your format string (*i.e.* first argument you supply).


#### Special Use Cases

- __Formatting Floating-Point Numbers with Custom Precision__

  You'll often want to format a floating-point number with custom precision, such as 3 digits after the decimal point. In that case:
  
  ```java
  public class MyClass {
  	public static void main(String[] args) {
  		double pi = 3.14159265;
  		System.out.printf("%.3f\n", pi);  // 3.142
  		System.out.printf("%.3e\n", pi);  // 3.142e+00
  	}
  }
  ```
  
  Notice the `.3` within the `%f`, which tells us to print __3__ digits after the decimal point.
 
  Also beware that the number is rounded to `3.142` (instead of `3.141`).

- __Comma Grouping__

  ```java
  public class MyClass {
  	public static void main(String[] args) {
  		int population = 1382300000;
  		System.out.printf("%,d\n",population);  // 1,382,300,000
  	}
  }
  ```
  
  Notice the `,` within the `%d`.
  
- __Padding:__

  ```java
  public class MyClass {
  	public static void main(String[] args) {
  		int shortNo = 12, exactNo = 12345, longNo = 1234567;
  		System.out.printf("%5d - Short\n", shortNo);
  		System.out.printf("%05d - Short (zero paddded)\n", shortNo);
  		System.out.printf("%-5d - Short (left aligned)\n", shortNo);
  		System.out.printf("%5d - Exact\n", exactNo);
  		System.out.printf("%5d - Long\n", longNo);
  	}
  }
  ```
  
  will print
  
  ```
     12 - Short
  00012 - Short (zero paddded)
  12    - Short (left aligned)
  12345 - Exact
  1234567 - Long
  ```

  Notice the `5` within the `%d`.
  
  Also beware that when the result is longer than the padding (*e.g.* the case with `longNo`), it
  is formatted as before.

## Arrays & Lists
As a thumb of rule, you **must** use lists whenever the number of items in it might change, which is impossible to be handled by arrays which have a fixed length upon creation.

If you are dynamically populating your array, you should consider using lists, since you won't have to keep track of the index of the last item you put to your array (otherwise you wouldn't know where to put the next item); instead you'll simply say `myList.add()` to append a new item.

### Converting Arrays and Lists to Each Other

#### Array to List
```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		Integer[] myArray = new Integer[]{4, 8, 15, 16, 23, 42};
		List<Integer> myList = new LinkedList<Integer>(Arrays.asList(myArray));
		
		System.out.println(Arrays.toString(myArray));           // [4, 8, 15, 16, 23, 42]
	}
}
```

#### List to Array
```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		Integer[] myArray = new Integer[]{4, 8, 15, 16, 23, 42};
		List<Integer> myList = new LinkedList<Integer>(Arrays.asList(myArray));
		
		// Create an array of the same size as the list
		Integer[] myArray2 = new Integer[myList.size()];

		// Convert the list to the array
		myList.toArray(myArray2);
		
		System.out.println(Arrays.toString(myArray2));
	}
}
```


## Bad Practises
1. Modifying a list you are iterating/looping over:

   ```java
	import java.util.*;

	public class MyClass {
		public static void main(String[] args) {
			List<Integer> xs = new LinkedList<Integer>(Arrays.asList(4, 8, 15, 16, 23, 42));
		
			for (Integer x: xs) {
				if (x % 2 == 0)
					xs.add(x * 2);
			}
		
			System.out.println(Arrays.toString(xs.toArray()));
		}
	}
   ```
   
   If you need to modify the list you are iterating on, consider cloning it first and modifying the clone instead:
   
   ```java
	import java.util.Arrays;
	import java.util.LinkedList;

	public class MyClass {
		public static void main(String[] args) {
			LinkedList<Integer> xs = new LinkedList<Integer>(Arrays.asList(4, 8, 15, 16, 23, 42));
			LinkedList<Integer> xsClone = (LinkedList<Integer>) xs.clone();
		
			for (Integer x: xs) {
				if (x % 2 == 0)
					xsClone.add(x * 2);
			}
		
			System.out.println(Arrays.toString(xs.toArray()));       // [4, 8, 15, 16, 23, 42]
			System.out.println(Arrays.toString(xsClone.toArray()));  // [4, 8, 15, 16, 23, 42, 8, 16, 32, 84]
		}
	}
   ```

## Good Practises

1. Check for extraordinary situations (`null` values, integers out of bounds, etc.) **at the very beginning** of your function, and return an error (usually `null`, or `false`) early.

   ```java
   public class Video {
	public boolean addRating(int rating) {
		if (!(1 <= rating && rating <= 5)) {
			System.out.printf("%d should be between 1 and 5.\n", rating);
			return false;
		}
		
		...
	}
   ```
2. It is easier to validate than to invalidate, in other words, instead of checking whether something is wrong, check if it it **not right**.

   For instance, in the previous example (1), we check if the `rating` is NOT in the valid range [1, 5], rather than checking if it's less than 1 and greater than 5.

3. Do NOT write `else` statement after an `if` statement with return.

   Since the execution of the function stops after `return`, there is no need to make things more complicated with adding an `else` statement to the chain.

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

