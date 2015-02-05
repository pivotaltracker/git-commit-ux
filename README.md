# git-commit-ux

Experimentation with improved user experience for presenting git commits and their relationship to project management software, release management, and deployment management.

# Problem

* Current user experience for presenting git commit data in most software (project management, CI, chat, etc) is rudimentary.  Most tools present a basic chronological stream of commits, sometimes associated with a story (Tracker).  However, in git workflows involving multiple branches, different branch types/roles, and rebasing of branches, much potentially valueable information is missing, and other information is misleading.

# Goals

* Re-imagine and experiment with improved user experience for integrating and presenting git commit data in user interfaces and APIs, with specific focus on git workflows involving multiple branches and frequent rebasing of feature branches.
* Facilitate better integration with [Release Management](http://en.wikipedia.org/wiki/Release_management) and [Software Deployment](http://en.wikipedia.org/wiki/Software_deployment) Management through APIs which expose the relationship between git commits and associated activity in project management software (e.g. Pivotal Tracker stories)
* Provide ideas for incorporating these improved UXs and APIs into Pivotal Tracker

# Potential Features

## User Interface Features

These UI features would be facilitated by the API and Webhook features below.

* Indicate to which environments (i.e. staging, prod) a given story has commits deployed.
* Present git commit data differently depending on different usages and user roles.  For example:
  * A project manager may only want to see a confirmation that all commits have been merged to master for a given delivered story.
  * A tester may want to see all commit messages on a feature branch for a given finished story (which will be tested on the feature branch prior to merging into master).
  * A developer may want to see all commits on all branches (feature branches or topic branches off a long-lived feature branch) for in-progress started stories
* Hide orphaned commit SHA that have been rewritten or eliminated through `rebase <upstream>` or `rebase --interactive` on a feature branch (but if you REALLY wanted to, you could optionally reveal them, maybe greyed out)


## API and Webhook Features

These API and Webhook features are specific to Pivotal Tracker, and would be implemented as extensions or additions to the [Pivotal Tracker API](https://www.pivotaltracker.com/help/api)

### Release Management

* API endpoint to indicate which branches contain unmerged commits for a given Tracker story.  I.e., if all commits for a given story have been merged from a feature branch(es) into master, then a finished story is ready to deliver.
* API endpoint to indicate which stories are included in a given release.  I.e., all accepted stories whose commits are all included in a prod release branch.
* API endpoint to indicate if un-accepted stories are about to be released.  E.g., if any (un-reverted) commits for any un-accepted stories exist in a prod release branch.

### Deployment Management

* Webhook to indicate which stories are delivered to which environments.  I.e. after a successful deployment, POST to the webhook with the environment name (e.g. 'staging') and the git SHA (HEAD) of the branch which was deployed.
* Automatic delivery of finished stories (using data from the above webhook)
* API endpoint (using data from above webhook) to indicate which stories are delivered to a given environment.  This would also include story states (to easily identify unfinished, undelivered, or unaccepted stories which still have commits deployed to the given environment)

# References

* Pivotal Tracker's current github commit integration: https://www.pivotaltracker.com/help/api?version=v5#Tracker_Updates_in_SCM_Post_Commit_Hooks
* GitHub's push event webhook JSON payload: https://developer.github.com/v3/activity/events/types/#pushevent
* GitHub's commit object JSON: https://developer.github.com/v3/git/commits/
* More information on rebase-based git workflows: https://github.com/thewoolleyman/gitrflow
* Release Management: http://en.wikipedia.org/wiki/Release_management
* Software Deployment: http://en.wikipedia.org/wiki/Software_deployment
