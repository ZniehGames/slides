# CUCUMBER JS

* Shared Business Language (Gherkin)
* Clear seperation of functional testing
* Moving forward all the benefits of a proven BDD framework

---

    !gherkin
    Given I am on the login page
    When I login
    Then I should be logged in

---

    !javascript
    this.Given('I am on the login page', function () {
      browser.get('#/login');
    });

    this.When('I login', function (done) {
      var user = 'ben', pass = 'pass';
      element(by.css('.email'))
        .sendKeys(user)
        .then(function() {
          return element(by.css('.password'))
            .sendKeys(pass)
        });
        .then(function() {
          return element(by.tagName('button')).click();
        });
        .then(function() {
          done();
        });
    });

    this.Then('I should be logged in', function (done) {
      browser.getCurrentUrl().then(function (url) {
        assert.equal(url, '#/login');
        done();
      });
    });

---

# A Page object is an API for a page

“When you write tests against a web page, you need to refer to elements within that web page in order to click links and determine what's displayed.

“However, if you write tests that manipulate the HTML elements directly your tests will be brittle to changes in the UI.

“A page object wraps an HTML page, or fragment, with an application-specific API, allowing you to manipulate page elements without digging around in the HTML.”

— Martin Fowler

---

## Page objects hide page details from the step definitions

A page object wraps an HTML page, or fragment, with an application-specific API, allowing you to manipulate page elements without digging around in the HTML.

If you have WebDriver APIs in your test methods, You're Doing It Wrong.

— Simon Stewart.

---

## An example page object

    !javascript
    // instantiate
    var page = new LoginPage();
    // load the page
    page.get();
    // do things (login in this case)
    page.login('ben', 'pass');
    // query the page (check if page has validation errors)
    if (page.hasErrors()) {
      console.log('✘');
    };

---

    !javascript
    var LoginPage = function () {
     var url = '#/login',
       emailElem = element(by.css('.email')),
       passwordElem = element(by.css('.password')),
       submitElem = element(by.css('button')),
       invalidElem = element(by.css('.ng-invalid'));
     this.get = function () {
       return browser.get(url);
     };
     this.login = function (email, password) {
       emailElem.sendKeys(email);
       passwordElem.sendKeys(password);
       return submitElem.click();
     };
     this.hasErrors = function () {
       return invalidElem.count().then(function (count) {
         return count > 0;
       })
     };
    };

---

## Using page objects in step definitions

    !javascript
    this.Given('I am on the login page', function (done) {
      new LoginPage()
        .get()
        .then(function () { done(); });
    });
    this.When('I login', function (done) {
      new LoginPage()
        .login('ben', 'password')
        .then(function () { done(); });
    });
    this.Then('I should be logged in', function (done) {
      new LoginPage()
        .hasErrors().should.become(false)
        .then(function () { done(); });
    });

---

## Chai/Chai as Promised for assertions

    !javascript
    promise.should.be.fulfilled;
    promise.should.be.rejected;
    promise.should.become('foo');
    promise.should.become('foo').notify(done);

---

# Tips

## Do not reuse element()s

Bad :

    !javascript
    this.When('I click submit', function () {
      return element(by.css('.submit')).click();
    })

Good :

    !javascript
    var submitElem = element(by.css('.submit'));
    this.When('I click submit', function () {
      return submitElem.click();
    })
