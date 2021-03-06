# Angular bootstrap date & time picker version: 0.4.1

Native AngularJS datetime picker directive styled by Twitter Bootstrap 3

[![MIT License][license-image]][license-url]
[![Build Status](https://travis-ci.org/dalelotts/angular-bootstrap-datetimepicker.png?branch=master)](https://travis-ci.org/dalelotts/angular-bootstrap-datetimepicker)
[![Dependency Status](https://david-dm.org/dalelotts/angular-bootstrap-datetimepicker.svg)](https://david-dm.org/dalelotts/angular-bootstrap-datetimepicker)
[![devDependency Status](https://david-dm.org/dalelotts/angular-bootstrap-datetimepicker/dev-status.png)](https://david-dm.org/dalelotts/angular-bootstrap-datetimepicker#info=devDependencies)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![PayPal donate button](http://img.shields.io/paypal/donate.png?color=yellow)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=F3FX5W6S2U4BW&lc=US&item_name=Dale%20Lotts&item_number=angular%2dbootstrap%2ddatetimepicker&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donate_SM%2egif%3aNonHosted "Donate one-time to this project using Paypal")
<a href="https://twitter.com/intent/tweet?original_referer=https%3A%2F%2Fabout.twitter.com%2Fresources%2Fbuttons&amp;text=Check%20out%20this%20%23AngularJS%20directive%20that%20makes%20it%20dead%20simple%20for%20users%20to%20select%20dates%20%26%20times&amp;tw_p=tweetbutton&amp;url=https%3A%2F%2Fgithub.com%2Fdalelotts%2Fangular-bootstrap-datetimepicker&amp;via=dalelotts" target="_blank">
  <img src="http://jpillora.com/github-twitter-button/img/tweet.png"></img>
</a>


[Home / demo page](http://dalelotts.github.io/angular-bootstrap-datetimepicker/)


#Dependencies

Requires:
 * AngularJS 1.4.x or higher (1.0.x will not work)
 * moment.js 2.8.3 or higher for date parsing and formatting
 * bootstrap's glyphicons for arrows (Can be overridden in css)
 
optional:
 * bootstrap's dropdown component (`dropdowns.less`)

#Testing
This directive was written using TDD and all enhancements and changes have related tests.

We use karma and jshint to ensure the quality of the code. The easiest way to run these checks is to use gulp:

```shell
npm install
npm test
```

The karma task will try to open Chrome as a browser in which to run the tests.
Make sure Chrome is available or change the browsers setting in karma.config.js

#Usage
We use npm for dependency management, run

```shell
npm install --save angular-bootstrap-datetimepicker
```

This will copy the angular-bootstrap-datetimepicker files into your components folder, along with its dependencies.

Add the css:

```html
<link rel="stylesheet" href="node_modules/bootstrap/dist/css/bootstrap.css">
<link rel="stylesheet" href="node_modules/angular-bootstrap-datetimepicker/src/css/datetimepicker.css"/>
```

Load the script files in your application:
```html
<script type="text/javascript" src="node_modules/moment/moment.js"></script>
<script type="text/javascript" src="node_modules/bootstrap/dist/js/bootstrap.js"></script>
<script type="text/javascript" src="node_modules/angular/angular.js"></script>
<script type="text/javascript" src="node_modules/angular-bootstrap-datetimepicker/src/js/datetimepicker.js"></script>
```

Add the date module as a dependency to your application module:

```html
var myAppModule = angular.module('MyApp', ['ui.bootstrap.datetimepicker'])
```

Apply the directive to your form elements:

```html
<datetimepicker data-ng-model="data.date"></datetimepicker>
```

## Callback functions


### before-render
Attribute on datetimepicker element

If the value of the before-render attribute is a function, the date time picker will call this function
before rendering a new view, passing in data about the view.

```html
<datetimepicker data-ng-model="data.date" data-before-render="beforeRender($view, $dates, $leftDate, $upDate, $rightDate)"></datetimepicker>
```
This function will be called every time a new view is rendered.
```javascript
$scope.beforeRender = function ($view, $dates, $leftDate, $upDate, $rightDate) {
    var index = Math.floor(Math.random() * $dates.length);
    $dates[index].selectable = false;
}
```

The following parameters are supplied by this directive :
 * '$view' the name of the view to be rendered
 * '$dates' a (possibly empty) array of DateObject's (see source) that the user can select in the view.
 * '$leftDate' the DateObject selected if the user clicks the left arrow.
 * '$upDate' the DateObject selected if the user clicks the text between the arrows.
 * '$rightDate' the DateObject selected if the user clicks the right arrow.


```
DateObject {
    utcDateValue: Number - UTC time value of this date object. It does NOT contain time zone information so take that into account when comparing to other dates (or use localDateValue function).
    localDateValue: FUNCTION that returns a Number - local time value of this date object - same as moment.valueOf() or Date.getTime().
    display: String - the way this value will be displayed on the calendar.
    active: true | false | undefined - indicates that date object is part of the model value.
    selectable: true | false | undefined - indicates that date value is selectable by the user.
    past: true | false | undefined - indicates that date value is prior to the date range of the current view.
    future: true | false | undefined - indicates that date value is after the date range of the current view.
}
```
Setting the .selectable property of a DateObject to false will prevent the user from being able to select that date value.

### on-set-time
Attribute on datetimepicker element

If the value of the on-set-time attribute is a function, the date time picker will call this function
passing in the selected value and previous value.
```html
<datetimepicker data-ng-model="data.date" data-on-set-time="onTimeSet(newDate, oldDate)"></datetimepicker>
```
This function will be called when the user selects a value on the minView.
```javascript
$scope.onTimeSet = function (newDate, oldDate) {
    console.log(newDate);
    console.log(oldDate);
}
```

data-on-set-time="onTimeSet()"  <-- This will work

data-on-set-time="onTimeSet"    <-- **This will NOT work, the ()'s are required**


## Configuration Options
***NOTE*** The configuration optionss are not attributes on the element but rather members of the configuration object,
which is specified in the data-datetimepicker-config attribute.

```html
<datetimepicker data-ng-model="data.date" data-datetimepicker-config="{ dropdownSelector: '.dropdown-toggle' }"></datetimepicker>
```

### startView

String.  Default: 'day'

The view that the datetimepicker should show when it is opened.
Accepts values of :
 * 'minute' for the minute view
 * 'hour' for the hour view
 * 'day' for the day view (the default)
 * 'month' for the 12-month view
 * 'year' for the 10-year overview. Useful for date-of-birth datetimepickers.
 * 'timezone' for the timezone you want to work with (use TZ code from here https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
 * 'timeformat' the timeformat used by moment

### minView

String. 'minute'

The lowest view that the datetimepicker should show.

Accepts the same values as startView.

### minuteStep

Number.  Default: 5

The increment used to build the hour view. A button is created for each <code>minuteStep</code> minutes.

### configureOn

String. Default: null

Causes the date/time picker to re-read its configuration when the specified event is received.

For example, perhaps the startView option in the configuration has changed and you would like the 
new configuration to be used. You can $broadcast the event to cause this directive to use the new configuration. 

### renderOn

String. Default: null

Causes the date/time picker to re-render its view when the specified event is received.

For example, if you want to disable any dates or times that are in the past. 
You can $broadcast the event at an interval to disable times in the past (or any other time valid dates change).

### dropdownSelector

When used within a Bootstrap dropdown and jQuery, the selector specified in dropdownSelector will toggle the dropdown when a date/time is selected.

**NOTE:** dropdownSelector **requires** jquery and bootstrap.js. If do not have these available do not specify this option. If you do, an error will
 be logged and this option will be removed.


## Working with ng-model
The angular-bootstrap-datetimepicker directive requires ng-model and the picked date/time is automatically synchronized with the model value.

This directive also plays nicely with validation directives such as ng-required.

The angular-bootstrap-datetimepicker directive stores and expects the model value to be a standard javascript Date object.

## ng-required directive
If you apply the required directive to element then the form element is invalid until a date is picked.

Note: Remember that the ng-required directive must be explicitly set, i.e. to "true".

## Examples

### Inline component.

```html
<datetimepicker data-ng-model="data.date" ></datetimepicker>
```
### Inline component with data bound to the page with the format specified via date filter:

```html
<datetimepicker data-ng-model="data.date" ></datetimepicker>
```
```
<p>Selected Date: {{ data.date | date:'yyyy-MM-dd HH:mm' }}</p>
```

Display formatting of the date field is controlled by Angular filters.

### As a drop-down:

```html
<div class="dropdown">
    <a class="dropdown-toggle" id="dLabel" role="button" data-toggle="dropdown" data-target="#" href="">
        Click here to show calendar
    </a>
    <ul class="dropdown-menu" role="menu" aria-labelledby="dLabel">
        <datetimepicker data-ng-model="data.date"
                        data-datetimepicker-config="{ dropdownSelector: '.dropdown-toggle' }"></datetimepicker>
    </ul>
</div>
```
In this example, the drop-down functionality is controlled by Twitter Bootstrap.
The <code>dropdownSelector</code> tells the datetimepicker which element is bound to the Twitter Bootstrap drop-down so
the drop-down is toggled closed after the user selectes a date/time.

### Drop-down component with associated input box.
```html
<div class="dropdown">
    <a class="dropdown-toggle my-toggle-select" id="dLabel" role="button" data-toggle="dropdown" data-target="#" href="">
        <div class="input-append"><input type="text" class="input-large" data-ng-model="data.date"><span class="add-on"><i
                class="icon-calendar"></i></span>
        </div>
    </a>
    <ul class="dropdown-menu" role="menu" aria-labelledby="dLabel">
        <datetimepicker data-ng-model="data.date"
                        data-datetimepicker-config="{ dropdownSelector: '.my-toggle-select' }"></datetimepicker>
    </ul>
</div>
```
In this example, the drop-down functionality is controlled by Twitter Bootstrap.
The <code>dropdownSelector</code> tells the datetimepicker which element is bound to the Twitter Bootstrap drop-down so
the drop-down is toggled closed after the user selectes a date/time.


## I18N / l10n support

All internationalization is handled by Moment.js, see Moment's documentation for details.
In most cases, all that is needed is a call to ```moment.locale(String)```

One exception is the title of the month view - moment does not (yet) have a localized format for month and year.

```JavaScript
moment.locale('en');        // English
moment.locale('zh-cn');     // Simplified chinese
```

## License

angular-bootstrap-datetimepicker-timezone is entireley based on the awesome project https://dalelotts.github.io/angular-bootstrap-datetimepicker/ by Dale Lotts. It's released under the MIT license and is copyright 2017 Francesco Stasi. Boiled down to smaller chunks, it can be described with the following conditions.

## It requires you to:

* Keep the license and copyright notice included in angular-bootstrap-datetimepicker's CSS and JavaScript files when you use them in your works

## It permits you to:

* Freely download and use angular-bootstrap-datetimepicker, in whole or in part, for personal, private, company internal, or commercial purposes
* Use angular-bootstrap-datetimepicker in packages or distributions that you create
* Modify the source code
* Grant a sublicense to modify and distribute angular-bootstrap-datetimepicker to third parties not included in the license

## It forbids you to:

* Hold the authors and license owners liable for damages as angular-bootstrap-datetimepicker is provided without warranty
* Hold the creators or copyright holders of angular-bootstrap-datetimepicker liable
* Redistribute any piece of angular-bootstrap-datetimepicker without proper attribution
* Use any marks owned by Knight Rider Consulting, Inc. in any way that might state or imply that Knight Rider Consulting, Inc. endorses your distribution
* Use any marks owned by Knight Rider Consulting, Inc. in any way that might state or imply that you created the Knight Rider Consulting, Inc. software in question

## It does not require you to:

* Include the source of angular-bootstrap-datetimepicker itself, or of any modifications you may have made to it, in any redistribution you may assemble that includes it
* Submit changes that you make to angular-bootstrap-datetimepicker back to the angular-bootstrap-datetimepicker project (though such feedback is encouraged)