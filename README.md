# project_repository

What's the difference between the Bitbucket Project Repo and Acquia's Repo
The Mediacurrent Bitbucket repo contains only project specific code and uses the project's composer.json file to manage all of the external dependencies, including Drupal core itself. The Acquia Project repository represents the complete project build including all external libraries, packages, Drupal core and Drupal contrib modules.

Acquia Project Repo
Acquia hosts the project codebase as a remote git repository. To get starting working with the Acquia repository, follow steps 1 and 2 from this Acquia Site Factory Preparing for the PAAS workflow documentation page.

You can find the project's git URL from the Acquia Application page: https://cloud.acquia.com/app/develop/applications/22d5a35d-ccca-4a7c-97e4-8abf9514d6e3 . Click the App Info icon to view the application information.

Once you have prepared your local environment, you're ready to develop.

Developing your Codebase
When developing new feature our applying security or version releases of Drupal core or contributed modules, you can reference the Acquia Site Factory Developing your Codebase documentation page as well as the project's repo README.md file, particularly regarding which code (installation profiles, custom modules, features, etc.) lives where.

Releasing new Code
At some point, you will want to deploy new code/features into the production environment. To do that, you'll need to following guidelines below to complete the following steps. See Acquia Site Factory Testing your feature branches documentation page for more information (note, the process defined here is different than the Acquia documentation page.)

To do that, you will first need to create a separate Git release branch from develop branch to contain all of the production ready code. Start a Release Branch.
Prepare your staging environment with copies of a sample of your production websites for smoke testing. Deploy staging environment
Deploy your new release branch to the staging environment to update the sample of websites. Update Code
Create a release branch
Below are some practices to manage which features and their code make into the next release. Skip to Start a Release Brach to get started.

Using JIRA Releases
In order to ensure the most transparent, efficient, and well-organized integration between JIRA and Bitbucket, we will utilize JIRA release "versions" to group and keep track off all tickets which are included in each code release. A single release version should be opened at the start of a new sprint, by default incrementing the minor number from the previous release. For example: if the previous release was 5.3.0, the next release version created should by 5.4.0. Note: This release version title can be updated at any time if needed. The project PM/CSM can create new release versions at the same time as opening new sprints, for ease and consistency, and close them when deployed to production. Tickets can be tagged with the appropriate release version by using the "Fix Version" field inside a JIRA ticket.

A list of release versions can be viewed by clicking on the appropriate sidebar icon while in JIRA (the icon looks like a ship on the sea) or by navigating to the Project Settings (use the gear icon in the sidebar) and to "Versions" in the left nav. By choosing to "Manage Versions" here, users can create new release versions with some or all of the following information:

Name (Release number)
Description (Brief overview of updates included)
Start Date (When the sprint began)
Release Date (When the release went to production)
Users can also "Release" (mark closed) each version here once deployed to production, "Unrelease" a version if additional tickets need to be added or changes are made, Archive, and Delete. Please exercise extreme caution to only use the Archive and Delete options if necessary.

Release versions should be created in JIRA at the start of each sprint, or immediately after the previous release has deployed to production, if outside of a standard sprint workflow. Tickets will be assigned pending dev team forecast and client approval. We should limit releases to only as many tickets as can be completed within a set timeframe (example: 2 week sprint or calendar month). Please note that this process is a work in progress and may be edited, from time to time, based on what works best in practice.

Pull Requests
Ensure all Feature Pull Requests have been approved and merged into the develop branch. Ensure all JIRA issues in current JIRA version have been UAT approved for release. If a JIRA Issues has been removed from the current release, do not merge any associated pull requests. This will keep any associated code changes out of the next release and prevent these changes from making into production.

Start a Release Branch
With Git Flow initialized, use it to start a release using the next release version number, see versioning section. Use command $ git flow release start major.minor.0. Use a JIRA Issue to track status of the release branch.

Start Release
$ git flow release start major.minor.0.
Push to remotes
If your origin is Bitbucket: git push --set-upstream origin release/major.minor.0
Acquia: git push acquia release/major.minor.0:release
Pushing current release to a remote Acquia branch named release
Note, ensure a previous Pantheon remote release branch does not exist. All previous release branches should be removed when finishing a release.
Create Pantheon Release mult-dev environment
Clone live database and files
Run update.php
Deploy Staging Environment
During the release, you will sign in to the Production Acquia Site Factory Console, using an account that's been assigned either the developer or release engineer role, to deploy a sample of the production sites ( 1 of each site type ) to the test Site Factory environment. In the admin menu, click Administration, and then click the Deploy staging environment link. See https://www.pwcacsf.acsitefactory.com/admin/gardens/staging/deploy  and screenshot below. Once this process is complete, you can proceed to Update Code step below.

image2017-3-27_22-37-33.png

Update Code
Once the sample of production sites have been deployed to the test environment, you can now sign into the test Site Factory environment, using an account that's been assigned the release engineer role, to update the code. In the admin menu, click Administration, and then click the Update code link. See https://www.test-pwcacsf.acsitefactory.com/admin/gardens/site-update/update  and screenshot below. Follow the onscreen instructions and linked documentation page to update the code.

Select immediately or scheduled a time.
Select the Release branch from the "Select from the options below to update the sites in this Site Factory"
You will almost always choose Code and databases update option.
Click Update.
Smoke Testing
Once the Test environment code has been updated, you can now review the sample of copied production sites with the latest code changes in the Release branch.

Bugfixes
When an issue identified during a release review needs code changes to resolve the issue, we need to use Bugfix branches created from and merged into the Release branch. These bugfix branches are reviewed, approved and merged via a a pull request. Once the issue has been merged into the Bitbucket remote release branch, the project lead needs to grab these changes and push them to the Acquia remote release branch for additional client testing and review. To review, repeat the Deploy Staging Environment and Update Code steps above to ensure the latest copies of a sample of production sites are updated with the code in the Release branch.

Finish Release
When Smoke Testing is complete and the release is approved for deployment, finish the release branch, tag the version and push to the remotes.

On your local, finish release and push master, integration and tags to both Bitbucket and Pantheon remotes.
$ git checkout release/major.minor.0
$ git flow release finish major.minor.0
This will merge the release branch, and any added bugfix code, into integration and master branches and tag the release.
When prompted, save each merge commit and leave tag message.
Note, when prompted, leave a tag message indicating which features, tasks, fixes are included in the tagged version.
$ git push origin develop
$ git push origin master
$ git push origin --tags
$ git push origin :release
$ git push acquia develop
$ git push acquia master
$ git push acquia --tags
$ git push acquia :release
Deploy Release
After we've finished the release, we can log into the production Site Factory environment, using an account that has been assigned the release engineer role, to Update Code. In the admin menu, click Administration, and then click the Update code link. See https://www.pwcacsf.acsitefactory.com/admin/gardens/site-update/update  and screenshot below. Follow the onscreen instructions and linked documentation page to update the code.

Select immediately or scheduled a time.
Select the Release branch from the "Select from the options below to update the sites in this Site Factory"
You will almost always choose Code and databases update option.
Click Update.
Managing Hotfixes
Use hotfix workflow to deploy changes quickly to production and bypass/exclude any changes within the develop branch.. This can be a change to quickly remedy an issue found shortly after a release. It can also be a change that needs be deployed without any other changes already included in the develop branch, and as such can leap frog code changes in develop.

Start Hotfix
With Git Flow initialized, use it to start a release using the next tag version number, major.minor.hotfix. See versioning section.

$ git flow hotfix start major.minor.hotfix.
Dev work completed on hotfix branch.
Dev pushes hotfix branch to Bitbucket for PR creation and review.
$ git push --set-upstream origin hotfix/major.minor.hotfix
Review Hotfix
Once code review is completed and PR is approved, but not yet merged, the changes should be verified by dev team and client before merging into master.

Push to Acquia for review
$ git push acquia hotfix/major.minor.hotfix:hotfix
Follow the Deploy Staging Environment and Update Code steps above to ensure the latest copies of a sample of production sites are updated with the code in the hotfix branch.
When Update Code in the Test Site Factory Environment, use the hotfix branch to deploy this code.
QA and UAT
Both QA and UAT can occur in the test site factory environment, which is using the code in the hotfix branch.

Finish Hotfix
Once UAT Approved, merge the Bitbucket PR and finish the hotfix branch.

On your local, finish release and push master, integration and tags to both Bitbucket and Acquia remotes.

$ git checkout hotfix/major.minor.hotfix
$ git flow hotfix finish major.minor.hotfix
This will merge the hotfix branch, and any added bugfix code, into integration and master branches and tag the hotfix.
When prompted, save each merge commit and leave tag message.
Note, when prompted, leave a tag message indicating which fixes are included in the tagged version.
$ git push origin develop
$ git push origin master
$ git push origin --tags
$ git push origin :hotfix
$ git push acquia develop
$ git push acquia master
$ git push acquia --tags
$ git push acquia :hotfix
Deploy Hotfix
To deploy the hotfix, now merge into master, follow the Deploy Release steps above and use the recent hotfix tag version to deploy the latest code.

Git Flow
Git Flow will help manage releases and hotfixes to ensure code 1) does not block a path to the production environment and 2) is properly merged into integration and master branches.

Initialize Git Flow if you haven't. When initializing Git Flow, ensure you have local master and integration branches before initializing.

Git Flow Initialization
Ensure you have both master and integration branches locally.
$ git branch: should reveal integration and master branches.
Use Git Flow Init to initialized Git Flow and use recommended defaults, except adding 'v' as tag prefix.
$ git flow init
Which branch should be used for bringing forth production releases?
Branch name for production releases: [master]
Which branch should be used for integration of the "next release"?
Branch name for "next release" development: [integration]
How to name your supporting branch prefixes?
Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? [] v
Versioning
Versioning Summary
For those who are well versed in versioning schema and standards for both the Drupal community and the software engineering community at large, there is only a few things to remember:

Official Releases are to use Semantic Versioning via Git tags and gitflow mechanics
Component Changes (modules, features, themes) use Drupal standard .info files and numbering conventions
Semantic Versioning
The Mediacurrent standard for versioning all releases will obey the principles and rules of semantic versioning v2.0.0. Since many of our websites do expose a public API that must be maintained, we should maintain a consistent standard for naming the component and platform releases.

Repeated for convenience here, semantic versioning is summarized as follows.

Given a version number MAJOR.MINOR.PATCH, increment the:

MAJOR version when you make incompatible API changes,
MINOR version when you add functionality in a backwards-compatible manner, and
PATCH version when you make backwards-compatible bug fixes.
Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.
Example: Given version 1.23.57 the following is true:

Major version: 1
Minor version: 23
Patch version: 57
If you want more detailed rules, the semver site has more documentation.
Drupal Component Versioning
The Drupal community standard does not allow us to follow the symantic versioning standard for modules, themes, features and anything else with a .info file. In Drupal standards, the major version remains intact; but, we have lost the minor version indicator. Based on this, we have no indication of whether new features are included in a release. This is unfortunate; however, it does simplify the issue a little: increment the patch version for EVERY release of a module/feature/theme regardless of the nature of the change. Major versions are reserved, as in semantic versioning, for non-backwards-compatible API changes in a module/theme/etc.

Example: Given Drupal version number 7.x-3.1 the following is true:

Drupal core version: 7
Component major version: 3
Component patch version: 1
Where to Use Versions
The Git tag will denote the "Platform Version" of the website. This semantic version number will reflect any change for the whole site, regardless of origin.

"Component Version" numbers will be utilized in any case where a Drupal *.info file may be used to denote versions of internal components. The same semantic versioning rules apply.

When to Change Versions
In summary: every time you create a release or make a change.

Platform Versioning
The platform version number for the site must be incremented on every public release without exception. If any new feature is added, regardless of where, the minor version must be incremented. If the release is a simple bug fix, the patch version must be incremented.

Component Versioning
Regardless of how miniscule a change may be, every time a change is made to any component that supports using a Drupal .info file, the version number will be incremented appropriately.

How to record version numbers
Version numbers are stored in Git tags and .info files.

Platform Version Number in Git
Mediacurrent is standardized on gitflow, this makes it easy to manage releases and, in the process, create version tag for the platform release.

The release process is normally managed using the git flow release start [name] command. To comply with the Mediacurrent versioning policy, the name of any release must be the platform version number for the website. This release name becomes the git tag ultimately used to version and deploy the system.

In there rare cases where a hotfix is required, there is a different gitflow hotfix command; however, the same rule applies for these releases: the name of the release is the platform version number.

Component Version Number in Git
Smaller components of the site (such as modules, features, themes, etc.) are versioned using the Drupal standard .info files. These files are checked into Git during the course of development. It is necessary to increment the version for these components only once per expected release. Typically, the next version number is obvious based on what is in production at the time versus what is being built.

We do not expect development versions to be separately tracked as a standard practice.
