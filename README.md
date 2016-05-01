# intellij-ACA-coding-guidelines-inspections

This repository contains a number of inspections defined to support the [ACA-IT solutions Collective Pod](https://collectiv.aca-it.be/en) coding guidelines.

## Importing

To import the settings go to Preferences - editor - inspections. 
Click on the manage dropdown next to the profile dropdown - select import - select the ACA-Guidelines.xml file. 
The ACA Guidelines profile can now be selected from the profile dropdown.

## Exporting

Select the ACA Guidelines profile in the profile dropdown.
Select export in the manage dropdown.

## Contributing

Contributing can be done by creating pull requests to the develop branch

## Overview of inspections

**DISCLAIMER:** javascript support in inspections is still very limited at the moment. A bug has been logged which describes these limitations: [link](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FIDEA-154183). Inspections marked with "limited" are impacted by this bug. This makes inspections in Javascript not very usable at the moment.

### Search

Only searches for patterns and marks them.

#### Javascript

##### string constant

Searches for string variables who's name is defined in camelcase instead of as a constant.

##### array constant

Searches for array variables who's name is defined in camelcase instead of as a constant.

### Search and replace

Searches for patterns and allows replacement of them.

##### Javascript

###### inject without minification (limited)

looks for "inject" syntax which looks like this:

	inject(['Address', function(Address) {
		address = Address;
	}]);
	
and replaces it with this:

	inject(function (Address) {
		address = Address;
	});
	
Limited because it cannot handle multiple statements inside the function body.

###### inject with underscores in param (very limited)

looks for "inject" syntax which looks like this:

	inject(function (Address) {
		address = Address;
	});
	
and replaces it with this:

	inject(function (_Address_) {
		address = _Address_;
	});
	
Limited because it cannot handle multiple parameters in the function or multiple statements inside the function body.

###### controller non-anonymous function (very limited)

looks for anonymous controller functions like this:

	angular.controller('MyController', ['ErrorContainer', function (ErrorContainer) {
		ErrorContainer.reset();
	}]);
	
and replaces them with this:

	angular.controller('MyController', ['ErrorContainer', MyController]);

	function MyController(ErrorContainer) {
		ErrorContainer.reset();
	}

Very limited because it can only handle "angular.controller" calls without any function calls in between (like "module" for example), so not very usable for the moment, since all our controllers are defined in separate modules. Also limited because multiple statements are not allowed in the function body.

###### convert mock to jasmine spy (limited)

looks for mock objects and converts them to jasmine spy objects

	var errorContainerMock = {
        noError: function() {
        }
    };
    
becomes

	jasmine.createSpyObj('errorContainerMock', ['noError']);
	
Limited because mocking of multiple functions or behaviour is not possible yet

#### Java

###### assertEquals to assertJ

Looks for any usage of assertEquals and replaces it with an assertThat().isEqualTo() clause

###### assertSame to assertJ

Looks for any usage of assertSame and replaces it with an assertThat().isSameAs() clause

###### assertNull to assertJ

Looks for any usage of assertNull and replaces it with an assertThat().isNull() clause

###### assertNotNull to assertJ

Looks for any usage of assertNotNull and replaces it with an assertThat().isNotNull() clause

###### assertTrue to assertJ

Looks for any usage of assertTrue and replaces it with an assertThat().isTrue() clause

###### assertFalse to assertJ

Looks for any usage of assertFalse and replaces it with an assertThat().isFalse() clause





