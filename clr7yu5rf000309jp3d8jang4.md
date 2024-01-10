---
title: "Autoboxing in Javascript"
datePublished: Wed Jan 10 2024 15:59:06 GMT+0000 (Coordinated Universal Time)
cuid: clr7yu5rf000309jp3d8jang4
slug: autoboxing-in-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/GopRYASfsOc/upload/ad93ddb6daf5b8aa7dbe7a9a26c9f788.jpeg
tags: javascript, advanced, autoboxing

---

Auto-boxing is the process in JavaScript where a [primitive data type](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures) is automatically converted to its corresponding object wrapper when you try to access properties or methods that are specific to objects. This process allows primitives to behave like objects temporarily.

Let's take the example of strings and auto-boxing:

```javascript
const myString = "Hello"; // Primitive string
console.log(myString.length); // Accessing length property
In the above code:
```

1. `myString` is a primitive string.
    
2. When you try to access the `length` property of `myString`, JavaScript automatically converts the primitive string to a `String` object behind the scenes (autoboxing).
    
3. The `length` property is then accessed on the temporary `String` object.
    

This process allows you to use properties and methods that belong to the corresponding object type (`String` in this case) on primitive values.

Similarly, auto-boxing occurs for other primitive types like numbers and booleans when you try to use properties or methods associated with their corresponding object wrappers (`Number` and `Boolean`, respectively).

```javascript
const myNumber = 42; // Primitive number
console.log(myNumber.toFixed(2)); // Accessing toFixed method
```

In this example, `myNumber` is a primitive number, but when you try to use the `toFixed` method, JavaScript auto-boxes it to a temporary `Number` object, allowing you to use the method.

It's important to note that these auto-boxing conversions are temporary, and the original primitive values remain unchanged. Once the property or method is accessed, the temporary object is discarded.