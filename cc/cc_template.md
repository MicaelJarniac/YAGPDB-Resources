# Basic CC Template

This is a template for basic custom commands.

## Setup

1. Create a new **Custom Commands** group (press <kbd><samp>+</samp></kbd>)
1. Name it `My Group` or something similar
1. For **Whitelist roles for who can use these commands**, pick the roles that should have access to the command
1. For **Channels these commands can be used in**, pick all channels where the command should run
1. Leave the other settings on their default
1. <kbd><samp>Save group settings</samp></kbd>
1. <kbd><samp>Create a new Custom Command</samp></kbd>
1. For **Trigger type** select **Reaction**, and then pick **Added reactions only**
1. For **Trigger type** select **Contains**, and leave the **Trigger** empty, to have it trigger on every message
1. On **Response** copy and paste the first command from the **Custom Command** section below
1. Ensure **Custom command group** remains the previously created group (`My Group`)
1. Disable **Output errors as command response**
1. Leave the other settings on their default
1. <kbd><samp>Save</samp></kbd>
1. Repeat those steps on the same group for the second command below
1. Test!

## Custom Command

#### The first

This is a custom command that does absolutely nothing!

```js
{{/* Basic configuration */}}

{{$setSomething   := true}}

{{$emojiFirst  := "chk_c:568993681982357525"}}
{{$emojiSecond := "chk_u:568993712508764186"}}
{{$emojiThird  := "checkmark:563166191871328276"}}

{{$roleFirst  := "First Role"}}
{{$roleSecond := "Second Role"}}
{{$roleThird  := "Third Role"}}

{{$channelFist   := "first-channel-name"}}
{{$channelSecond := "second-channel-name"}}
{{$channelThird  := "third-channel-name"}}

{{/* Cooldown configuration */}}

{{$cooldownTime := 60}}
{{$cooldownName := "myCooldown"}}

{{/* -- END OF CONFIGURATION -- */}}

{{$doSomething := true}}
```

#### The second

And this one does even less!

```js
{{/* Basic configuration */}}

{{$setSomething := true}}

{{/* -- END OF CONFIGURATION -- */}}

{{$doSomething := true}}
```

## To-do

- [x] Create a simple template
- [ ] Send it to space
  - [ ] Buy a rocket
  - [ ] Schedule the launch

## Configuration

At the beginning of each custom command, there's a section for configuring it.

`setSomething` is a boolean (`true` or `false`) that configures whether the user should wonder if the custom command does something or not.

`emojiFirst`, `emojiSecond` and `emojiThird` are emoji variables that should be changed to the desired emojis. You can do so by following [this guide](/emoji-instructions.md).

`roleFirst`, `roleSecond` and `roleThird` are role name variables that should be changed to the desired role names (inside quotes and without the `@`).

`channelFirst`, `channelSecond` and `channelThird` are channel name variables, and should be changed to the desired text channel names (inside quotes, and without the `#`).

`cooldownTime` and `cooldownName` are variables to configure the cooldown of each command. The name can be anything, and the time is how long the command will be in cooldown for.
