JavaScript Functions

📌 Introduction

A function in JavaScript is a reusable block of code designed to perform a specific task. Functions help make programs shorter, organized, and easier to maintain.


---

1. Function Declaration

A function can be created using the function keyword.

function greet() {
    console.log("Hello, Divyanshu!");
}

greet();

Output:

Hello, Divyanshu!


---

2. Function with Parameters

Parameters allow us to pass data into a function.

function add(a, b) {
    return a + b;
}

console.log(add(10, 20));

Output:

30


---

3. Function with Return Value

The return statement sends a value back from the function.

function square(number) {
    return number * number;
}

let result = square(5);
console.log(result);

Output:

25


---

4. Function Expression

A function can be stored inside a variable.

const multiply = function(a, b) {
    return a * b;
};

console.log(multiply(4, 5));

Output:

20


---

5. Arrow Function

Arrow functions provide a shorter way to write functions.

const subtract = (a, b) => {
    return a - b;
};

console.log(subtract(20, 8));

Output:

12

For a single expression, it can be shortened further:

const cube = number => number * number * number;

console.log(cube(3));

Output:

27


---

6. Default Parameters

A default value is used when an argument is not provided.

function welcome(name = "Divyanshu") {
    console.log("Welcome " + name);
}

welcome();

Output:

Welcome Divyanshu


---

7. Rest Parameters

Rest parameters allow a function to accept multiple arguments.

function total(...numbers) {
    return numbers.reduce((sum, number) => sum + number, 0);
}

console.log(total(10, 20, 30, 40));

Output:

100


---

8. Callback Function

A function passed as an argument to another function is called a callback function.

function calculate(a, b, operation) {
    return operation(a, b);
}

const add = (x, y) => x + y;

console.log(calculate(10, 5, add));

Output:

15


---

9. Scope of Functions

Variables declared inside a function are generally available only inside that function.

function test() {
    let message = "Hello";
    console.log(message);
}

test();

The message variable cannot normally be accessed outside the function.


---

10. Why Functions Are Useful

Functions help us:

♻️ Reuse code

📖 Make code easier to understand

🛠️ Reduce repeated code

🧩 Divide a large program into smaller parts

🐛 Make debugging easier

🔄 Perform the same task with different values



---

