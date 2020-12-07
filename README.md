# Functions, Arrays, Pointers and Dynamic Memory Allocation in C Language.
-----------------------------------------------------------------------------------
## Functions
What is a function? A function is a group of statements that together perform a task.

To use a function you need to provide a **function definition** and you need to **call** that function.
If you're using a library(For example when you include 'stdio.h') many functions have been already defined there(For example: printf(); ), so you don't need to define them again.

We define functions outside of our 'main()' function. Note that this could be in a different file too, but in that case we'd have to include that file.
You can group functions into two categories; ones that return a value and ones that do not.
Functions that do not return a value are type **void** functions. Typically you use a **void** function to perform some sort of action.

### Defining functions
#### Void functions

The general way of defining a **void** function is the following:

*void functionName( parameterList )*

*{*

   *Statements*
 
*}*

**Let's define and look at a function that displays the word 'hello' a given number of times.**
```C
// We do not have a return value, so this is a **void** function.
void showHello(int n)
{
	for (int i = 0; i < n; i++)
	{
		printf("Hello\n");
	}
}
```
The 'int n' parameter means that our function expects an **int** value passed to when you call this function.

#### Now let's look at functions with a return value

The general way of defining a function that returns a value is the following:

*typeName functionName(parameterList)*

*{*

*Statements*

*return value;*
	
*}*

In the sample above *typeName* means that our function will return that specific type of value.
For example if the *typeName* is *int*, then our funtion will return an *int*.

Note that the return value cannot be an array; though everything else is possible for example: integers, floating-point numbers, pointers, structures.

**Let's look at a simple funtion that returns the sum of two integers.**
```C
// The function will return an 'int' type, and expects two integer parameters.
int sumIntegers(int a, int b)
{
	return a + b;
}
```



<br>
<br>


A function terminates after executing a return statement. 
So if a function has more than one return statement the function terminates after it executes the first return satement.

**Let's look at an example; This function compares two integers and returns the larger value.**
```C
// Again, returning an 'int' and expecting two integers as parameters.
int bigger(int a, int b)
{
	if (a > b)
		return a; // If 'a' is greater than 'b', the function terminates here and returns 'a'. Does not go to the next line.
	else
		return b;
}
```
<br>
<br>

### Calling Functions

Calling a function basically means "using" a function, which has been already defined.

**In the next code snippet we're going to call 'sumIntegers' and 'showHello' functions.**
```C
//Function definition
void showHello(int n)
{
	for (int i = 0; i < n; i++)
	{
		printf("Hello\n");
	}
}

//Function definition
int sumIntegers(int a, int b)
{
	return a + b;
}


int main()
{
	showHello(3); // Calling the 'showHello' function. This will print 'Hello' three times.
	sumIntegers(1, 2); // In this line the function returns an 'integer', more specifically '3' but we don't store it, or do anything with it so it has no use.

	int num = sumIntegers(2, 7); // Here 'sumIntegers' function is called with parameters '2' and '7', it returns '9' which is then stored in the 'num' variable.

	printf("%d\n", num); // Output: 9

	showHello(sumIntegers(1, 4)); // This line is valid too! 'sumIntegers' returns an 'int' (5), which is passed to 'showHello' as an argument and will print 'Hello' five times.

	return 0; // Here 'return 0' only means that our program executed successfully, you can kinda ignore this for now
}
```
-----------------------------------------------------------------------------------

## Arrays
What is an array? **An array is a data form that can hold several values, all of one type!**

To create an array, you use a declaration statement which needs to indicate three things:
- The type of value to be stored in each element(For example int, double, etc)
- The name of the array
- The number of elements in the array

The general form of declaring an array is the following:

*typeName arrayName[arraySize];*
```C
//This is an array of 12 ints.
int months[12];
```
'arraySize' which is the number of elements must be an integer constant or a constant value.
Important: 'arraySize' cannot be a variable whose value is set while the program is running!
**Which means the following code is absolutely wrong.** We'll learn how to work around this soon.

```C
int main()
{
	//THIS IS BAD
	
	int size;
	printf("Enter array size: ");
	scanf("%d", &size);

	int my_array[size]; // Error! The size cannot be set this way during running time!

	return 0; 
}
```
<br>
<br>

A useful thing is that you can access an array's elemets individually. The way to do this is to use an index(A number) in brackets.
Important: 
- Array numbering starts with 0. This means that the first element of the array *months[12]* is *months[0]* and the last is *months[11]*!
- The compiler doesn't check if you use a valid subscript. For example the compiler won't complain if you assign a value to *months[771]*, but it might cause problems when you run the program, so you need to pay attention to this!


**A few simple useful properties of arrays we can do:**
```C
	int my_array[10]; // Creates an array with 10 elements.
	my_array[0] = 3; // Assigning value to the first element.
	my_array[1] = 7; // Can do the same with the rest

	int second_array[10] = {6, 1, 7, 5, 9, 1, 2, 5, 2, 6}; // You can initialize an array when you create it too!
```
A very important thing is that you can't just simply copy an array's content into another one. You need to copy each element with a loop for example.
```C
my_array = second_array; // Error!
```
<br>
<br>

If you leave the brackets ( [] ) empty when you initialize an array, the compiler will count the elements for you.

In this case the main concern might be that you don't know how large the array is (Which is useful to know, when for example you wanna go through each element with a loop).
```C
int third_array[] = {1, 2, 3, 4, 7}; // The compiler makes 'third_array' an array of 5 elements.
```
In this case we can calculate the length of our array the following way:
```C
int array_length = sizeof(third_array) / sizeof(int);
```
Let's see why this works.
```C
printf("%d", sizeof(third_array)); // Output: 20, which means the size of 'third_array' is 20 bytes.
printf("%d", sizeof(int)); // Output: 4, which means an 'int' is 4 bytes in size.
```
We know that our array is 20 bytes in size, and one 'int' is 4 bytes, so if we divide the size of the array with the size of a type of an element, we get the amount of elements in an array. (Which is 5 in this case)

<br>

## Strings

What is a string? **A string is a series of characters stored in consecutive bytes of memory, terminated by a null character.** (This is important to remember)
This implies that you can store a string as an array of 'char', with each character kept in its own array element.

C language style strings have a special feature. The last character of every string must be the *null character*. It is written as \0 and it marks the end of a string.

Let's look at three examples:
```C
	char first[5] = {'f', 'i', 'r', 's', 't'}; // Not a string, just an array of characters because doesn't end with a '\0' !
	char second[7] = { 's', 'e', 'c', 'o', 'n', 'd', '\0' }; // This is a string!
	char third[5] = {'t', 'h', 'i', '\0', 'd'}; // What happens here?

	printf("%s", third); // printf goes until it reaches a null character, so the output will be "thi".
```
Creating string this way looks tedious, and luckily there's a better way to initialize a character array to a string.
We just need to use a quoted string(String with quotation marks), called a string constant as in the following:
```C
char first[12] = "Hello World"; // Important: "Hello World" is 11 characters, but we need an extra element for the '\0'!
char second[] = "Not hello"; // Let the compiler count
```
This way the null character is automatically included.

<br>

This brings up the important fact that the following two are not equal.
```C
char ch = 'S'; // Fine
char ch = "S"; // Error, illegal type mismatch (This is actually two characters, an 'S' and a '\0')
```
<br>
<br>

**String operations**

We've learned that you can't just simply copy an array to another with just an assignment operator(=), and this applies to strings as well but luckily, including '#include <string.h>' gives us many useful functions to work with.
Here you can see a list of functions that we can use with strings by including *string.h*

![alt text](https://i.gyazo.com/e34da6addadf110b1376745f8f26b28e.png)

*(Link: https://fresh2refresh.com/c-programming/c-function/string-h-library-functions/)*

-----------------------------------------------------------------------------------

## Structures

Let's say you wanna store information about candy bars. You might wanna store the name of the candy, the weight and price, etc.
You would like some sort of data form that can hold all these informations in one unit.
An array won't be enough, since it can only hold values of the same type, one array can't hold both doubles and ints.
This is where structures come in. **A structure can hold items of multiple data types!**

Creating a structure is a two way process.

First, you define a structure description that describes the different types of data that can be stored in a structure.
Then you can create structure variables that follow the description's plans.

To access a structure's member you use the membership operator(.) such as: 'variableName.member'.

**Let's see how this works in practice, let's create a structure and do some operations with it.**
```C
//This is the definition of the structure.
//This did not create a structure, think of this as a blueprint of what a variable of this type looks like.
struct Candies
{
	char name[50];
	int price;
	double weight;
};


int main()
{
	// Struct keyword is needed, 'Candies' is a type of variable (Almost like you'd say an int), and 'candy1' is the name of the variable.
	struct Candies candy1;

	strcpy(candy1.name, "Kit Kat"); // We've learned about string functions above.
	candy1.price = 200;
	candy1.weight = 17.5;


	printf("The candy's name is: %s\n", candy1.name); // Output: The candy's name is: Kit Kat
	printf("The candy's price is: $%d\n", candy1.price); // Output: The candy's price is: $200
	printf("The candy weighs %f grams.\n", candy1.weight); // Output: The candy weighs 17.500000 grams.
	
	//Also, just like with other types, you can create arrays of structures too!
	
	struct Candies sour_candies[20]; // This creates an array of 20 'Candies' type structures.
	sour_candies[0].price = 20; // Accessing an array element's member is just as easy as if it was a regular type.

	return 0; 
}
```

-----------------------------------------------------------------------------------

## Pointers

A program must keep track of three fundamental properties when it stores data.
- Where the information is stored
- What value is kept there
- What kind of information is stored

We've used a strategy to accomplish these before, by defining a simple variable.
The declaration statement provides the type and a symbolic name for the value. 
It also causes the program to allocate memory for the value and to keep track of the location internally.

Let's look at a second strategy now; This strategy is based on **pointers, which are variables that store addresses of values rather than the values themselves.**

Before we discuss pointers a bit deeper, let's talk about how to find addresses for ordinary variables. **You just apply the address operator '&' to a variable to get its location.**
For example if 'cats' is a variable, '&cats' is its address.
```C
	int cats = 7;
	printf("%d", cats); // Output: 7
	printf("%p", &cats); // Example output: 000000EA259DFE90

	// Note that we use "%d" to print integers and "%p" to display pointer address.
```
<br>
<br>

**A special type of variable — the pointer — holds the address of a value.**
This means that the name of the pointer represents the location. Applying the * operator yields the value at the location.

Let's look at an example with a few basic operations with pointers:
```C
int main()
{
	int water = 6; // Declaring a variable.
	int* p_water; // Declaring a pointer to an int. (When you declare a pointer you need to use the *)

	p_water = &water; // Assigning the address of 'water' to pointer. (Remember, "name of the pointer represents the location" so no * here)

	printf("%d\n", water); // Output: 6
	printf("%d\n", *p_water); // Output: 6 | ("Applying the * operator yields the value at the location.")

	printf("%p\n", &water); // Example output: 000000E78D4FFA10 | ("You just apply the address operator '&' to a regular variable to get its location.")
	printf("%p\n", p_water); // Example output: 000000E78D4FFA10 | ("the name of the pointer represents the location")

	*p_water = 4; // "the * operator yields the value at the location", so because we use *, we modify the value at a location.

	printf("%d\n", water); // Output: 4

	return 0; 
}
```

As you can see the int variable **water** and the pointer variable **p_water** are just two sides of the same coin.<br>
The variable **water** represents the value as primary and uses the & operator to get the address.<br>
On the other hand **p_water** represents the address as primary and uses the * operator to get the value.
<br>
Because **p_water** points to **water**, **\*p_water** and **water** are completely equivalent(Note the addresses in the example program above)!<br>
This means that you can use **\*p_water** exactly as you would use a type int variable.<br>

<br>

**Caution with pointers**<br>
Let's look at the following code and see why it's wrong:
```C
	int* fish; // Creating a pointer to an int
	*fish = 12; // Where would we place this value?
```
Sure, 'fish' is a pointer. But where does it point? So where is the value '12' placed? We can't say.

<br>

***"Pointer Golden Rule: Always initialize a pointer to a definite and appropriate address before you apple the * operator to it!***

-----------------------------------------------------------------------------------

## Dynamic memory allocation

The true worth of pointers comes into play when you allocate 'unnamed' memory during runtime to hold values. In this case pointers become the only way to access to that memory.<br>
We'll be using a function named *malloc()* which is defined in 'stdlib.h' to allocate memory during our program's run time.<br>
*malloc()* allocates the requested memory and returns a pointer to it. Let's look at an example.<br>
```C
int* p1 = (int*)malloc(sizeof(int));
```
In the code above we create a pointer 'p1' that can hold an address.<br>
Then we call the function 'malloc()' which expects a parameter; how many bytes to allocate. (In this case we tell it to allocate 'sizeof(int)' bytes of memory which is usually 4 bytes.)<br>
The return type of malloc is 'void*' so we need to "cast" it to the type we want to work with, which is '(int*)' in this example. (This step might not needed, depending on compiler.)<br>

Once these are done, *malloc()* returns the pointer to the beginning of newly allocated memory, which is then assigned to 'p1'.<br>
**Then we can use the allocated memory like we did before with pointers, for example:**
```C
int main()
{
	int* p1 = (int*)malloc(sizeof(int)); // Allocating 'sizeof(int)' bytes of memory.

	*p1 = 7; // Assigning the value '7' to that address.

	printf("%d\n", *p1); // Output: 7
	printf("%p\n", p1); // Example Output: 00000255FEF08310

	free(p1); // This line is very important, we'll discuss it in a bit.

	return 0; 
}
```
<br>
<br>

Allocating memory with *malloc()* is just half of the fun of memory management. The other half is deallocating that memory when we no longer need it.<br>
We use the *free()* function to deallocate memory which has been previously allocated by *malloc()*. This frees the memory which 'p1' points to; doesn't remove the pointer itself, so you could possibly reuse 'p1' later if needed.<br><br>
A **very important rule** is that you should always balance the usage of *malloc()* and *free()*, otherwise you could end up with a memory leak — that is, memory that has been allocated but can no longer be used.<br>
**Also important** that you can't use *free()* to to free memory created by declaring ordinary variables(That is handled automatically when the program exits).<br>
```C
int* pt = (int*)malloc(sizeof(int)); // Ok
free(pt); // Ok
free(pt); // Not ok, already been deallocated.

int apples = 5; // Ok
int* ap = &apples; // Ok, the address of 'apples' is assigned to 'ap'
free(ap); // Not allowed, memory is not allocated by 'malloc()'
```

-----------------------------------------------------------------------------------

## Using calloc() to create dynamic arrays

Typically you use dynamic memory allocation with larger chunks of data, such as arrays, strings, etc.<br>
Suppose, for example you're writing a program that might or might not need an array, depending on information given to the program while running.<br>
If you create an array by declaring it, the space is allocated when the program is compiled. Whether or not the program finally uses the array, the array is still there, using up memory.<br>
But with *calloc()* you can create an array during runtime if you need it, or skip creating it if you don't need it. Or you can even specify array size while the program is running.<br>
*Side note: malloc() and calloc() work in a similar way, but the difference is that calloc() initializes the allocated memory block to 0, so it's easier for us to use.*

<br>

Creating a dynamic array is quite simple; *calloc()* expects two arguments: how many objects to create and the size of each object. So to create a dynamic array that can hold 10 ints we can do the following:
```C
int* pt = (int*)calloc(10, sizeof(int)); // Requesting 10 objects with the size of an int each. Note that calloc() returns with a pointer to the beginning of newly allocated memory. So 'pt' will hold the address to the first element.
```
To free a dynamically created array, you just apply 'free()' to the pointer that holds the address to the first element. (So in this case just free(pt); ).

To access a dynamically created array's elements you can just do it as if it was a regular array, like:
```C
	int* pt = (int*)calloc(10, sizeof(int));

	pt[0] = 2; // Assigning 2 to the first element.

	printf("%d\n", pt[0]); // Output: 2
	printf("%d\n", pt[1]); // Output: 0

	free(pt); // Don't forget about freeing allocated memory once we don't need it anymore!
```
	

<br>
<br>

While we are here, let's look at something interesting about pointers and arrays. If you add one to an integer variable, it increases its value by one, BUT **adding one to a pointer variable increases its value by the number of bytes of the type to which it points.** Now this might sound a bit confusing but the upcoming examples might help understand it:
```C
	int arr[3] = { 4, 8, 11 };

	int* arpt = arr; // Why does this work? This is valid because in C, the name of an array is the address of the first element. (Has the address of the first element of our array)
	int* arpt2 = &arr[0]; // This means the same, but expressed differently. (Has the address of the first element of our array)

	printf("%d\n", *arpt); // Output: 4 | The first element of the array.
	printf("%d\n", *(arpt+1)); // Output: 8 | The second element of the array. Why?
```

Let's carefully look at the following: *(arpt+1)<br><br>
We take 'arpt' which is currently pointing to the first element of our array and we're adding one to it. Remember, that "adding one to a pointer variable increases its value by the number of bytes of the type to which it points".<br><br>
In this case adding one to it means that it points to 4 bytes further in the memory(And we know that array elements are stored in consecutive bytes of memory) so basically it points to the second element.<br><br>
The reason why we do: *(arpt+1) instead of *arpt+1 is because they mean completely different things(The expression in parentheses are evaluated first).<br><br>
*The first one means:* "point 4 bytes further in the memory and get the value stored there"<br>
*And the second one means:* "Get the value stored in the current location and add one to that value"<br>

-----------------------------------------------------------------------------------

## Pointers and strings

Remember, that  the name of an array(Such as a string, which is an array of chars) is the address to its first element, so the following two printf() behave the same way.<br>
```C
	char ar[10] = "asd";

	printf("%s", ar); // Output: asd
	printf("%s", &ar[0]); // Output: asd
```

With string you might have encountered something like the following:<br>
```C
const char* str = "hello world";
```
Here '*str' is a pointer to 'char' that holds the address of "hello world".<br>

In the following code you tell printf() to display 'str' (Which holds the address of the first character of the string "hello world").<br>
Then printf() "walks" in the memory from that address and prints out everything until it reaches the string ending '\0' (Which is automatically there because we used "").<br>
```C
printf("%s", str); // Output: hello world
```
We can use string functions we've learned before with 'str', like:<br>
```C
printf("%d\n", strlen(str)); // Prints out 11. (Does not include the string ending '\0')
```
To get a bit more comfortable with dynamic memory allocation and string operations, let's make a small program that copies a string to newly allocated memory:<br>
```C
	const char* str = "hello doomy"; // This is our string; Again, *str is a pointer that holds the address of the first character of our string.

	char* str2 = (char*)calloc(strlen(str) + 1, sizeof(char)); // We request enough memory that can hold our string.

	strcpy(str2, str); // Then we copy "hello doomy" to the memory we just allocated.

	printf("%s\n", str2); // Prints: hello doomy

	free(str2); // Finally the mandatory memory deallocation.
```
**Note:** When we allocate memory we say "strlen(str) + 1", this is because strlen() returns the amount of characters in a string, not including the string ending '\0', so we need some extra space.

<br>

While we're here, let's practice a bit more with dynamic memory allocation and pointers.<br>
Let's make a program that asks the user how large of an array they want to create, and ask them to fill it up with numbers of their choice.<br>
```C
int main()
{
	int size = 0;

	printf("Enter the size of the array: ");
	scanf("%d", &size); // Asking the user the enter a number.

	int* spt = (int*)calloc(size, sizeof(int)); // Allocating 'size' blocks of memory that are 'sizeof(int)' large each, and assigning the address of the first block to 'spt'.

	for (int i = 0; i < size; i++) // Creating a loop that will repeat 'size' times.
	{
		printf("\nEnter the value of array element #%d: ", i + 1);
		scanf("%d", spt + i); // Getting data from the user to each dynamically created array element. 'spt' is the base address and with each loop iteration we go to the next element.
	}

	printf("The array has %d elements and the elements are the following: ", size);

	for (int i = 0; i < size; i++) // Printing out the contents of the newly allocated array.
	{
		printf("%d, ", *(spt + i));
	}

	free(spt); // The mandatory free() after allocating memory.

	return 0; 
}
```




























