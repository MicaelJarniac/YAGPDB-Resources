```js
{{$deletedTrigger := false}}
{{if eq (len .Args) 1}}
    {{deleteTrigger 1}}
    {{$deletedTrigger = true}}
{{end}}

{{$quoteURLRegex := "https?:\\/\\/discord(?:app)?.com\\/channels\\/\\d+\\/\\d+\\/\\d+"}}

{{$quoteURL := reFind $quoteURLRegex .Cmd}}

{{$quoteIDs := reFindAll "\\d+" $quoteURL}}

{{$quoteMessageID := index $quoteIDs (sub (len $quoteIDs) 1)}}
{{$quoteChannelID := index $quoteIDs (sub (len $quoteIDs) 2)}}

{{$quoteMessage := getMessage $quoteChannelID $quoteMessageID}}
{{$quoteUser := $quoteMessage.Author}}

{{$rawEmbed := sdict
    "description" $quoteMessage.Content
    "author" (sdict "name" $quoteUser.Username "icon_url" ($quoteUser.AvatarURL "256"))
    "timestamp" $quoteMessage.Timestamp
}}

{{$rawContent := joinStr "" "Quote by " $quoteUser.Mention}}

{{if $deletedTrigger}}
    {{$rawContent = joinStr " - " $rawContent $quoteURL}}
{{end}}

{{$files := ""}}

{{$quoteAttachmentImage := false}}
{{if gt (len $quoteMessage.Attachments) 0}}
    {{$quoteAttachment := index $quoteMessage.Attachments 0}}
    {{if (and $quoteAttachment.Height (not $quoteAttachmentImage))}}
        {{$rawEmbed.Set "image" (sdict "url" $quoteAttachment.URL)}}
        {{$quoteAttachmentImage = true}}
    {{else}}
        {{if eq (len $files) 0}}
            {{$files = "\n\n**Files:**"}}
        {{end}}
        {{$files = joinStr "" $files "\n`" $quoteAttachment.Filename "`: " $quoteAttachment.URL}}
    {{end}}
{{end}}

{{$messageContent := joinStr "" $rawContent $files}}

{{$messageEmbed := cembed $rawEmbed}}

{{$message := complexMessage "content" $messageContent "embed" $messageEmbed}}

{{sendMessage nil $message}}
```
