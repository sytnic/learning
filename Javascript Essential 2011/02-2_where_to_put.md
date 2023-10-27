# Where to put

В файле
```html
<!DOCTYPE html> 
<html lang="en">
<title>Hello</title>
<head>
</head>
<body>
  <div>
    <p>regular body text</p>
  </div>
  <script>
    alert("hello");
  </script>
</body>
</html>
```

Внешний файл
```html
simple.html

<!DOCTYPE html>
<html lang="en">
<title>Hello</title>
<head>
</head>
<body>
    <div>
        <p>regular body text</p>
    </div>
    <script src="myscript.js">
    </script>
</body>
</html>

myscript.js

alert("hello");
```

WHAT ABOUT THE TYPE ATTRIBUTE?  
Необязательный атрибут type.
```html
<script src="myscript.js" 
type="text/javascript"></script> 

type="text/ecmascript"  
type="application/javascript"  
type="application/ecmascript"  
```

LOCATION OF THE SCRIPT TAG
```html
<!DOCTYPE html>
<html lang="en">
<head>
<title>Hello</title>
</head>
<body>
    <div>
    <p>regular body text</p>
    </div>
<script src="myscript.js"></script>
</body>
</html>
```


