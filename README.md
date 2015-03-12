# Description

While i was working on few projects, i have come across few situations where i had to write some directives. Here i want to mention few of the directives here. These might be useful for you. I am mentioning few directives here, so that you don't need to search for these directives at some other place. Each and every directive has a **demo** link.

## List of my directives

- [standard-time-meridian](#standard-time-meridian)
- [standard-time-no-meridian](#standard-time-no-meridian)
- [date-string-to-epoch](#date-string-to-epoch)
- [ionic-time-picker](#ionic-time-picker)
- [tooltip-follow-cursor](#tooltip-follow-cursor)
- [check-with-current-time](#check-with-current-time)


### standard-time-meridian
#### Description 
Converts **Epoch Time** to **HH:MM:AM/PM** 12-hour format.

[Know more about Epoch Converter](http://www.epochconverter.com/)

[Demo](http://plnkr.co/edit/xuPoZhftWFtr9PufyLQT)

##### Controller
````javascript
$scope.epochTime = 12600;
````
##### HTML
````HTML
<standard-time-meridian etime='epochTime'></standard-time-meridian>
````

##### Directive
````javascript
.directive('standardTimeMeridian', function() {
  return {
    restrict: 'AE',
    replace: true,
    scope: {
      etime: '=etime'
    },
    template: "<strong>{{stime}}</strong>",
    link: function(scope, elem, attrs) {

      scope.stime = epochParser(scope.etime, 'time');

      function prependZero(param) {
        if (String(param).length < 2) {
          return "0" + String(param);
        }
        return param;
      }

      function epochParser(val, opType) {
        if (val === null) {
          return "00:00";
        } else {
          var meridian = ['AM', 'PM'];

          if (opType === 'time') {
            var hours = parseInt(val / 3600);
            var minutes = (val / 60) % 60;
            var hoursRes = hours > 12 ? (hours - 12) : hours;

            var currentMeridian = meridian[parseInt(hours / 12)];

            return (prependZero(hoursRes) + ":" + prependZero(minutes) + " " + currentMeridian);
          }
        }
      }

      scope.$watch('etime', function(newValue, oldValue) {
        scope.stime = epochParser(scope.etime, 'time');
      });

    }
  };
})
````

*[Back to top](#description)*

### standard-time-no-meridian
#### Description
Converts **Epoch Time** to **HH:MM** 24-hour format

[Demo](http://plnkr.co/edit/jT0uZ5ULIIRZDnBwaUfj)


##### Controller
````javascript
$scope.epochTime = 12600;
````
 
##### HTML
````html
<standard-time-no-meridian etime='epochTime'></standard-time-no-meridian>
````

##### Directive
````javascript
.directive('standardTimeNoMeridian', function() {
  return {
    restrict: 'AE',
    replace: true,
    scope: {
      etime: '=etime'
    },
    template: "<strong>{{stime}}</strong>",
    link: function(scope, elem, attrs) {

      scope.stime = epochParser(scope.etime, 'time');

      function prependZero(param) {
        if (String(param).length < 2) {
          return "0" + String(param);
        }
        return param;
      }

      function epochParser(val, opType) {
        if (val === null) {
          return "00:00";
        } else {
          if (opType === 'time') {
            var hours = parseInt(val / 3600);
            var minutes = (val / 60) % 60;

            return (prependZero(hours) + ":" + prependZero(minutes));
          }
        }
      }

      scope.$watch('etime', function(newValue, oldValue) {
        scope.stime = epochParser(scope.etime, 'time');
      });

    }
  };
})
````

*[Back to top](#description)*

### date-string-to-epoch
#### Description
Converts **string** format to **Epoch Time**

[Demo](http://plnkr.co/edit/wuiEuFCkeF8mZmZj11G2)

##### Controller
In you controller if you have a date string in the below format, then you can use this directive.

````javascript
$scope.dateString = 'Jan 1, 2015';
````


##### HTML
````html
<span date-string-to-epoch='{{dateString}}'></span>
````

##### Directive


````javascript
.directive('dateStringToEpoch', function() {
  return {
    scope: {
      ipDate: '@dateStringToEpoch'
    },
    restrict : 'A',
    template: '<strong>{{stringDate}}</strong>',
    link: function(scope, elem, attrs) {
      var inputString = scope.ipDate;

      var arr = inputString.split(/[ ,]/g);
      arr = ["Jan", "1", "", "2015"];
      arr.splice(2, 1);

      var monthArr = ['Jan', 'Feb', 'March', 'April', 'May', 'June', 'July', 'Aug', 'Sept', 'Oct', 'Nov', 'Dec'];
      var index = monthArr.indexOf(arr[0]);
      var epochTime = new Date(arr[2], index, arr[1]);

      scope.stringDate = +epochTime;
    }
  }
});
````

*[Back to top](#description)*

### ionic-time-picker
#### Description
It was initially implemented as a directive and later converted to a **bower component**.

You can check out the bower component [here](https://github.com/rajeshwarpatlolla/ionic-timepicker)

It works in Ionic frameworks applications.

[Demo](http://rajeshwarpatlolla.github.io/TimePickerForIonicFramework/demo)

##### Controller
````javascript
$scope.epochTime = 12600;
 ````
 
##### HTML
````html
<ionic-time-picker etime="epochTime" format="12" step="15">    
    {{epochTime}}
</ionic-time-picker>
````

##### Directive
````javascript
.directive('ionicTimePicker', function($ionicPopup) {
  return {
    restrict: 'AE',
    replace: true,
    scope: {
      etime: '=etime',
      format: '=format',
      step: '=step'
    },
    link: function(scope, element, attrs) {

      element.on("click", function() {

        var obj = {
          epochTime: scope.etime,
          step: scope.step,
          format: scope.format
        };

        scope.time = {
          hours: 0,
          minutes: 0,
          meridian: ""
        };

        var objDate = new Date(obj.epochTime * 1000); // Epoch time in milliseconds.

        scope.increaseHours = function() {
          if (obj.format == 12) {
            if (scope.time.hours != 12) {
              scope.time.hours += 1;
            } else {
              scope.time.hours = 1;
            }
          }
          if (obj.format == 24) {
            if (scope.time.hours != 23) {
              scope.time.hours += 1;
            } else {
              scope.time.hours = 0;
            }
          }
        };

        scope.decreaseHours = function() {
          if (obj.format == 12) {
            if (scope.time.hours > 1) {
              scope.time.hours -= 1;
            } else {
              scope.time.hours = 12;
            }
          }
          if (obj.format == 24) {
            if (scope.time.hours > 0) {
              scope.time.hours -= 1;
            } else {
              scope.time.hours = 23;
            }
          }
        };

        scope.increaseMinutes = function() {
          if (scope.time.minutes != (60 - obj.step)) {
            scope.time.minutes += obj.step;
          } else {
            scope.time.minutes = 0;
          }
        };

        scope.decreaseMinutes = function() {
          if (scope.time.minutes != 0) {
            scope.time.minutes -= obj.step;
          } else {
            scope.time.minutes = 60 - obj.step;
          }
        };

        if (obj.format == 12) {

          scope.time.meridian = (objDate.getUTCHours() >= 12) ? "PM" : "AM";
          scope.time.hours = (objDate.getUTCHours() > 12) ? ((objDate.getUTCHours() - 12)) : (objDate.getUTCHours());
          scope.time.minutes = (objDate.getUTCMinutes());

          if (scope.time.hours == 0 && scope.time.meridian == "AM") {
            scope.time.hours = 12;
          }

          scope.changeMeridian = function() {
            scope.time.meridian = (scope.time.meridian === "AM") ? "PM" : "AM";
          };

          $ionicPopup.show({
            templateUrl: '../templates/time-picker-12-hour.html',
            title: '<strong>12-Hour Format</strong>',
            subTitle: '',
            scope: scope,
            buttons: [{
              text: 'Cancel'
            }, {
              text: 'Set',
              type: 'button-positive',
              onTap: function(e) {

                scope.loadingContent = true;

                var totalSec = 0;

                if (scope.time.hours != 12) {
                  totalSec = (scope.time.hours * 60 * 60) + (scope.time.minutes * 60);
                } else {
                  totalSec = scope.time.minutes * 60;
                }

                if (scope.time.meridian === "AM") {
                  totalSec += 0;
                } else if (scope.time.meridian === "PM") {
                  totalSec += 43200;
                }
                scope.etime = totalSec;
              }
            }]
          })

        }

        if (obj.format == 24) {

          scope.time.hours = (objDate.getUTCHours());
          scope.time.minutes = (objDate.getUTCMinutes());

          $ionicPopup.show({
            templateUrl: '../templates/time-picker-24-hour.html',
            title: '<strong>24-Hour Format</strong>',
            subTitle: '',
            scope: scope,
            buttons: [{
              text: 'Cancel'
            }, {
              text: 'Set',
              type: 'button-positive',
              onTap: function(e) {

                scope.loadingContent = true;

                var totalSec = 0;

                if (scope.time.hours != 24) {
                  totalSec = (scope.time.hours * 60 * 60) + (scope.time.minutes * 60);
                } else {
                  totalSec = scope.time.minutes * 60;
                }
                scope.etime = totalSec;
              }
            }]
          })
        }
      });
    }
  };
});
````

*[Back to top](#description)*

### tooltip-follow-cursor
#### Description
A direcive which makes a tooltip follows the cursor.


[Demo](http://plnkr.co/edit/8p02DDId5ysgRnkgoSBT?p=preview)
 
##### HTML
````html
<div class="tooltipCustom" tooltip-follow-cursor>
  Hover Mouse
  <span class="tooltipCustomContent" >This is a tooltip</span>
</div>
````

##### Directive
````javascript
.directive('tooltipFollowCursor', function() {
  return {
    restrict: 'A',
    link: function(scope, element, attrs) {
      var x, y;
      element.on('mousemove', function(e) {
        x = e.pageX, y = e.pageY;
        this.children[0].style.top = (y + 15) + 'px';
        this.children[0].style.left = (x + 15) + 'px';
      });
    }
  };
})
````

*[Back to top](#description)*

### check-with-current-time
#### Description
A direcive which compares a time string(11:12 PM), with the current time.

[Demo](http://plnkr.co/edit/Ms886sg3LwD5r7O0JzLg?p=preview)
 
##### HTML
````html
<div check-with-current-time="11:12 PM"></div>
````

##### Directive
````javascript
.directive('checkWithCurrentTime', function() {
  return {
    scope: '=',
    template: '{{result}}',
    link: function(scope, element, attrs) {
      var presentHours = (new Date()).getHours();
      var presentMinutes = (new Date()).getMinutes();
      var prevTime = attrs.checkWithCurrentTime;
      var timeArray = prevTime.split(/[ :]+/);

      timeArray[0] = parseInt(timeArray[0]);
      timeArray[1] = parseInt(timeArray[1]);

      if (timeArray[2] === 'PM') {
        timeArray[0] += 12;
      }

      if (presentHours < timeArray[0]) {
        result = 'Greater than current time';
      } else {
        result = 'Less than current time';
      }

      if ((presentHours === timeArray[0]) && (presentMinutes < timeArray[1])) {
        result = 'Greater than current time';
      }

      if ((presentHours === timeArray[0]) && (presentMinutes === timeArray[1])) {
        result = 'Both are same.';
      }
      scope.result = result;
    }
  }
});
````

*[Back to top](#description)*

#### NOTE
Copyright (c) 2015 [Rajeshwar Patlolla](https://github.com/rajeshwarpatlolla)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense. 