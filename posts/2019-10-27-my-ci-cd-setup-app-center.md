---
title: My CI/CD Setup (App Center)
permalink: my-ci-cd-setup-app-center-19-10-27
date: 2019-10-27 20:59
---

![app-center-CI-CD](https://server.shaneqi.com/public/storage/4ueMdgIecldg.png)

## Roles:

- Me.
- Telegram Bot is the coordinator.
- Build Configs Repo stores all the build configs.
- App Center provides the infrastructure to build and compile the project.
- App Store Connect is the destination of the flow.

## Steps

### 1. I send command to the bot

The message include which project, which branch, which commit I need this building process to happen on and which build config I want to use.

Obviously, my CI/CD isn't really **continuous** because it doesn't build on every commit, it only does when I send the command. 

My projects are iOS apps, comparing to deploy a web app, it's kind of heavy to deploy an iOS build: there is the '.ipa' file needs to be uploaded, App Store Connect needs to process every build, internal testers will receive notifications to update, and all the builds will clutter the dashboard. That's why I only want the deployment to happen based on my commands.

### 2. Bot fetches the build config

The build configs are identified by the combination of the project name and the config name.

### 3. Bot find the build config

As you can see from the example snapshot of a build config file. It defines configurations like Xcode version, provisioning profile, destination (TestFlight or App Center adhoc), etc.

### 4. Bot sends the build config to App Center

The build hasn't started yet, the bot needs to set the config first (this is how App Center API works).

### 5. Bot triggers the build

The bot triggers the build and App Center build will be configured as the build config the bot sent in the previous step.

### 6. App Center API response of build start

App Center responses to the bot if the build config was set successfully and if the build started successfully.

### 7. Bot sends a message of build start

The bot will notify me the response it gets from the App Center.

### 8. App Center builds and uploads

App Center builds the project and uploads the artifact ('.ipa' file) to the destination (App Store Connect or App Center Releases).

### 9. App Center notifies the bot thru webhook

Once the App Center finishes building and uploading, it will notify the Telegram bot through a webhook.

### 10. Bot sends a message of build result

The bot will notify me the result of the build.

