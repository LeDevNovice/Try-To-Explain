# Try to explain... What is Binary Search and how to implement it ?
_Instinctively, whether we are looking for a word in a dictionary or a name in a contact directory, we don't start from the beginning or the first pages if we are searching for the word "Koala" or the name "Kevin." Instead, we open it somewhere around the middle to save time. Then, depending on where we are relative to what we are searching for, we repeat the process until we reach the element we are looking for._

_Binary search is an algorithm to check for the presence of an element in a sorted list, like a dictionary or a contact book. Instead of starting the search from the beginning, we start in the middle of the list, significantly reducing the number of necessary attempts._

---

Imagine a game where you need to guess a number between 1 and 100. A simple method would be to start at 1, then 2, and so on. But if the number to find is 100, it would take 100 tries. And if the game changes the range to 1-1 000 000, it could take up to a million tries.

This is called linear search, where each attempt eliminates only one number at each try.

A more efficient method would be to always choose the middle number. Each choice then eliminates half of the remaining possibilities, whether the range is 100 or 1,000,000 numbers. This process greatly reduces the number of tries needed to find the correct number.

**This is the principle of Binary Search.**

On a sorted sample of 100 numbers, the maximum number of tries to find an element is 7. For a list of 128 numbers, it takes at most 8 tries. And for a list of 256 numbers, only 9 tries are needed. Only one more try for a a double larger list. This is because binary search divides the list in half with each attempt.

Binary search halves the search space with each attempt, significantly reducing the number of tries needed to find an element. It is especially useful for large sorted lists.

> <h3>Binary search only works on sorted lists !</h3>

---

A binary search implementation function takes a sorted array and an element to search for as input. The principle is to keep track of the initial portion of the array, which is reduced with each attempt. Each time, we define the middle element and choose it as our next guess. If the guess is too low, all lower values are removed from the portion of the sorted array. If it is too high, all higher values are removed from the portion of the sorted array.

### Iterative Version
``` typescript
function binarySearch(list: number[], elementToSearch: number): number {
    let low = 0;
    let high = list.length - 1;

    while (low <= high) {
        const mid = Math.floor((low + high) / 2);

        if (list[mid] === elementToSearch) {
            return mid;
        } else if (list[mid] < elementToSearch) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    return -1; // Element not found
}
```

### Recursive Version
Binary search can also be implemented recursively. However, this approach can lead to stack overflow if the list is very large.
``` typescript
function binarySearchRecursive(list: number[], elementToSearch: number, low: number = 0, high: number = list.length - 1): number {
    if (low > high) {
        return -1;  // Element not found
    }

    const mid = Math.floor((low + high) / 2);

    if (list[mid] === elementToSearch) {
        return mid;
    } else if (list[mid] < elementToSearch) {
        return binarySearchRecursive(list, elementToSearch, mid + 1, high);
    } else {
        return binarySearchRecursive(list, elementToSearch, low, mid - 1);
    }
}

```
## Ressources : 
### Books
  - **Grokking Algorithms, Aditya Bhargava :** An illustrated explanation with diagrams and real-life examples.
### Vidéos
  - **[HackerRank](https://www.youtube.com/watch?v=P3YID7liBug) :** An animated explanation by Gayle Laakmann McDowell, author of Cracking the Coding Interview.

