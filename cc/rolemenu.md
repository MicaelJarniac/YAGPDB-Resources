# Rolemenu

This is a rolemenu that automatically removes the reactions.

## Setup

1. Create a new **Custom Commands** group (press <kbd><samp>+</samp></kbd>)
1. Name it `Rolemenu` or something similar
1. Leave the other settings on their default
1. <kbd><samp>Save group settings</samp></kbd>
1. WIP...

## Custom Command

#### Main Rolemenu

This is what handles the reactions and gives roles.

```js
{{$setEmoji := "accept:560961075613401088"}}
{{$setRole  := "274328749217153024"}}

{{$theUser  := (.Reaction.UserID)}}
{{$theEmoji := (joinStr ":" (.Reaction.Emoji.Name) (toString .Reaction.Emoji.ID))}}

{{deleteMessageReaction nil .Reaction.MessageID $theUser $theEmoji}}

{{if eq $theEmoji $setEmoji}}
  {{addRoleID $setRole}}
{{else}}
  {{deleteAllMessageReactions .Reaction.ChannelID .Reaction.MessageID}}
  {{addReactions $setEmoji}}
{{end}}
```

#### Rolemenu Configuration

This is what allows the rolemenu to be configured.

```js
{{$roleDBRaw := (dbGet 0 "baseroles").Value}}
{{$roleDBNew := .CmdArgs}}
{{$roleDB    := false}}

{{if $roleDBRaw}}
  {{$roleDB = ((cslice).AppendSlice $roleDBRaw)}}
  {{$roleDB = $roleDB.AppendSlice $roleDBNew}}
{{else}}
  {{$roleDB = $roleDBNew}}
{{end}}

{{dbSet 0 "baseroles" $roleDB}}

{{sendMessage nil $roleDB}}
```

## To-do

- [ ] Link both CCs

## Configuration

At the beginning of each custom command, there's a section for configuring it, starting at `{{/* Basic configuration */}}` and ending at `{{/* -- END OF CONFIGURATION -- */}}`.

`setEmoji` is an emoji variable that should be changed to the desired emoji. You can do so by following [this guide](/emoji-instructions.md).

`setRole` is a role id variable that should be changed to the desired role ID.
