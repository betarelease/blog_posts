---

layout: post title: Mingle Loves GitHub

tags: [mingle, github, integration, push, commits, murmurs]
-----------------------------------------------------------

Mingle has a new look and feel, if you have not seen it already. In improving the experience we have also been looking at improving how Mingle integrates with the world.

One of features that was missing on the Mingle Cloud version was integration with code. Given the popularity and market penetration it made sense to integrate with GitHub. Also GitHub's openness to promote open source software collaboration made it a clear first choice.

GitHub's API for interacting with repositories gave us an easy way to implement this. We have recently built and released GitHub integration for Mingle(soft woohoo!) In designing this feature we also thought about how people would use it with Mingle and how it would enhance their experience of being part of the team.

How it works
------------

GitHub exposes a number of ways to integrate with itself. All that the user is required to do is setup a webhook for GitHub to be able to integrate with your application. More information on webhooks and how to set them up is on (github docs)[https://developer.github.com/webhooks/].

Choose between webhook and service
----------------------------------

GitHub allows users to build a webservice hook - called service - to integrate their apps. GitHub also allows users to connect with their repositories with simple configuration - called webhook.

We chose to go the webhook way because it was really easy to work with and required no code development and publishing on our part. We were able to leverage the simple webhook configuration screen to configure a webhook programmatically.

Connect with Mingle
-------------------

To connect your GitHub repository to Mingle all you need is your username/repository name and an API token to interact with GitHub API. A simple form connects to GitHub using this information. Once it is connected GitHub sends notifications about interesting events to Mingle. Currently we have only configured it to send ‘push’ events. So any code that is pushed upstream to master branch of the repository is published to Mingle via Murmurs.

You should be able to see the check-in information on Mingle Murmurs.

Using it for Profit
-------------------

If you really want to make the most of this feature you need not stop at looking at the commits in Mingle Murmurs. If you follow a certain etiquette around your commits - you can leverage a little bit more of Mingle. Once you prepend your commits with project_name/#card_number, these commits will be linked to the appropriate card on Mingle

We also built another feature that makes it really easy to link a card with your commit. We call this feature (Clippy)[http://getmingle.io/news/2014/04/18/Clippy.html].

More integration with GitHub issues, deployment, etc. are forthcoming. Watch this space or our blog at (getmingle.io)[http://getmingle.io] or follow us on twitter (@thatsmingle)[http://twitter.com/thatsmingle]
