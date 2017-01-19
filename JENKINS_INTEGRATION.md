# Prerequisites

- Admin access to the Github project
- A Github token, valid for the target Github project, with at least the following permissions set:
  - repo - all
  - admin:org - all
  - admin:public_key - all
  - admin:repo_hook - all
  - You can find your tokens at [https://github.com/settings/tokens/](https://github.com/settings/tokens/) while logged in
- Admin access to Jenkins
- The following Jenkins plugins:
  - Github
  - Github PR Builder

# Github Setup

Head to the Settings page for your target repo. Then down to "Integrations and Services".

Add a service and search for "Jenkins". You want to install "Jenkins (Github)".

You'll need to add a web hook URL. For the Bitnami Jenkins machine this is [https://jenkins-web.somo-tools.com/jenkins/github-webhook/](https://jenkins-web.somo-tools.com/jenkins/github-webhook/)

This will allow Jenkins to be aware of Github events and therefore trigger builds when we want.

# Jenkins Setup

From the main Jenkins dashboard click "New Item".

Create a "Freestyle" job and give it a sensible name.

In the project configuration screen select the "Github project" checkbox and paste the HTTPS link to your target Github repo.

Under **Source Code Management** select "Git"

Enter the same HTTPS URL you would to clone the repo in the "Repository URL" field.

Leave "Credentials" set to none.

Set "Name" to "origin"

To build PRs only set the "Refspec" to "+refs/pull/*:refs/remotes/origin/pr/*

Set the "	Branch Specifier" to "S{sah1}"

Head to the **Build Triggers** section and check the "Github Pull Request Builder" box.

Leave your Github API credentials as the default selection.

Add a Github user with admin rights to the target project in the "Admin list"

Check the "Use github hooks for build triggering" box

Click the **Trigger Setip** button at the bottom of this section.

Here you add "Build Status Messages" to that comments will be posted to the PR when the build succeeds or fails..

Also add "Update commit status during build" and in the "Commit Status Context" field enter a sensible, descriptive name for this job. You can also enter values in "Commit Status Build Triggered" and "Commit Status Build Started" to give the user feedback as this job is requested and starts up.

Finally, in the **Build** section you can add shell scripts to execute as part of this job. Exiting with a status of 1 will cause the build to fail.

