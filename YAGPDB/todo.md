# TODO

Here there should be a short description of it.
**WIP**

## Setup

1. Create a new **Custom Commands** group (hit the **+** sign)
1. Name it `TODO` or something similar
1. For **Whitelist roles for who can use these commands**, pick the roles that should have access to this command (usually `Staff` or `Moderator`) and leave the other settings on their default
1. **Create a new Custom Command**
1. For **Trigger type** select **Reaction**, and then pick **Added reactions only**
1. On **Response** copy and paste the first command from the **Custom Command** section below
1. Ensure **Custom command group** remains the previously created group (`Support`)
1. Disable **Output errors as command response** and leave the other settings on their default
1. Save and repeat those steps on the same group for the second command below
1. For the second command, you can tick **Only run in the following channels** and then select the normal texting channels users usually misuse as support, in order to prevent messages from other channels being moved to support
1. Save and test!

## Custom Command

#### New message in TODO
```js
{{$emojiTodoFalse := "<:chk_u:568993712508764186>"}}
{{$emojiTodoTrue  := "<:chk_c:568993681982357525>"}}
{{$todoKey        := "todo"}}

{{$theContent := .Message.Content}}
{{$theChannel := .Channel.ID}}

{{$todoDBRaw := (dbGet 0 $todoKey).Value}}
{{$todoDB    := false}}
{{$todoLast  := 0}}

{{if $todoDBRaw}}
  {{$todoDB   = sdict $todoDBRaw}}
  {{$todoLast = $todoDB.Get "todoLast"}}
{{else}}
  {{$todoDB = sdict "todoLast" $todoLast}}
{{end}}

{{if (reFind "\\D+" $theContent)}}
  {{/*Has text*/}}
  {{$todoThis    := (add $todoLast 1)}}
  {{$todoThisStr := (printf "%04d" $todoThis)}}
  {{$todoMessage := (sendMessageRetID $theChannel (joinStr "" "`" $todoThisStr "` " $emojiTodoFalse " " $theContent))}}
  {{$todoDB.Set "todoLast" $todoThis}}
  {{$todoDB.Set $todoThisStr $todoMessage}}
{{else}}
  {{/*Doesn't have text*/}}
  {{$theID := toInt $theContent}}
  {{range $key, $value := $todoDB}}
    {{if (eq (toInt $key) (toInt $theID))}}
      {{$todoMessageContent := (getMessage $theChannel $value).Content}}
      {{editMessage $theChannel $value (reReplace $emojiTodoFalse $todoMessageContent $emojiTodoTrue)}}
    {{end}}
  {{end}}
{{end}}

{{dbSet 0 $todoKey $todoDB}}

{{deleteTrigger 1}}
```

#### To-do (actual to-do list for this custom command)
- [ ] Create sub sdict inside each entry, for the message ID and completed status
- [ ] Create remaining custom commands, for adding new tasks from anywhere
- [ ] Logic for editing existing TODO

## Configuration
At the beginning of the custom command, there's a section for configuring it.

Here there should be info about it.
