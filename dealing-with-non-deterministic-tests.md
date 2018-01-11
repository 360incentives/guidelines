# Dealing with Non-Deterministic Tests

Non-deterministic tests can reduce a team's confidence in the tests suite and can cause neadless delays and reduce velocity. 

## Discovering non-deterministic tests
If a test fails and subsequently passes either locally or the CI server, either individually or as part of the suite it 
may be a non-deterministic test. If it fails as part of the whole suite then it may be another test which is causing the
issue. Once you have discovered the test you should act to restore the suite to green as quick as possible

## Removing tests from the build
if the test is in master
- pull the `configuration_changes` branch
- merge in `master`
- skip the test with an easy to grep tag and add a comment with a link to the breaking build on CircleCI, e.g.:
```ruby
context 'something' do
  it 'adds a user' do
    skip "flakytest:jc-01-03-2017" 
    # https://circleci.com/gh/CorporateRewards/myrewards/9451

    ...
  end

  ...
end
```
- commit and push (fixes master on next deployment)
- merge `configuration_changes` branch into `develop` and push (fixes develop)

## Create a ticket for fixing the tests 
- open an issue to fix the flaky test(s) mentioning the greppable tag (eg flakytest:jc-01-03-2017)
- use the "technical" lable
- put the ticket in the "Planning" column
