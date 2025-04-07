## Question:
The procedure BinarySearch(numList, target) correctly implements binary search on numList.
Which of the following conditions must be met in order for the procedure to work as intended? Explain why.

Choices:

a) The length of numList must be even
b) The list numList must not contain any duplicate values
c) The values in numList must be in sorted order ✅
d) The value of target must not be equal to -1

Correct Answer: c
Explanation:
Binary Search requires the list to be sorted in order to work properly. It assumes the middle value can help decide which half of the list to search next. If the list is unsorted, the search can go in the wrong direction and miss the target.

## Question 2:
Which of the following statements correctly describes a disadvantage of binary search compared to linear search? Explain why your answer is correct and why the others are wrong.

Choices:

a) Binary search takes more time on average than linear search
b) Binary search cannot be used on unsorted lists without modifications ✅
c) Binary search always returns the first occurrence of the target
d) Binary search can only be used on lists with unique values

✔️ Correct Answer: b
Explanation:
Binary search only works on sorted lists. If the data isn't sorted, it must be sorted first (which takes time).

Popcorn Hack #3
```
def binary_search_chars(arr, target):
    low, high = 0, len(arr) - 1

    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1

    return -1

# Example usage:
chars = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
print(binary_search_chars(chars, 'c'))  # Output: 2
```

## Homework Hack
```
import pandas as pd

# Load dataset
data = pd.read_csv("school_supplies.csv")

# Clean data
data_cleaned = data.dropna()

# Sort by 'Price'
data_sorted = data_cleaned.sort_values(by="Price")

# Extract prices as a sorted list
price_list = data_sorted["Price"].tolist()

# Preview data
print("First few rows of sorted data:")
print(data_sorted.head())
print("Original row count:", len(data))
print("Cleaned row count:", len(data_cleaned))

# Binary Search Function
def binary_search(arr, target):
    low, high = 0, len(arr) - 1

    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1

    return -1

# Prices to search for
prices_to_check = [1.25, 6.49, 10.00]

# Run search
for price in prices_to_check:
    result = binary_search(price_list, price)
    if result != -1:
        print(f"✅ Price ${price} found at index {result}.")
    else:
        print(f"❌ Price ${price} not found in the list.")
```
Explanation: 
- Loads CSV data using Pandas.

- Cleans the data by dropping rows with missing values.

- Sorts the prices .

- Uses a binary search to look for specific prices.

- Efficient for large datasets because it narrows the search space logarithmically.

