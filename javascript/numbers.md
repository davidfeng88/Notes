# Numbers

## Convert Strings to Number

```javascript
parseInt('123', 10); // 10 is the base. In old browsers, strings start with 0 will be converted in octal (base 8).
// parseFloat() always uses base 10
+ '42';   // 42
+ '010';  // 10
+ '0x10'; // 16. 0x is 16-based.
parseInt('123abc', 10); // 123
+ '123abc'; // NaN
// Number(str) is the same as + str
// for +, if both perands are numbers, add them. otherwise, convert to strings and concat.
+'3' + (+'4') // 7
```

## NaN: Not a number

```javascript
typeof NaN; // number
NaN === NaN; // false
NaN !== NaN; // true
isNaN(NaN); // true
isNaN("Hello"); // true. Try to convert "Hello" to a number and results in NaN
isFinite(NaN); // false
isFinite(-Infinity); // false
```

## [Bitwise trick to convert Float to Int](https://huytd.github.io/bitwise-float-int-trick.html)

```javascript
~~(5.423451) === 5 // Double not
5.423451 | 0 === 5 // Or
5.423451 << 0 === 5 // Right shift
5.423451 >> 0 === 5 // Left shift
```

[This is faster than `Math.floor`.](http://jsben.ch/Ui4Gy) Note that for negative numbers, `~~(-5.423451) === -5` but `Math.floor(-5.423451) === -6`.

## Scientific Number Literals

```javascript
1.234e3; // 1234
1.234E3; // 1234
12340e-1; // 1234
```

## Interesting Arithmetics

```javascript
0.1 + 0.2; // 0.30000000000000004
(0.3 - 0.2) === (0.2 - 0.1); // false
Math.sin(Math.PI); // 1.2246467991473532e-16. should be 0
Number.POSITIVE_INFINITY; // Infinity
Number.POSITIVE_INFINITY * 2; // Infinity
Number.MAX_VALUE; // 1.7976931348623157e+308
Number.MAX_VALUE + 1; // 1.7976931348623157e+308
Number.MAX_VALUE * 2; // Infinity
Number.MIN_VALUE; // 5e-324
Number.MAX_VALUE / 2; // 8.988465674311579e+307
Number.MAX_SAFE_INTEGER; // 9007199254740991
Number.MAX_SAFE_INTEGER + 1; // 9007199254740992
Number.MAX_SAFE_INTEGER + 2; // 9007199254740992
Infinity === Infinity + 1; // true
NaN === NaN + 1; // false
```

## [9999999999999999.0 - 9999999999999998.0](http://geocar.sdf1.org/numbers.html)

The answer is 2 in JavaScript \(same as a lot of other programming languages\), which is obviously wrong. This is due to the inaccurate nature of floating point numbers. Actually if you type `9999999999999999.0` in the console, it would return `10000000000000000`. Since 9999999999999999.0 is larger than `Number.MAX_SAFE_INTEGER` \(9007199254740991\), we really should not be surprised at any weird arithmetic results.

