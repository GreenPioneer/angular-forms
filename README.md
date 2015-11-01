# angular-forms
This is all examples based code to help people understand more about angular form validation & submissions. Start with ex1 and work your way up to ex4 . by then end you will have a form being validated and posted to your mock backend.

## Example One - Build the basics of the form

```html
    <div class="form-group" >
        <label>Username</label>
        <input name="username" type="text" maxLength=10 class="form-control" data-ng-model="formData.username" id="username" placeholder="Username" required>
    </div>
```

## Example Two - Add Validation to the form

```html
    <div class="form-group" ng-class="{ 'has-error' : submitted && formExample.username.$invalid }">
        <label>Username</label>
        <input name="username" type="text"  ng-minlength="3" ng-maxlength="8" class="form-control" data-ng-model="formData.username" id="username" placeholder="Username" required>
        <div ng-show="submitted && formExample.username.$invalid" class="help-block">
            <p ng-show="formExample.username.$error.required">Username is required</p>
            <p ng-show="formExample.username.$error.minlength" class="help-block">Username is too short.</p>
            <p ng-show="formExample.username.$error.maxlength" class="help-block">Username is too long.</p>
        </div>
    </div>
```

## Example Three - Add $http to post to a mock backand

```javascript
    $http.post('/form',$scope.formData).
      then(function(response) {
      	//success response
        $scope.submittedData.push(response.data)
        $scope.formData = {};
        $scope.submitted = false;
        alert("The form was posted to the httpBackend")
      }, function(response) {
        //error response
      });
```

## Example Four - Add $resource to post to a mock backand

```javascript
    var Forms = $resource('/form');
	Forms.save($scope.formData, function(data){
	  	$scope.submittedData.push(data)
        $scope.formData = {};
        $scope.submitted = false;
        alert("The form was posted to the httpBackend")
	});
```

## MISC - Build Mock Backend
```javascript
    $httpBackend.whenPOST('/form').respond(function(method, url, data, headers){
        console.log('Received these data:', method, url, data, headers);
        return [200, data, {}];
    });
```

## MISC - END JS SOLUTION
```javascript
	var formApp = angular.module('formApp', ['ngMockE2E','ngResource']);
    // create angular controller
    formApp.controller('formController', function ($scope,$resource,$httpBackend) {
        //create variables needed  - form data object, submitted forms array
        $scope.formData = {};
        $scope.submittedData=[];      
            // create function to submit the form after all validation has occurred      
        $scope.submitForm = function (isValid) {   
            // check to make sure the form is completely valid if so - push the form being submitted - then clear that form
            // if not make scope.submitted true 
            if (!isValid) {
            	//https://docs.angularjs.org/api/ngResource/service/$resource
            	var Forms = $resource('/form');
				Forms.save($scope.formData, function(data){
				  	$scope.submittedData.push(data)
                    $scope.formData = {};
                    $scope.submitted = false;
                    alert("The form was posted to the httpBackend")
				});
            } else {
                $scope.submitted = true;
            }
        };
        //set up an angular mock backend to handle the form submission
        $httpBackend.whenPOST('/form').respond(function(method, url, data, headers){
            console.log('Received these data:', method, url, data, headers);
            return [200, data, {}];
        });
    });
```

## MISC - END HTML SOLUTION - SHORTEN
```html
	<form name="formExample" role="form" data-ng-submit="submitForm(formExample.$valid)" novalidate>
        <div class="form-group" ng-class="{ 'has-error' : submitted && formExample.name.$invalid }">
            <label>Name</label>
            <input name="name" type="text" class="form-control" data-ng-model="formData.name" id="name" placeholder="Name" required>
            <div ng-show="submitted && formExample.name.$invalid" class="help-block">
                <p ng-show="formExample.name.$error.required">Name is required</p>
            </div>
        </div>
        <!-- More inputs go here  -->
        <div class="form-group">
            <button type="submit" class="btn btn-success btn-lg btn-block">
                <span class="glyphicon glyphicon-ok"></span> Submit!
            </button>
        </div>
    </form>
```

#### This is [on GitHub](https://github.com/GreenPioneer/angular-forms)
#### Find us [on GitHub](https://github.com/GreenPioneer)
#### Find us [on Twitter](https://twitter.com/greenpioneerdev)
#### Find us [on Facebook](https://www.facebook.com/Green-Pioneer-Solutions-1023752974341910)
#### Find us [on The Web](http://greenpioneersolutions.com/)