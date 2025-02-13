### Classes and Methods
#### Define Class
```
class MyClass:
    def __init__(self, name, age):
        self.name = name  # Instance attribute
        self.age = age
```

#### Adding method
```
class MyClass:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        print(f"Hello, my name is {self.name} and I am {self.age} years old.")
```

## Example 2: Class EasyDict

Make an `EasyDict` class that can be used with both attribute and item syntax. This can be done by implementing `__getitem__` and `__setitem__` methods.

Example:
```python
>>> a = EasyDict()
>>> a['shoe'] = "blue"
>>> a.shoe
"blue"

>>> a['shoe']
"blue"
	
>>> a.car = "green"
>>> a['car']
"green"
```

Source Code:
```python
class EasyDict:
    def __init__(self):
        pass

   	def __getitem__(self, item):
        return self.__dict__[item]

    def __setitem__(self, key, value):
        self.__dict__[key] = value
```
### Time Complexity 

Big O time complexity is a way to describe how the runtime of an algorithm scales with the size of its input. It focuses on the worst-case scenario and ignores constants and lower-order terms to provide a high-level understanding of performance as input size grows.

#### Common Big O Notations:
1. **O(1) – Constant Time**
    - The runtime does not depend on the input size.
    - Example: Accessing an element in an array by index.
2. **O(log n) – Logarithmic Time**
    - The runtime grows logarithmically as the input size increases.
    - Example: Binary search.
3. **O(n) – Linear Time**
    - The runtime grows linearly with the input size.
    - Example: Iterating through a list.
4. **O(n log n) – Quasilinear Time**
    - The runtime grows proportionally to n×log⁡(n)n \times \log(n)n×log(n).
    - Example: Merge sort, quicksort (average case).
5. **O(n²) – Quadratic Time**
    - The runtime grows quadratically as the input size increases.
    - Example: Nested loops over the same dataset (e.g., bubble sort).
6. **O(2ⁿ) – Exponential Time**
    - The runtime doubles with each additional input.
    - Example: Solving the Traveling Salesman Problem with brute force.
7. **O(n!) – Factorial Time**
    - The runtime grows factorially with the input size.
    - Example: Generating all permutations of a set.

### How to Interpret Big O:
- **Focus on growth**: It shows how the algorithm’s runtime will increase as the input size grows.
- **Worst-case scenario**: Big O usually describes the maximum time an algorithm could take.
- **Drop constants and small terms**: Simplify to the term that dominates growth as input size increases.

### Practical Tips:

- **Prefer lower complexities (O(1), O(log n), O(n))** for scalability.
- **Avoid higher complexities (O(2ⁿ), O(n!))** unless input size is small or unavoidable.
- Analyze loops, recursion, and data structures to determine complexity.