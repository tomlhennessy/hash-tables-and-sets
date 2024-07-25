# Hash Functions

* Definition
A hash function maps arbitrary data to a deterministic, fixed-size output. In simpler terms, it takes an input, processes it through deterministic steps, and returns a unique, scrambled output. Given the same input, the output will always be the same.

* Key Characteristics
    • Deterministic: Same input always results in the same output
    • One-way function: Impossible to reverse the process to retreive the original input
    • Fixed-size output: Regardless of input size, the output size is fixed


* Simple Hash Function Example
    A basic function in JavaScript that converts a string to an integer by summing ASCII values

    ```js
    function simpleHash(str) {
        let hashValue = 0;
        for (let i = 0; i < str.length; i++) {
            hashValue += str.charCodeAt(i);
        }
        return hashValue;
    }
    ```


* Characteristics

    • Same input results in same output:
    ```js
    simpleHash("Hello, world!"); // 1161
    simpleHash("Hello, world!"); // 1161
    ```

    • Different inputs generally produce different outputs:
    ```js
    simpleHash("ABC");          // 198
    simpleHash("abc");          // 294
    ```

    • Different inputs can sometimes result in the same output:
    ```js
    simpleHash("ABCDEF");       // 405
    simpleHash("ABBEEF");       // 405
    ```


* SHA256 Hashing
For cryptographic purposes, more complex algorithms like SHA256 are used. SHA256 (Secure Hash Algorithm 256-bit) is both fast and secure

* Example in JavaScript
```js
const sha256 = require('js-sha256');
sha256("Hello, world!");
// '315f5bdb76d078c43b8ac0064e4a0164612b1fce77c869345bfc94c75894edd3'
```

* Characteristics
    • Produces a 256-bit output, represented by 64 hexadecimal characters
    • Outputs are varied and seemingly random, even for similar inputs:
    ```js
    sha256("ABC");
    // 'b5d4045c3f466fa91fe2cc6abe79232a1a57cdf104f7a26e716e0a1e2789df78'

    sha256("ABCDEF");
    // 'e9c0f8b575cbfcb42ab3b78ecc87efa3b011d9a5d10b09fa4e96f240bf6a82f5'
    ```

* Practical Applications
    • Security: Password hashing, data integrity verification
    • Data Indexing: Efficient data retreival in hash tables

* Conclusion
Hash functions are critical in computing for their ability to create unique, fixed-size outputs from arbritrary input data. Understanding their deterministic nature and one-way functionality is essential for applications in security and efficient data management.



# Hash Tables

* Definition
A hash table (or hash map) is a data structure that provides key/value storage with constant time complexity (O(1)) for insertion, deletion, access and search operations

* Key Characteristics
    • Key/value storage: stores data in key/value pairs
    • Efficient Operations: constant time complexity for insertion, deletion, access, and search
    • Underlying Structure: utilises an array where elements are indexed by hashed keys


* Components of a Hash Table

    1. Array of Buckets
        • A fixed-size array where each slot (bucket) is initialised to null
        • Example initialisation
        ```js
        let data = [null, null, null, null, null, null, null, null];
        ```

    2. Hash Function
        • Converts keys to integers
        • Example
        ```js
        function hash(str) {
            let hashValue = 0;
            for (let i = 0; i < str.length; i++) {
                hashValue += str.charCodeAt(i);
            }
            return hashValue;
        }
        ```

    3. Hash Mod Function
        • Converts the key hash into a valid array index using the modulo operator
        • Example:
        ```js
        function hashMod(key) {
            return hash(key) % data.length;
        }
        ```

        • Example Usage:
        ```js
        hashMod("Key"); // returns 1
        hashMod("new key"); // returns 3
        ```

* Inserting into a Hash Table

    1. Hash and Modulo Key: calculate the array index
    ```js
    let index = hashMod("key"); // Example index: 1
    ```

    2. Create Key/value pair: Store the pair in the calculated index
    ```js
    class KeyValuePair {
        constructor(key, value) {
            this.key = key;
            this.value = value;
        }
    }
    data[index] = new keyValuePair("key", "value");
    ```

    • Example State of Hash Table
    ```js
    HashTable {
        data: [
            null,
            KeyValuePair { key: 'key', value: 'value' },
            null,
            KeyValuePair { key: 'new key', value: 'new value' },
            null,
            KeyValuePair { key: 'App Academy', value: 'Computer Science' },
            null,
            KeyValuePair { key: 'She sells seashells by the seashore...', value: 'Sally Seashell' }
        ]
    }
    ```

* Hash Collisions
    • Occur when two keys hash to the same bucket
    • Resolved using methods such as chaining or open addressing (discussed in further readings)

* Performance Analysis

    Time Complexity
        • Insertion/deletion/access/search: O(1)
        • Hash Function: O(n) (where n is the length of the key, typically negligable for small keys)

    Space Complexity
        • Storage: O(n): where n is the number of key/value pairs
        • Memory Overhead: higher than the arrays due to empty buckets and storing both keys and pairs

* Summary
    • Hash tables combine hash functions with an array to provide efficient key/value storage
    • Operations like insertion, deletion, access and search are performed in constant time
    • Hash tables are foundational for many programming applications including JavaScript objects and sets


# Hash Table Collisions

* What is a Hash Collision?
A hash collision occurs when two different keys hash to the same bucket index in a hash table. This happens because the hash function and modulo operation may return the same index for different keys.

* Resolving Hash Collisions

    1. Linked List Chaining:
        • Concept: Each bucket in the hash table points to a linked list of key-value pairs
        • Implementation: Add a pointer to each key-value pair, linking them in a chain
        • Performance: Traversing the linked list is O(n) where n is the number of collisions in that bucket, but typically it's close to O(1) due to low collision probability.

    2. Array Resizing:
        • Concept: Increase the number of buckets to reduce the probability of collisions
        • Implementation: Double the array size and rehash all existing key-value pairs
        • Load Factor: Triggers resizing when the number of key-value pairs exceeds a threshold (commonly 0.7 times the number of buckets)
        • Performance: Resizing is O(n) where n is the number of elements in the hash table, but this operation is infrequent

* Example of Linked List Chaining
    ```js
    const ht = new HashTable();

    ht.insert("a", "apple");
    ht.insert("b", "banana");
    ht.insert("c", "candy");
    ht.insert("d", "durian");
    ht.insert("e", "egg");
    ht.insert("f", "fish");
    ht.insert("g", "garlic");
    ht.insert("h", "hamburger");
    ht.insert("i", "ice cream");  // Collision with "a"

    ht.data;
    // 0: ["h", "hamburger"] -> null
    // 1: ["a", "apple"] -> ["i", "ice cream"] -> null
    // 2: ["b", "banana"] -> null
    // 3: ["c", "candy"] -> null
    // 4: ["d", "durian"] -> null
    // 5: ["e", "egg"] -> null
    // 6: ["f", "fish"] -> null
    // 7: ["g", "garlic"] -> null
    ```

* Other Collision Resolution Methods
    1. Double Hashing: Using a second hash function to determine the step size for probing
    2. Open Addressing: Places collided elements in neighbouring buckets, then searches sequentially

* Performance Implications
    • Worst-case: O(n) if all keys hash to the same bucket
    • Average-case: O(1) due to low collision probability with a well-distributed hash function and appropriate bucket resizing

* Load Factor and Resizing
    • Load Factor: Ratio of the number of elements to the number of buckets
    • Resizing: Increases the number of buckets to maintain efficient operations, commonly done when the load factor exceeds 0.7

* Summary
    • Hash Collisions: occur when different keys map to the same bucket
    • Resolution: linked list chaining and array resizing are common techniques
    • Performance: generally efficient with O(1) operations, but can degrade to O(n) in worst-case scenarios
    • Load Factor: critical for determining when to resize the hash table to maintain performance



# Sets

* Definition
    • Set (Mathematics): A well-defined collection of distinct elements
    • Set (Computer Science): An abstract data type used to store a collection of unique, unordered values

* Properties of a Set
    1. No duplicates: a set contains no duplicate elements
    2. Unordered: The elements in a set are not stored in any particular order
    3. Constant Time Lookup: checking if an element is in a set is done in O(1) time

* When to Use a Set
    • Use a set when you need to store unique values without any specific order
    • the main advantage over arrays is the constant time lookup

* Comparing Arrays and Sets
    • Array: uses `.includes(x)` method with O(n) time complexity
    • Set: uses `.has(x)` method with O(1) time complexity

Performance Test Example
```js
let n = 100000;

// fill an array with integers
const arr = [];
for (let i = 0; i < n; i++) {
    arr.push(i);
}

// fill a set with integers
const set = new Set(arr);

// search the array
let startTimeArray = Date.now();
for (let i = 0; i < 2 * n; i++) {
    arr.includes(i);
}
let endTimeArray = Date.now();

// search the set
let startTimeSet = Date.now();
for (let i = 0; i < 2 * n; i++) {
    set.has(i);
}
let endTimeSet = Date.now();

console.log(`Array: ${endTimeArray - startTimeArray}ms`);
console.log(`Set: ${endTimeSet - startTimeSet}ms`);
```

* Performance Results
    • Array:
        n = 50000: 3630ms
        n = 100000: 15028ms

    • Set:
        n = 50000: 7ms
        n = 100000: 12ms


* Set Implementation
    • Abstract Data Type (ADT): implementation varies by language and use case
    • Typically Uses Hash Tables:
        • Keys only: stores keys without values
        • Underlying structure: an array with preallocated buckets
        • Hashing: values are hashed and stored in specific array indices
        • Collision Handling: manages collisions, worst-case O(n) for that bucket
        • Load Factor: when exceeding 0.7, a new larger array is allocated, values rehashed and copied

* Advantages of Using Sets
    • O(1) Search Functionality: achieved through hash tables
    • Uniqueness and Unordered: ensured by hash table properties
    • Space Complexity: O(n) with some overhead for empty buckets

* Summary
    • Key Properties: unique elements, unordered, O(1) lookup
    • Performance: Significant gains over arrays for large datasets
    • Implementation: Hash tables ensure efficiency and property adherance

* Practical Usage
    • Recognise situations where sets can optimise performance over arrays, especially in scenarios requiring frequent membership checks or ensuring unique elements
