---
layout: post
title: 'Updating github tokens - a not so secret document about secrets in github'
tags:
  - security
  - github
  - secrets
  - documentation
  - howto
image: /images/github_tokens/secrets_and_variables.png
---

As it happens I am managing and organization on github which has been both easy and sometimes tricky. Many a times it has been tricky due to the way github decided to manage projects and project related settings.

Github provides a good way to start an opensource project by providing a set of services such a project needs - all in one place. Although, there are gaps in the user experience and documentation. A lot of this becomes more painful as you try to manage projects from within github. Github actions get even trickier to implement because of security and access protections and concerns.

I learnt the hard way how to do some of the things while using github and here is an attempt to write some of this as documentation so that it helps someone trying to get work done.

## Security for github actions

Github provides you two ways to be able to trigger and run the github actioins - using a personal access token or creating a github app. Now creating a github app seems to be what they would like to nudge people to do - but the steps involved are so many and the process so complex that we choose to stick with the access token.
If you are trying to leverage github actions to build a unique and useful workflow github apps would be the way to go. This is how the apps in the github marketplace are built.

Given that all we needed to do is run a set of available github actions under some user account, we choose to use the personal access tokens for our case.
Github actions are run on behalf of a user account on github. Any such  user account can be used to generate a personal access token and this token can be used as a secret for running github actions. This personal access token is secure because it is generated once and never shown on the screen. This token can be downloaded and used by the owner as they choose.
Github got this part right - the security and secrecy of this token should be left to the humans - and it relies on the human to make sure that this token is copied and used appropriately instead of doing some magic behind the scenes. If the owner loses the token or it is compromised github provides a UI to re-generate the token as well. Re-generation will cause the previous token to become invalid - another good security feature.


We use a personal access token which is owned by a team member to ensure that this secret is owned and updated by this person and becomes a responsibility (a physical key instead of something controlled by an algorithm/machine).


## The Missing documentation

You can find the documentation about the steps to create a personal access token - 
[Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).
Although, it is not documented how to use this in practice. I will walk you through the process below.

### Generate the token
Personal access tokens can be created by one of the maintainers on the project/organization. If you are the maintainer, go to `Settings` and find `Developer Settings` as shown here.

<img src="{{ root_url }}/images/github_tokens/developer_settings.png" alt="Developer Settings"/>

Find the personal access tokens section and the Tokens(classic) under it
<img src="{{ root_url }}/images/github_tokens/persona_access_tokens.png" alt="Personal access tokens"/>
<img src="{{ root_url }}/images/github_tokens/tokens_classic.png" alt="tokens classic"/>
<img src="{{ root_url }}/images/github_tokens/tokens_page.png" alt="tokens page"/>
<img src="{{ root_url }}/images/github_tokens/tokens_expiration.png" alt="tokens expiration"/>
<img src="{{ root_url }}/images/github_tokens/tokens_expiration_custom.png" alt="tokens expiration custom"/>

Generate a new token
<img src="{{ root_url }}/images/github_tokens/generate_token.png" alt="generate new token"/>
(remember to choose the classic token)

Select the appropriate permissions for your project and then generate token.
<img src="{{ root_url }}/images/github_tokens/generate_new_token.png" alt="generate token"/>

`!Important!` Remember to copy the token to your clipboard and save it somewhere safe.
<img src="{{ root_url }}/images/github_tokens/copy_token.png" alt="copy token"/>

This concludes the part of the process where you have a token that you can use for the automations.

### Use the Token