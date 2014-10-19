# Patterns

---

## Keep acceptance criteria high-level

Bad:

    !gherkin
    Given I am a user with permission 55

Good:

    !gherkin
    Given I am user who can delete posts

Specification shouldn't need to change when implementation does.

---

## Avoid using parameters, tables, etc, in criteria

Bad:

    !gherkin
    When I login with username "ben" and password "pass"

Good:

    !gherkin
    When I login with valid credentials


Don't write a script, write a specification

---

# Specifications should be about business functionality, not software design

## Avoid writing specifications that are tightly coupled with code

---

## Never say click

Bad:

    !gherkin
    When I select "Summary" from the "Report Type" dropdown

Good:

    !gherkin
    When I choose the summary report type

Don’t get trapped in user interface details

---

## Group by application domain not actor

Bad:

- All unauthenticated user tests in one feature
- All admin user tests in one feature

Good:

- All dashboard tests in one feature

---

## Stop once you've hit an assertion

Bad:


    !gherkin
    When I go to the registration page
    And I complete the form and submit it
    Then I should see the 2nd registration page
    When I complete the form and submit it
    ...

Good:

    !gherkin
    Scenario: Registration page 1
      When I go to the registration page
      When I complete the form and submit it
      Then I should see the 2nd registration page

    Scenario: Registration page 2
      Given I'm on the 2nd registration page

Don’t explicitly set up all the dependencies in the specification
Don't create flow-like descriptions