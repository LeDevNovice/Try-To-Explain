# Try to explain... Why array index starts to 0 and not to 1 ?
To understand why JavaScript array indexes start at 0, it is essential to look at other programming languages, especially C and its origins, which was designed in the 1970s by Dennis Ritchie. JavaScript, like many other languages, was influenced by C in its syntax but also in its conventions and operation.

Arrays are closely related to memory management. An array is nothing more than a continuous sequence of cells in memory. The index of an array is used to access a specific memory address, allowing the value stored at that location to be retrieved or modified.

Think of a computer's memory as a series of small boxes lined up, each of which can hold a piece of data. I currently have no knowledge of C. But it seems that when you declare an array in C, you get a pointer that points to the address of the first of these boxes (i.e. the first cell in the array). This is where the concept of the 0 index comes into its own:

```
*(arr + i)
```

Here, `arr` represents the address of the first cell in the array, and `i` is the index of the desired element. When `i` is 0, the expression simply becomes `*(arr + 0)`, that is, `arr`, the starting address. This means that index 0 points directly to the first cell in the array. If the indexes started at 1, the calculation would require an additional adjustment at each access, such as `*(arr + (i - 1))`. While this calculation is possible, it would introduce unnecessary overhead and increased risks of errors in address calculations, especially in the days when computing resources were precious and limited.

JavaScript, while very different from C in its usage and abstraction, inherited this indexing convention for several reasons. First, JavaScript was designed to be familiar to developers coming from languages ​​like C, C++, and Java, where indexing starts at 0.