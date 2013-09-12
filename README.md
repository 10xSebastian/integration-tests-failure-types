Integration test failure types
======================
A list of integration test failure types that are difficult to reproduce or hard to fix, using capybara.

---------------------------------------

Existing but not visible elements
----------------------
```
  Element is not currently visible and so may not be interacted with (Selenium::WebDriver::Error::ElementNotVisibleError)
```
Imagin you have a dropdown, and you perform `find("#dropdown).click; find("#dropdown-item").click` then it could happen that when capybara tries to click on the item it's not yet visible. To solve this you should ensure that capybara interacts only with the visible dropdown-item not with the still hidden one `find("#dropdown-item", :visible => true).click`.

---------------------------------------

Different sequence of database entries
----------------------
```
  Called id for nil, which would mistakenly be 4
  undefined method `***' for nil:NilClass (NoMethodError)
```
Dont rely on sequence of elements in your local database. On another environment a sequence could potentially be different.

---------------------------------------

Different screen sizes
----------------------
```
   Unable to find css "***" (Capybara::ElementNotFound)
   Element is not clickable at point (XXX, XXX) 
```
Consider, not every environment has the same screen size that you have on your local machine. Try to run tests using the same screen resolution that is used by the system where the test actually is failing.

---------------------------------------

DOM element changed
----------------------
```
   Element is no longer attached to the DOM (Selenium::WebDriver::Error::StaleElementReferenceError)
```
Avoid using chained finders/selectors like `find('body').find('ul').find('li')`. Use one selector instead `find('body ul li')`. Also try not to store dom elements in a variable and then access them later, also avoid reloading them with `element.reload`, but find those elements again in the dom.

DOM elements not yet ready
----------------------
```
  first(".button").click
  undefined method `click' for nil:NilClass (NoMethodError)
```
ensure that the element is present before perform an action on it
```
  page.should have_selector(".button")
  first(".button").click
```
in alternative, when the element should be unique, the find method is wating for it
```
  find(".button").click
```
