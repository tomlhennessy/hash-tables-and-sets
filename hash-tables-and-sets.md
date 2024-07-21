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
