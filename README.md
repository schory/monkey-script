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
 _b.addRow => download "http://banana.com"         , (_)=> @b=_; _b.exec 1
 _b.addRow => storeInDB @b                         , (_)=> _b.exec 2
 _b.addRow => alert "Downloaded #{@b} and saved it", (_)=> _b.exec 3
 _b.exec

class Block
 rowFuncs = []
 addRow = (f) => rowFuncs << f
 exec = (_lineNumber = 0) => rowFuncs[_lineNumber]()
```
And Evetually to the horrifying JavaScript code bellow:
```JavaScript
var Block, getBanana;

getBanana = function() {
  var _b,
    _this = this;
  _b = new Block();
  _b.addRow(function() {
    return download("http://banana.com", function(_) {
      _this.b = _;
      return _b.exec(1);
    });
  });
  _b.addRow(function() {
    return storeInDB(_this.b, function(_) {
      return _b.exec(2);
    });
  });
  _b.addRow(function() {
    return alert("Downloaded " + _this.b + " and saved it", function(_) {
      return _b.exec(3);
    });
  });
  return _b.exec;
};

Block = (function() {
  var addRow, exec, rowFuncs,
    _this = this;

  function Block() {}

  rowFuncs = [];

  addRow = function(f) {
    return rowFuncs << f;
  };

  exec = function(_lineNumber) {
    if (_lineNumber == null) {
      _lineNumber = 0;
    }
    return rowFuncs[_lineNumber]();
  };

  return Block;

}).call(this);
```
Happy monkey scripting!
