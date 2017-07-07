# Createk's Development Pipeline

## Our approach to development

Createk currently operates in a hybridized Kanban/Scrum fashion. What that
means is that while we don't appoint values to our tickets on the Kanban
board we still operate in two week sprint blocks

During our Sprint meetings we'll:
- Start with a Retrospective where we review the previous sprint looking at
what went well, what could be better, and what actions we can take to improve
- Move tickets from our Backlog/Planning columns into Planning-Done.
- Discuss potentially problematic tickets; i.e. blockers to completion, potential
solutions
- Sometimes assign specific individuals to tickets if the tickets have a
particularly high priority; i.e. The tickets are part of a large feature with a
looming deadline where the developer has a good deal of experience



### Step 1: Selection
- Select a ticket from the top of the Planning-Done column (these are
ranked by highest priority; descending) and assign yourself and anyone
else working on it with you

### Step 2: Development
- Commit your changes. Be sure to push often to allow Circle to run your
newly added tests often to receive feedback from the test suite

### Step 3: Review
- Create a pull request. Look over the checklist and get members of the team
to review your work. If the product owner is available try to
show them the completed work to make sure it meets their expectations

### Step 4: Changes
- Make the requested changes, if there are any

### Step 5: Merge
- Merge to Staging
- Start a new ticket

### Step 6: Test
- Corporate Rewards manually tests on Staging

### Step 7: Re-work
- Potential re-work (If necessary, stop work on your newly selected ticket and
complete the requested changes then move through steps 3 to 6 again)

### Step 8: Deploy
- [See our Git workflow](http://github.com/CreatekIO/guidelines/blob/master/git_workflow.md)
