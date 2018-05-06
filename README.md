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
		List<Integer> xs = new ArrayList<Integer>(Arrays.asList(4, 8, 15, 16, 23, 42));
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

`List<ElementType>` is the abstract type for lists, `ArrayList<ElementType>` and `LinkedList<ElementType>` are its (concrete) implementations. Prefer `ArrayList` over `LinkedList` if you are unsure.

### Converting Arrays and Lists to Each Other

#### Array to List
```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		Integer[] myArray = new Integer[]{4, 8, 15, 16, 23, 42};
		List<Integer> myList = new ArrayList<Integer>(Arrays.asList(myArray));

		System.out.println(Arrays.toString(myArray));  // [4, 8, 15, 16, 23, 42]
	}
}
```

#### List to Array
```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		Integer[] myArray = new Integer[]{4, 8, 15, 16, 23, 42};
		List<Integer> myList = new ArrayList<Integer>(Arrays.asList(myArray));

		// Create an array of the same size as the list
		Integer[] myArray2 = new Integer[myList.size()];

		// Convert the list to the array
		myList.toArray(myArray2);

		System.out.println(Arrays.toString(myArray2));  // [4, 8, 15, 16, 23, 42]
	}
}
```

### Sorting
#### Arrays
```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		Integer[] xs = new Integer[]{8, 4, 16, 15, 42, 23};
		System.out.println(Arrays.toString(xs));  // [8, 4, 16, 15, 42, 23]
		Arrays.sort(xs);
		System.out.println(Arrays.toString(xs));  // [4, 8, 15, 16, 23, 42]
	}
}
```

#### Lists
```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		List<Integer> xs = new ArrayList<Integer>(Arrays.asList(8, 4, 16, 15, 42, 23));
		System.out.println(Arrays.toString(xs.toArray()));  // [8, 4, 16, 15, 42, 23]
		Collections.sort(xs);
		System.out.println(Arrays.toString(xs.toArray()));  // [4, 8, 15, 16, 23, 42]
	}
}
```

### Reversing
#### Arrays
```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		int[] xs = new int[]{4, 8, 15, 16, 23, 42};
		System.out.println(Arrays.toString(xs));  // [8, 4, 16, 15, 42, 23]
		reverseArray(xs);
		System.out.println(Arrays.toString(xs));  // [4, 8, 15, 16, 23, 42]
	}

	// Source: https://stackoverflow.com/a/3523066/4466589
	public static void reverseArray(int[] arr) {
	    int left = 0;
	    int right = arr.length - 1;

	    while (left < right) {
	        // Swap the values at the left and right indices
	        int temp   = arr[left];
	        arr[left]  = arr[right];
	        arr[right] = temp;

	        // Move the left and right index pointers in toward the center
	        left++;
	        right--;
	    }
	}
}
```

#### Lists
```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		List<Integer> xs = new ArrayList<Integer>(Arrays.asList(4, 8, 15, 16, 23, 42));
		System.out.println(Arrays.toString(xs.toArray()));  // [4, 8, 15, 16, 23, 42]
		Collections.reverse(xs);
		System.out.println(Arrays.toString(xs.toArray()));  // [42, 23, 16, 15, 8, 4]
	}
}
```

## Maps
Maps are objects which *map* each key to a single value.

`Map<KeyType, ValueType>` is the abstract type, `HashMap<KeyType, ValueType>` is its (concrete) implementation.

Beware that `HashMap` allows null keys and null values, but it is strongly advised that you do not use null keys and values.

### Basic Usage
```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		Map <String, Integer> studentIDs = new HashMap<String, Integer>();
		studentIDs.put("robyn", 12345);
		studentIDs.put("bora",  67890);
		System.out.println(studentIDs.get("bora"));  // 67890
	}
}
```

### Usage as a Counter
One common usage pattern of maps is to implement counters.

```java
import java.util.*;

public class MyClass {
	public static void main(String[] args) {
		String[] xs = {"foo", "bar", "foo", "qux", "baz", "qux", "foo"};
		Map <String, Integer> counter = new HashMap<String, Integer>();

		for (String x: xs) {
			// The following line sets `count` to the value `arg` is mapped to
			// if exists, else to zero.
			int count = counter.getOrDefault(x, 0);
			// Update the count of `arg`
			counter.put(x, count + 1);
		}

		System.out.println(counter);  // {bar=1, qux=2, foo=3, baz=1}

		String mp = mostPopular(counter);
		System.out.printf("%s -> %d\n", mp, counter.get(mp));  // foo -> 3

	}

	// Returns the first most popular (i.e. key that maps to the greatest
	// integer) in a given counter.
	// If counter is empty, returns null.
	public static String mostPopular(Map<String, Integer> counter) {
		if (counter == null || counter.size() == 0)
			return null;

		String maxKey = null;

		for (String key: counter.keySet())
			if (counter.get(key) > counter.get(maxKey) || maxKey == null)
				maxKey = key;

		return maxKey;
	}
}
```

## Hints

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

4. Use `this` to while using instance variables and methods. Being more explicit will prevent errors such that accessing a static variable (which is shared amongst **all** instances of the class), or help you remind that you are in a static method.

5. Do NOT modify a list or map you are iterating/looping over:

   ```java
	import java.util.*;

	public class MyClass {
		public static void main(String[] args) {
			List<Integer> xs = new ArrayList<Integer>(Arrays.asList(4, 8, 15, 16, 23, 42));

			// WRONG!
			for (Integer x: xs) {
				if (x % 2 == 0)
					xs.add(x * 2);
			}

			System.out.println(Arrays.toString(xs.toArray()));
		}
	}
   ```

   If you need to modify the object you are iterating on, consider cloning it first and modifying the clone instead:

   ```java
	import java.util.Arrays;
	import java.util.LinkedList;

	public class MyClass {
		public static void main(String[] args) {
			List<Integer> xs = new ArrayList<Integer>(Arrays.asList(4, 8, 15, 16, 23, 42));
			List<Integer> xsClone = (ArrayList<Integer>) xs.clone();

			for (Integer x: xs) {
				if (x % 2 == 0)
					xsClone.add(x * 2);
			}

			System.out.println(Arrays.toString(xs.toArray()));       // [4, 8, 15, 16, 23, 42]
			System.out.println(Arrays.toString(xsClone.toArray()));  // [4, 8, 15, 16, 23, 42, 8, 16, 32, 84]
		}
	}
   ```

6. Do NOT use `++` operator (*i.e. `variable++` or `++variable`), use `variable += 1` instead, except in the first line of for loops; same goes with `--` operator.



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

