monkey-script
=============

MonkeyScript is a tiny language for writing serieses of non-blocking JavaScript commands in a blockish manner.

For example, the following MonkeyScript code:
```CoffeeScript
getBanana =  ->
 b = download "http://banana.com"
 storeInDB b
 alert "Downloaded #{b} and saved it"
```
Compiles to this CoffeScript Code:
```CoffeeScript
getBanana =  ->
 _b = new Block()
 _b.addRow => @b = download "http://banana.com"    , => _b.exec 1
 _b.addRow => storeInDB @b                         , => _b.exec 2
 _b.addRow => alert "Downloaded #{@b} and saved it", => _b.exec 3
 _b.exec
```
And Evetually to the horrifying JavaScript code bellow:
```JavaScript
var getBanana;

getBanana = function() {
  var _b,
    _this = this;
  _b = new Block();
  _b.addRow(function() {
    return _this.b = download("http://banana.com", function() {
      return _b.exec(1);
    });
  });
  _b.addRow(function() {
    return storeInDB(_this.b, function() {
      return _b.exec(2);
    });
  });
  _b.addRow(function() {
    return alert("Downloaded " + _this.b + " and saved it", function() {
      return _b.exec(3);
    });
  });
  return _b.exec;
};
```
Happy monkey scripting!
