# Using objects with functions

The function will allow me to pull data from the `myBook` object. It does this by passing the argument `book` and then the key within the object we want to collect so `book.title` for example.

```javascript
let getSummary = (book) => {
    return {
        summary: `${book.title} by ${book.author}`,
        summaryPageCount: `${book.title} is ${book.pageCount} long`,
    };
};
```

To get the return statement to return the information from the book object, we have to pass this when calling the function. Such as `let bookSummary = getSummary(myBook)`

Full example below:

```javascript
let myBook = {
    title: '1984',
    author: 'George Orwel',
    pageCount: 326,
};

let otherBook = {
    title: 'A Peoples History',
    author: 'Howard Zinn',
    pageCount: 723,
};

let getSummary = (book) => {
    return {
        summary: `${book.title} by ${book.author}`,
        summaryPageCount: `${book.title} is ${book.pageCount} long`,
    };
};

let bookSummary = getSummary(myBook);
console.log(bookSummary);
// prints ---
// summary: '1984 by George Orwel',
// summaryPageCount: '1984 is 326 long'

let otherBookSummary = getSummary(otherBook);
console.log(otherBookSummary);
// prints ---
// summary: 'A Peoples History by Howard Zinn',
// summaryPageCount: 'A Peoples History is 723 long'

// Another Example
let tempConversion = (fahrenheit) => {
    return {
        fahrenheit: fahrenheit,
        celsius: ((fahrenheit - 32) * 5) / 9,
        kelvin: ((fahrenheit + 459.67) * 5) / 9,
    };
};

let conversion = tempConversion(745);
console.log(conversion.celsius); // returns the celsius value
```
