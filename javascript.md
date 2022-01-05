# JavaScript

* [Classes](#classes)
* [Modules](#modules)

## Classes

This example declares two classes: `Nurse` which extends from `HospitalEmployee`.
It also have a static method (callable from the class, not from an instance of the class).

``` javascript
class HospitalEmployee {
  constructor(name) {
    this._name = name;
    this._remainingVacationDays = 20;
  }
  
  get name() {
    return this._name;
  }
  
  get remainingVacationDays() {
    return this._remainingVacationDays;
  }
  
  takeVacationDays(daysOff) {
    this._remainingVacationDays -= daysOff;
  }

  static generatePassword() {
    return Math.floor(Math.random() * 10000);
  }
}

class Nurse extends HospitalEmployee {
  constructor(name, certifications) {
    super(name);
    this._certifications = certifications;
  } 
  
  get certifications() {
    return this._certifications;
  }
  
  addCertification(newCertification) {
    this.certifications.push(newCertification);
  }
}

const nurseOlynyk = new Nurse('Olynyk', ['Trauma','Pediatrics']);
nurseOlynyk.takeVacationDays(5);
console.log(nurseOlynyk.remainingVacationDays);
nurseOlynyk.addCertification('Genetics');
console.log(nurseOlynyk.certifications);
console.log('Your password is ' + HospitalEmployee.generatePassword());
```

Output:

``` text
15
[ 'Trauma', 'Pediatrics', 'Genetics' ]
Your password is 3989
```

## Modules

### Export module

This example exports two functions: `squareArea` and `circleArea`.

``` javascript
/* shape-area.js */
const PI = Math.PI;

// Define and export circleArea() and squareArea() below
function squareArea(sideLength) {
  return sideLength * sideLength;
}
 
module.exports.squareArea = squareArea;
 
module.exports.circleArea = function(radiusLength) {
  return PI * radiusLength * radiusLength;
};
```

### Import module

Use require to import the whole module:

``` javascript
/* app.js */ 

const radius = 5;
const sideLength = 10;

// Option 1: import the entire shape-area.js module here.
const areaFunctions = require('./shape-area.js');

// use the imported .circleArea() and .squareArea() methods here
// With option 1:
const areaOfCircle = areaFunctions.circleArea(radius);
const areaOfSquare = areaFunctions.squareArea(sideLength);
```

Or load only what we need with *object destructuring*:

``` javascript
// Option 2: import circleArea and squareArea with object destructuring
const { circleArea, squareArea } = require('./shape-area.js');

// with option 2:
const areaOfCircle = circleArea(radius);
const areaOfSquare = squareArea(sideLength);
```
