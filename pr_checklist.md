# PR Checklist
Bugs and rework are expensive and time-consuming. The purpose of this list is to reduce such waste and increase the teams' overall velocity. Before you open
a Pull Request, check this list to ensure you haven't missed anything.

## Bugs
- has a test been added to test for the bug

## Features
- is there a feature test for the happy-path?
- are there controller tests for other actions?
- are there model tests for all public methods on the models?
- are there unit tests for all public methods on non-model classes?

## APIs
- has the documentation been updated?
- will the change impact existing users of the API?

## Data changes
- have you discussed the impact of data changes?
- do data migrations needs to be run as a result of the change?
- will DB:Slurp need to be updated after the change?

## Application-specific considerations

### MyRewards

- have you checked that the feature works across all themes?
- have you considered the impact on the API?
- have you considered the impact on GPS?
- are there any sidekiq jobs that need to be altered?

