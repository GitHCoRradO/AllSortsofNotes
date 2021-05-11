### Java language problems

#### What does ```final``` modifier do in method arguements, in method body, in class level, in for loop?
+ sample code:
	```
	public class SomeClassImpl {
		// class level
		private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(SomeClassImpl.class);
		// method arguement
		public static void someClassImplMethod1(final String arg1, int arg2) {
			// method body
			final Set<String> setOfStrings;
			/*
				code initializing setOfStrings
			*/
			// for loop
			for (final singleStr: setOfStrings) {
				/*
					code processing singleStr s
				*/
			}
		}
	}
	```
+ the ```final``` keyword in Java [reference](https://www.geeksforgeeks.org/final-keyword-java/)
+ all in all, ```final``` keyword means cannot be changed for variables or cannot be inherited for classes
	+ variable
		+ When a variable is declared with ```final``` keyword, its value can’t be modified, essentially, a constant. 
		+ We must initialize a ```final``` variable, otherwise compiler will throw compile-time error. There are 3 ways to initialize a ```final``` variable:
			+ initialize a final variable when it is declared.
			+ A final variable is called blank final variable,if it is not initialized while declaration.
				+ A blank final variable can be initialized inside instance-initializer block or inside constructor.
				+ A blank final static variable can be initialized inside static block.
		+ code sample
		```
		//Java program to demonstrate different
		// ways of initializing a final variable
		class GFG
		{
			// a final variable
			// direct initialize
			final int THRESHOLD = 5;
			
			// a blank final variable
			final int CAPACITY;
			
			// another blank final variable
			final int MINIMUM;
			
			// a final static variable PI
			// direct initialize
			static final double PI = 3.141592653589793;
			
			// a blank final static variable
			static final double EULERCONSTANT;
			
			// instance initializer block for
			// initializing CAPACITY
			{
				CAPACITY = 25;
			}
			
			// static initializer block for
			// initializing EULERCONSTANT
			static{
				EULERCONSTANT = 2.3;
			}
			
			// constructor for initializing MINIMUM
			// Note that if there are more than one
			// constructor, you must initialize MINIMUM
			// in them also
			public GFG()
			{
				MINIMUM = -1;
			}
		}
		```
		+ the internal state of reference final variable, that is a reference to an object, can be changed
			+ code sample
			```
			// Java program to demonstrate 
			// reference final variable
			class Gfg
			{
			    public static void main(String[] args) 
			    {
			        // a final reference variable sb
			        final StringBuilder sb = new StringBuilder("Geeks");
			          
			        System.out.println(sb);
			          
			        // changing internal state of object
			        // reference by final reference variable sb
			        sb.append("ForGeeks");
			          
			        System.out.println(sb);
			    }    
			}
			```
		+ When a final variable is created inside a method/constructor/block, it is called local final variable, and it must initialize once where it is created.
			+ code sample:
			```
			// Java program to demonstrate
			// local final variable
			// The following program compiles and runs fine
			class Gfg
			{
			    public static void main(String args[])
			    {
			        // local final variable
			        final int i;
			        i = 20; 
			        System.out.println(i);
			    }
			}
			```
		+ A final variable can be assigned value later, but only once.
	+ ```final``` with foreach loop : final with for-each statement is a legal statement.
		+ Explanation : Since the ```i``` variable goes out of scope with each iteration of the loop, it is actually re-declaration each iteration, allowing the same token (i.e. i) to be used to represent multiple variables.
	+ When a class is declared with final keyword, it is called a final class. A final class cannot be extended(inherited). There are 2 usages
		+ One is definitely to prevent inheritance, as final classes cannot be extended.
		+ The other use of final with classes is to create an immutable class like the predefined String class.
	+ When a method is declared with final ```keyword```, it is called a final method. A final method cannot be overridden.
		+ We must declare methods with final keyword for which we required to follow the same implementation throughout all the derived classes.
+ here is an explanation to the first sample code:
	```
	public class SomeClassImpl {
		// log variable is initialized when declared. it cannot be changed later
		private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(SomeClassImpl.class);
		// method arguement
		// arg1 variable cannot be changed inside this methods body
		public static void someClassImplMethod1(final String arg1, int arg2) {
			// setOfStrings is declared but assigned a value later; but it can only be assigned once, and cannot be changed after assignment.
			final Set<String> setOfStrings;
			/*
				code assigning setOfStrings; once and for all.
			*/
			// foreach loop
			// each singleStr is initialized when before executing the for code block
			// each singleStr cannot be changed inside the for block
			for (final singleStr: setOfStrings) {
				/*
					code processing singleStr s; but not changing its value
				*/
			}
		}
	}
	```

#### ```StringBuilder.setLength``` usage
+ ```setLength(0)``` clears the builder [SO reference](https://stackoverflow.com/questions/2242471/clearing-a-string-buffer-builder-after-loop)
+ [Java doc description](https://docs.oracle.com/javase/6/docs/api/java/lang/StringBuilder.html#setLength(int))
	+ ```sb.setLength(newLength)```
	+ Sets the length of the character sequence. The sequence is changed to a new character sequence whose length is specified by the argument. For every nonnegative index k less than newLength, the character at index k in the new character sequence is the same as the character at index k in the old sequence if k is less than the length of the old character sequence; otherwise, it is the null character '\u0000'. In other words, if the newLength argument is less than the current length, the length is changed to the specified length.
	+ If the newLength argument is greater than or equal to the current length, sufficient null characters ('\u0000') are appended so that length becomes the newLength argument.
	+ The newLength argument must be greater than or equal to 0.
	+ in other words, if newLength is less than oldLength, trailing chars of oldStringBuilder will be truncated; if newLength is greater than oldLength, newly appended spaces will be filled with null character.
+ test code snippet:
	```
	public class StringBuilderTest {
	    public static void main(String[] args) {
	        StringBuilder sb = new StringBuilder("Abcdefg");
	        sb.setLength(3);
	        String str1 = sb.toString();
	        // prints:
	        // Abc length:3
	        System.out.println(str1 + " length:" + str1.length());
	        sb.setLength(7);
	        String str2 = sb.toString();
	        // prints:
	        // Abc length:7
	        System.out.println(str2 + " length:" + str2.length());
	    }
	}
	```
+ test StringBuilder with null character:
	```
	public class StringBuilderTest {
	    public static void main(String[] args) {
	        StringBuilder sb = new StringBuilder("Abcdefg");
	        sb.setLength(0);
	        sb.setLength(5);
	        String strBuilder = sb.toString();
	        /* prints:
			strBuilder:||
			strBuilder length: 5
	        */
	        System.out.println("strBuilder:|" + strBuilder + "|");
	        System.out.println("strBuilder length: " + strBuilder.length());
	        sb.setCharAt(4, 'b');
	        String strBuilder1 = sb.toString();
	        /* prints:
			strBuilder:|b|
			strBuilder length: 5
	        */
	        System.out.println("strBuilder:|" + strBuilder1 + "|");
	        System.out.println("strBuilder length: " + strBuilder1.length());
	        String nullStr = "";
	        /* prints:
			strBuilder:||
			strBuilder length: 0
	        */
	        System.out.println("nullStr:|" + nullStr + "|");
	        System.out.println("nullStr length: " + nullStr.length());
	    }
	}
	```

#### ```ZonedDateTime.parse(s"2020-04-08T03:30:56.953967+00:00").format(DateTimeFormatter.ofPattern("YYYY-mm-DD HH:mm:SS").withZone(ZoneId.systemDefault))``` prints ```res6: String = "2020-30-99 11:30:95"```

#### Java ```ExecutorService```
+ [reference](https://www.baeldung.com/java-executor-service-tutorial)

#### Java ```Runnable``` vs ```Callable```
+ [reference](https://www.baeldung.com/java-runnable-callable)

#### Java interface ```Function<T1, R>```

#### Java lambda expression