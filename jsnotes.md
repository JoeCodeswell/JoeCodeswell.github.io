# JavaScript Notes


## Idioms
``` javascript
/* BUMMERS:
  - as of Nov 2, 2017 import statements are NOT in Node.js LTS
  - ES6 postponed Array comprehensions (likePython).
        (https://stackoverflow.com/a/35637068/601770)
  - typeof object name discovery: https://stackoverflow.com/q/332422/601770
  - google: es6 prototypes vs closures vs classes
    - NOT LIKE: Zen of Python is, "There should be one — and preferably -
        only one obvious way to do it."
*/
 
/* Create Array lodash Python-like range */
 
var _ = require('lodash'); // previously, npm install lodash 
searchRadii: _.range(50, 750, 50);
// expected [ 50, 100, 150, 200, 250, 300, 350, 400, 450, 500, 550, 600, 650, 700 ]
// NB last is 700  NOT 750
 
/* Destructuring assignment [array or object](https://goo.gl/LBQ5eC) */
 
// array
var a, b, rest;
[a, b] = [10, 20];
console.log(a);
// expected output: 10
 
// object 
var o = {p: 42, q: true};
var {p, q} = o;
console.log(p); // 42
console.log(q); // true
 
// i/o
const fs = require('fs');
 
/* read to string one liner */
var mystr= fs.readFileSync("./mytextfile.txt").toString('utf-8')
 
/* write string one liner */
fs.writeFileSync('anthemTemplateOptions.txt', 'Hello Node', (err) => { if (err) throw err; console.log('write was OK!'); } );
 
 
/* Re-direct redir console.log  https://stackoverflow.com/a/45291964/601770 */
const out = fs.createWriteStream('./stdout.log') 
const err = fs.createWriteStream('./stderr.log')
const jconsole = new console.Console(out, err);
jconsole .log('54-40 or fight!')
 
 
/* readdirSync */
let items = fs.readdirSync(nextSundayEmailDirPath)
for (let value of items ) {
    console.log('value: '+value)
    if (value.endsWith('.htm')){nextSundayEmailFilename = value;
} 
 
 
/* forEach with index == ix */
const items = ['item1', 'item2', 'item3'];
items.forEach((itm,ix) => {  // loop
    console.log(itm+ix.toString());
}); // end loop
OUTPUT:
item10
item21
item32
 
/* forEach */
const items = ['item1', 'item2', 'item3'];
items.forEach(itm => {  // loop
    console.log(itm);
}); // end loop
OUTPUT:
item1
item2
item3
 
/* for-of (need also for-in) */
let myIterable = [10, 20, 30]
 
for (let value of myIterable ) {
  value += 1
  console.log(value)
}
// 11
// 21
// 31
 
/* timestamp */
<p id="myp"></p>
<script>
window.onload = function(){ 
  let ts = (new Date().toISOString()).replace(/:/g, '.').replace('T', '__').slice(0, -5);
  document.getElementById("myp").innerHTML = "Hello. It's " +ts;   // 2017-11-13__18.33.16
};
</script>
 
/* string format %s   */
const util = require('util')
util.format('Timestamp: %s', '2017-11-13_18.26.52')
// 2017-11-13_18.26.52
 
/* slice NOT same as Python */
 +---+---+---+---+---+
 | H | e | l | p | A |
 +---+---+---+---+---+
 0   1   2   3   4   5
-5  -4  -3  -2  -1
 
> s.slice(null)
'HelpA'
> s.slice(0)
'HelpA'
> s.slice(-1)
'A'
> s.slice(-5)
'HelpA'
> s.slice(null,null)
''
> s.slice(null, 5)
'HelpA'
> s.slice(null, 4)
'Help'
> s.slice(0, 4)
'Help'
> s.slice(0, -2)
'Hel'
 
/** ES6 string functions & string features 
 
2ality.com/2015/01/es6-strings.html
exploringjs.com/es6/ch_strings.html
 
**/
 
/* ES6 String Templates & Multi-line String Literals */
// see https://stackoverflow.com/a/32695337/601770
//     https://developers.google.com/web/updates/2015/01/ES6-Template-Strings
let name = 'Fred'
let tm = `Dear ${name},
This is to inform you, ${name}, that you are
IN VIOLATION of Penal Code 64.302-4.
Surrender yourself IMMEDIATELY!
THIS MEANS YOU, ${name}!!!
`
console.log(tm)
// OUTPUT:
Dear Fred,
This is to inform you, Fred, that you are
IN VIOLATION of Penal Code 64.302-4.
Surrender yourself IMMEDIATELY!
THIS MEANS YOU, Fred!!!
 
/* ES6 String substr & substring */
// see https://stackoverflow.com/a/3745518/601770
> let s = 'href="https://docs.g.com/doc/d/1lRuC1p7TXRbmI/edit?usp=sharing_eil&ts=5a1f0fbf"'
 
> s.substring(6)  // same as s.substr(6)
'https://docs.g.com/doc/d/1lRuC1p7TXRbmI/edit?usp=sharing_eil&ts=5a1f0fbf"'
> s.substring(6) === s.substr(6)
true
 
// HOWEVER with 2nd arg false
> s.substring(6, s.length-1) === s.substr(6, s.length-1)
false
 
> s.substring(6, s.length-1) // 2nd arg is index to stop at (but not include)
'https://docs.g.com/doc/d/1lRuC1p7TXRbmI/edit?usp=sharing_eil&ts=5a1f0fbf'
 
> s.substr(6, s.length-1) // 2nd arg is the maximum length to return
'https://docs.g.com/doc/d/1lRuC1p7TXRbmI/edit?usp=sharing_eil&ts=5a1f0fbf"'
 
/* get string between 2 strings  (lstr & rstr) */
function between(instr, lstr, rstr){return instr.split(lstr)[1].split(rstr)[0]}
 
var emailURLTest = 'https://docs.google.com/document/d/1lRuCJUNK_IN_HEREbgbeCZKGeg4yfdfnw7TXRbmI/edit?usp=sharing_eil&ts=5aJUNK2bf'
 
between(emailURLTest, 'https://docs.google.com/document/d/', '/edit?')
 
// '1lRuCJUNK_IN_HEREbgbeCZKGeg4yfdfnw7TXRbmI'
 
/* [JSON  parse & stringify](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON) */
 
// parse
var superHeroesText = fs.readFileSync("./superHeroesText.txt").toString('utf-8')
var superHeroes = JSON.parse(superHeroesText)
 
// stringify
var myJSON = { "name": "Chris", "age": "38" };
myJSON
var myString = JSON.stringify(myJSON);
myString
 
// inspect object  circular object first level only NO imports
var output = '';
var property = null;
for (property in this.themap) {
  output += property + ': ' + this.themap[property]+'; ';
};
console.log(output);
 
/* node.js modules ( != es6 modules )
https://www.w3schools.com/nodejs/nodejs_modules.asp
Content:
    // importing code in app.js
    // exporting code in j_utils.js
    // OUTPUT
*/
 
// importing code in app.js
let util = require('./j_utils.js')
console.log(util.myTimeStamp())
console.log(util.cwd())
 
// exporting code in j_utils.js
exports.myTimeStamp= function () {
    let ts = (new Date().toISOString()).replace(/:/g, '.').replace('T','__').slice(0, -5)
    return ts
}
exports.cwd= function () {
    return process.cwd()
}
 
// OUTPUT
TrinitySundayMusic$ node app.js
2017-11-22__16.33.41
/mnt/c/1d/TrinitySundayMusicPj/TrinitySundayMusic
TrinitySundayMusic$
 
/* like Python if __main__ */
 
my_main = function () {
    console.log('In my_main()')
}
exports.getMusicURL = function () {
    let music_url = my_main()
    return music_url
}
// run from node
if (require.main === module) {
    my_main()
}
 
/* Arrow Function */
// [tldr](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 
let funcName = (param1, param2, …, paramN) => { statements } 
 
/* class example */
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes
// [Cory Rylan](https://coryrylan.com/blog/javascript-es6-class-syntax)
/*
Cory Rylan: "Here is one final look at our Person and Programmer classes. The getters and setters are not necessary in this use case but are there to demonstrate the new syntax."
Joe: "VERY NICE!"
*/
 
class Person {
  constructor(name) {
    this._name = name;
  }
 
  get name() {
    return this._name;
  }
 
  set name(newName) {
    this._name = newName;
  }
 
  walk() {
    console.log(this._name + ' is walking.');
  }
}
          
class Programmer extends Person { 
  constructor(name, programmingLanguage) {
    super(name);
    this._programmingLanguage = programmingLanguage;
  }
 
  get programmingLanguage() {
    return this._programmingLanguage;
  }
 
  set programmingLanguage(newprogrammingLanguage) {
    this._programmingLanguage = newprogrammingLanguage;
  }
 
  writeCode() {
    console.log(this._name + ' is coding in ' + this._programmingLanguage + '.');
  }
}
          
let bob = new Person('Bob');
bob.walk();
          
let cory = new Programmer('Cory', 'JavaScript');
cory.walk();
cory.writeCode();
console.log(cory.name);
 
 
 
/* fetch urls in a map loop */
// https://stackoverflow.com/a/38364482
const fetch = require('node-fetch')
let urls = [
   "https://jsonplaceholder.typicode.com/todos/1",
   "https://jsonplaceholder.typicode.com/todos/2",
   "https://jsonplaceholder.typicode.com/todos/3"
];
let myPromises = urls.map((itm, ix) => {
  console.log("The current iteration is: " + ix);
  console.log("The current element is: " + itm);
  return fetch(itm);
}); // end loop
The current iteration is: 0
The current element is: https://jsonplaceholder.typicode.com/todos/1
The current iteration is: 1
The current element is: https://jsonplaceholder.typicode.com/todos/2
The current iteration is: 2
The current element is: https://jsonplaceholder.typicode.com/todos/3
undefined
> console.log(myPromises.length);
3
> myPromises[0].then(response => response.json()).then(json => console.log(json))
Promise {
  <pending>,
  domain:
   Domain {
     domain: null,
     _events:
      { removeListener: [Function: updateExceptionCapture],
        newListener: [Function: updateExceptionCapture],
        error: [Function: debugDomainError] },
     _eventsCount: 3,
     _maxListeners: undefined,
     members: [] } }
> { userId: 1,
  id: 1,
  title: 'delectus aut autem',
  completed: false }
> myPromises[1].then(response => response.json()).then(json => console.log(JSON.stringify(json)))
Promise {
  <pending>,
  domain:
   Domain {
     domain: null,
     _events:
      { removeListener: [Function: updateExceptionCapture],
        newListener: [Function: updateExceptionCapture],
        error: [Function: debugDomainError] },
     _eventsCount: 3,
     _maxListeners: undefined,
     members: [] } }
> {"userId":1,"id":2,"title":"quis ut nam facilis et officia qui","completed":false}
Share this:
```