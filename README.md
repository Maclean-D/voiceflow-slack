# voiceflow-slack

![voiceflow-slack](https://github.com/Maclean-D/voiceflow-slack/raw/main/voiceflow-slack.png)

Connect any Voiceflow agent to a Slack App via the dialog API

## Features

- Connect to any Voiceflow Agent
- Supports messages, buttons, and images (when sent individually)
- Passes the user's full Slack name to Voiceflow

<img width="816" height="384" alt="Demo" src="https://github.com/user-attachments/assets/cc6a7a35-0745-4f06-8ed8-ff83ab96c28d" />

## Setup

### Create your Slack App

1. [Create an app](https://api.slack.com/apps?new_app=1)
2. Select `From a manifest`
3. Select the workspace you want to use your app in
4. Replace the JSON with:

```json
{
    "display_information": {
        "name": "Voiceflow Slack Demo",
        "description": "Slack Bot using Voiceflow's Dialog Manager API",
        "background_color": "#37393d"
    },
    "features": {
        "app_home": {
            "home_tab_enabled": true,
            "messages_tab_enabled": true,
            "messages_tab_read_only_enabled": false
        },
        "bot_user": {
            "display_name": "Voiceflow Demo",
            "always_online": true
        }
    },
    "oauth_config": {
        "scopes": {
            "user": [
                "users:read"
            ],
            "bot": [
                "app_mentions:read",
                "channels:history",
                "chat:write",
                "im:history",
                "im:read",
                "im:write",
                "mpim:history",
                "mpim:read",
                "mpim:write",
                "users.profile:read",
                "users:read"
            ]
        }
    },
    "settings": {
        "event_subscriptions": {
            "user_events": [
                "message.app_home",
                "user_change"
            ],
            "bot_events": [
                "app_home_opened",
                "app_mention",
                "message.channels",
                "message.im",
                "message.mpim",
                "user_change"
            ]
        },
        "interactivity": {
            "is_enabled": true
        },
        "org_deploy_enabled": false,
        "socket_mode_enabled": true,
        "token_rotation_enabled": false
    }
}
```

5. Click `Next` then `Create`
6. Navigate to the `Install App` tab on the left
7. Click `Install`
8. Click `Allow`

### Obtain Slack Keys and Tokens

#### Slack Signing Secret

1. Navigate to the `Basic Information` tab on the left
2. Copy the `Signing Secret` and save it somewhere for later

#### Slack App Token

1. Scroll down and click `Generate Token and Scopes`
2. Name the token `Voiceflow Token`
3. Click `Add Scope` and select `connections:write`
4. Click Generate
5. Copy the app token and save it somewhere for later

#### Slack Bot Token

1. Navigate to the `OAuth & Permissions tab` on the left
2. Copy the `Bot User OAuth Token` and save it somewhere for later

### Obtain Voiceflow Keys and Tokens

- [Open a Voiceflow Agent](https://creator.voiceflow.com/workspace/)
- Make sure it works and that you've Published it (Green publish button in the top right)

> [!Tip]  
> Slack's formatting works different for apps then users, consider adding [this](doc/prompts/prompt.md) to your agent's prompt

> [!NOTE]  
> Ensure that your Agent has an end function. (Logic > End). Anytime you edit your agent you will need to publish it, and start a new chat for the changes to take effect.

- Open `Settings` by pressing `7` or clicking the cogwheel on the left

#### Voiceflow API Key

1. Navigate to the `API Keys` tab on the left
2. Copy your `Primary key` and save it somewhere for later

#### Project and Version ID

1. Navigate to the `General` tab on the left
2. Copy the `Project Key` and save it somewhere for later
3. Copy the `Version ID` and save it somewhere for later

### Create your Replit app

1. Click `Run on Replit` below

[![Run on Repl.it](https://replit.com/badge/github/Maclean-D/voiceflow-slack)](https://replit.com/new/github/Maclean-D/voiceflow-slack)

2. Click `Import from  GitHub`
3. A `Configure Repl` window should have opened, scroll down and click `Confirm and Close`
4. Press the Search button on the top right of the screen
5. Search for `Secrets` and click the first option
6. Click `Edit as JSON` in the bottom left of the secrets tab and replace the content in it with:

```json
{
  "VOICEFLOW_API_KEY":"VF.12345678",
  "VOICEFLOW_VERSION_ID":"12345678",
  "VOICEFLOW_PROJECT_ID":"12345678",
  "VOICEFLOW_RUNTIME_ENDPOINT":"general-runtime.voiceflow.com",
  "SLACK_APP_TOKEN":"xapp-XXXXX",
  "SLACK_BOT_TOKEN":"xoxb-",
  "SLACK_SIGNING_SECRET":"XXXXXXX"
}
```

> [!IMPORTANT]  
> Switch out all of the placeholders with the keys and tokens you saved for later earlier

7. Press `Save`
8. Click `Deploy` in the top right
9. Make sure `Reserved VM` is selected
10. Click `Set up your deployment`
11. Under App Type select `Background Worker`
12. Press the `Deploy` button at the bottom

## Using your Slack App

### Via DM

1. In Slack search for 'Voiceflow Slack Demo' or the app name you created earlier.
2. Navigate to the Messages tab from the app's page

### In a Channel

> [!IMPORTANT]  
> Your app will respond to every message in channels it's added to

1. Open the Slack channel you want to add it to
2. @ the bot in a message and attempt to send it
3. Slack will prompt you `Want to add this person instead?`, press `Add to Channel`

### Custom commands

- Send `hi` to not pass a message to Voiceflow, and have it generate a response.
- @ the bot in a message to check if the server is online, regardless of being hooked up to Voiceflow

## Troubleshooting

### Images Are Not Appearing

Images are only supported when sent via the image block in voiceflow, not from an agent message.

### Error. Please try again a bit later

This usually happens when your Replit secrets are entered wrong, or a message broke the voiceflow loging (i.e. a message over 4,000 characters was sent). Once you're sure your secrets are entered correctly:

1. Create a new channel
2. Add your app to the channel
3. Sending a message should break the `Error. Please try again a bit later` loop.

### Can't end chat

If you forgot to add an end chat flow, unfortunately the easiest workaround is to:

1. Duplicate your agent in Voiceflow
2. Change the secrets in Replit to match the new agent
3. Redeploy Replit
4. Create a new channel
5. Add your app to the channel
6. Send a message there to fix the logging error.

### Using an outdated version

If the slack app is using an older published version of your voiceflow agent, first try ending the chat and creating a new one. If that doesn't fix the issue, try removing the `VOICEFLOW_VERSION_ID` secret from Replit.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Maclean-D/voiceflow-slack&type=Date)](https://star-history.com/#Maclean-D/voiceflow-slack&Date)

## Contributors

<a href="https://github.com/Maclean-D/voiceflow-slack/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=Maclean-D/voiceflow-slack" />
</a>
