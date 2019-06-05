# Power using Recursion 
A function pow(x, n) that raises x to a natural power of n. In other words, multiplies x by itself n times.
```javascript
function pow(x, n) {
  if (n == 1) {
    return x;
  } else {
    return x * pow(x, n - 1);
  }
}
alert( pow(2, 3) ); // 8
```

