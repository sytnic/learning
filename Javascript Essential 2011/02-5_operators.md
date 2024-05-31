#


ARITHMETIC OPERATORS

    Score += 10;
    += -= *= /=


INCREMENT / DECREMENT

    a = a + 1;    a = a - 1;
    a += 1;          a -= 1;
    a++;                a--;
    ++a;                --a;


ASSIGNMENT INSTEAD OF EQUALITY

    var a = 5;
    var b = 10;
    if (a = b) {
        // always true!
    }

COMPARISON

    if (a == b) { ... 
    if (a != b) { ... 
    if (a === b) { ... 
    if (a !== b) { ...
    if (a > b) { ...
    if (a < b) { ... 
    if (a >= b) { ... 
    if (a <= b) { ...


LOGICAL AND/OR

    if (a === b && c === d ) { ...
    if (a === b || c === d ) { ...
    if ( (a > b) && (c < d) ) {...
    if (
        (a > b)
          &&
        (c < d) ) { ...


MODULUS

    var year = 2003;
    var remainder = year % 4; // remainder is 3

Prefix / Postfix

    var a = 5;

    alert(++a); // output 6, a == 6

    alert(a++); // output 5, a == 6
 
  code For console, F12:

    var a = 5;
    alert(++a); // output 6, a == 6
    alert(a);

    var a = 5;
    alert(a++); // output 5, a == 6
    alert(a);


TERNARY

    condition ? true : false


TERNARY OPERATOR EXAMPLE

    var playerOne = 500; 
    var playerTwo = 600;

    // regular
    
    if (playerOne > playerTwo) {
        highScore = playerOne;
    }        
    else {
        highScore = playerTwo;
    }

    // alternatively... condition ? true : false

    var highScore = (playerOne > playerTwo) ? playerOne : playerTwo;

For console, F12:

    var a = 5;
    var b = "5";

    if ( a == b ) {
        alert("Yes, they're equal");
    } else {
        alert("They are NOT equal");
    }