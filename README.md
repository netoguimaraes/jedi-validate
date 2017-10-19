# Jedi Validate

[![npm version](https://badge.fury.io/js/jedi-validate.svg)](https://www.npmjs.com/package/jedi-validate)
[![doc coverage](http://dmitriytat.github.io/jedi-validate/badge.svg)](http://dmitriytat.github.io/jedi-validate/)
[![Build Status](https://travis-ci.org/dmitriytat/jedi-validate.svg?branch=master)](https://travis-ci.org/dmitriytat/jedi-validate)
[![Coverage Status](https://coveralls.io/repos/github/dmitriytat/jedi-validate/badge.svg?branch=master)](https://coveralls.io/github/dmitriytat/jedi-validate?branch=master)
[![Dependencies](https://david-dm.org/dmitriytat/jedi-validate.svg)](https://david-dm.org/dmitriytat/jedi-validate)

* [Introduction & documentation](#intro)
* [Installation and Usage](#install)
* [Options](#options)
  * [Default Options](#default)
  * [Ajax Parameters](#ajax)
  * [Data Encoding Options](#encoding)
* [Validation Rules](#rules)
* [Validators](#validators)
* [Error Message](#error)


<h1>
    <a name="intro"></a>
    Introduction
</h1>


Jedi Validate is a lightweight form validation component.

- [Documentation](http://dmitriytat.github.io/jedi-validate/)
- [Test coverage](https://coveralls.io/github/dmitriytat/jedi-validate)

## How Can I Use It?

This is a JS class, and you can create a new instance by passing in a DOM element and an options object.

```javascript
// npm install jedi-validate


import JediValidate from 'jedi-validate';

const JediValidate = new JediValidate(document.querySelector('#form5'));
```


By default, the form will be sent via ajax with the parameters which were set in HTML.

## Why Should I Use It?

Because it provides a strict json format for interaction. You can send a form in many different ways:

* serialized
* as a JSON object
* as FormData.

But server answer always has one structure. It is easier to implement.

<h2>
    <a name="install"></a>
    Build and Test
</h2>

If you would like to build the source code, run tests, or contribute, then first fork or clone this repo onto your local machine. Ensure NodeJS is installed and in turn npm. Check in a terminal with `node -v` and `npm -v`.

To install project dependencies first run,

```
npm install
```

### Build
To build the source and watch for changes in the terminal run,

```
npm run build
```

### Build and Serve
To bundle the source and serve it to `localhost:4000` run,

```
npm run dev
```
This will open a webpack local server where you can navigate to the desired directory or resource. The test page is located in **`example/bootstrap.html`**

### Running Tests
The tests are not yet complete, and runtime errors will occur when attempting to run the tests in the console or through the test browser.

<h1>
    <a name="options"></a>
    Options
</h1>

There are three types of options:

* Default component options
* Form attributes such as action or method
* Initialization options

<h2>
   <a name="default"></a>
   Default Options
</h2>


```javascript
    {
        ajax: {
            url: null,
            enctype: 'application/x-www-form-urlencoded',
            sendType: 'serialize', // 'formData', 'json'
            method: 'GET'
        },
        rules: {},
        messages: {},
        containers: {
            parent: 'form-group',
            message: 'help-block',
            baseMessage: 'base-error'
        },
        states: {
            error: 'error',
            valid: 'valid',
            pristine: 'pristine',
            dirty: 'dirty'
        },
        callbacks: {
            success: function () {
            },
            error: function () {
            }
        },
        clean: true,
        redirect: true
    }
```

<h2>
  <a name="ajax"></a>
  ajax
</h2>

Under the ajax option we define how to send the form.
It can be ```null``` if we do not want the form to be sent,
or it can be an object with the following options:

#### url
default: ```null```
Can be overridden by the `action` form attribute or init options.

#### enctype
default: ```'application/x-www-form-urlencoded'```
Can be overridden by the `enctype` form attribute, init options, or `sendType`.

#### method
default: ```'GET'```
Can be overridden by the `method` form attribute or init options.

<a name="encoding"></a>
#### sendType
default: ```'serialize'```

You can encode and send the data in three different ways. Valid options are:

* ```'formData'``` - send form as FormData. ```'Content-type'``` to ```'multipart/form-data'```
* ```'json'``` - send form as a JSON object. Set ```'Content-type'``` to ```'application/json; charset=utf-8'```
* ```'serialize'``` - send form as a regular request. Set ```'Content-type'``` to ```'application/x-www-form-urlencoded'```

Files can only be sent using 'formData' encoding.

#### serialize

```
    name=111&phone=222222222&email=wow%40wow.com
```

#### formData

```
-----------------------------678106150613000712676411464
Content-Disposition: form-data; name="name"

111
-----------------------------678106150613000712676411464
Content-Disposition: form-data; name="phone"

222222222
-----------------------------678106150613000712676411464
Content-Disposition: form-data; name="email"
...
```

#### json

```json
    {"name":"111","phone":"222222222","email":"wow@wow.com","file":"index.html"}
```

<h1>
  <a name="rules"></a>
  Validation Rules
</h1>

Rules used to validate input. Each form element will be matched by the 'name' attribute with a corresponding rule, if one exists. If no rule exists, then no validation will occur.

#### Basic Rules:

Rules are not defined by default, but they can be set via attributes or classes in HTML, or in the init options.

> - required :  boolean
> - regexp : RegExp
> - email :  boolean
> - tel :  boolean
> - url :  boolean
> - filesize: number
> - extension: string
> - min: number
> - max: number
> - step: number
> - minDate: Date
> - maxDate: Date
> - accepts: string


These attributes can be used
> - type - email, tel or url (regexp will be used for each type)
> - pattern - regexp with attribute value
> - required - check input for empty value
> - min - min for number and date (ISO) inputs
> - max - max value for number and date (ISO) inputs
> - step - step for number and date inputs

Example:

```html
    <input id="name" type="text" name="name" required class="required">
    <input id="email" type="email" name="email" class="required">
    <input id="date" type="date" name="date" min="2011-12-30" class="required">
    <input id="number" type="number" name="number" min="2" max="20" step="2">
    <input id="file" type="file" name="file" accept=".csv">
```

* type="email" or class="email" to validate as email
* required or class="required" to validate as a required field

#### Custom Validation Rules

You can set your own rules using the ```addMethod``` function:

```
JediValidate.addMethod('methodName', function (value, options) {
    return // true if valid
}, 'Error message');
```

<h1>
  <a name="validators"></a>
  Validators
</h1>

Default validation functions that you can use as example of validators.

#### regexp
Validate based on a specified regular expression.
```
function regexp(value, exp)
```
Example:
```
regexp('Hello', /^\w+$/) === true
```

#### email
Validate that the given input is an email.
```
function email(value)
```
Example:
```
email('ddd@dd.dd') === true
email('ddddd.dd') === false
```

#### filesize
Given a `FileList`, validates that the file size is less than the given size.
```
filesize(value, size)
```
Example:
```
filesize([{ size: 10 }], 20) === true
```

#### extension
Validates that the file extension of the given file is of a certain type/value.
```
extension(value, extensions)
```
Example:
```
extension([{ name: 'lol.png' }], 'png|jpg') === true
```

#### tel
Validates that the string passed is a telephone number.
```
tel(value)
```
Example:
```
tel('8800123124') === true
tel('231') === false
```

#### url
Validates that the string passed is a URL.
```
url(value)
```
Example:
```
url('http://github.com') === true
url('231') === false
```

#### min
Validates that the given number is larger than min.
```
min(value, number)
```
Example:
```
min(10, 5) === true
min(1, 5) === false
```

#### max
Validates that the given number is smaller than max.
```
max(value, number)
```
Example:
```
max(10, 50) === true
max(5, 1) === false
```

#### step
Validates that the given number is a multiple of another.
```
step(value, number)
```
Example:
```
step(5, 5) === true
step(5, 12) === false
```

#### mindate
Validates that the given date is later than mindate.
```
minDate(date, mindate)
```
Example:
```
minDate(new Date('2017/10/12'), new Date('2017/10/10')) === true
minDate(new Date('2017/10/12'), new Date('2017/10/13')) === false
```

#### maxdate
Validates that the given date is earlier than maxdate.
```
maxDate(date, maxdate)
```
Example:
```
maxDate(new Date('2017/10/12'), new Date('2017/10/12')) === true
maxDate(new Date('2017/10/12'), new Date('2017/10/10')) === false
```

### Initialization Options Example

Add rules as part of your options object when initializing:

```javascript
    const validator = new JediValidate(formWrapper, {
        rules: {
            name: {
                required: true
            },
            email: {
                email: true
            },
            phone: {
                regexp: /^([\+]+)*[0-9\x20\x28\x29\-]{5,20}$/
            },
            file: {
                filesize: 10000,
                extension: "html|css|txt"
            }
            file2: {
                filesize: [10000, "two-files-checkbox"], // check only if checkbox checked
                extension: ["html|css|txt", "two-files-checkbox"], // without recollect by default
                another: ['param', function(oldData) {
                   const newData = validator.collect('two-files-checkbox'); // manual data recollect for concrete field
                   return !!newData['two-files-checkbox'];
               }];
            }
        }
    });
```

You can recollect data of all form or just input by name. Method return new data.

```
validator.collect(); // all form
validator.collect('two-files-checkbox'); // one field
```


<h1>
  <a name="error"></a>
  Error Messages
</h1>

You can define your own error messages in case validation fails. In case a form element fails validation, then the message corresponding to the element's 'name' attribute will apply.

```javascript
    messages: {
        phone: {
            regexp: "Invalid phone number"
        },
        file: {
            filesize: "File is too big"
        },
        email: {
            regexp: "Invalid email"
        },
        extension: {
            extension: "Invalid file extension make sure your file ends with html, css, txt extensions."
        }
    },
```
