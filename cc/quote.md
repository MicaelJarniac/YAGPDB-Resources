```regex
https?:\/\/(?:www\.)?(?:\w*\.)?discord(?:app)?.com\/channels\/\d+\/\d+\/\d+
```

```js
{{if eq (len .Args) 1}}
    {{deleteTrigger 1}}
{{end}}

{{$quoteURLRegex := "https?:\\/\\/(?:www\\.)?(?:\\w*\\.)?discord(?:app)?.com\\/channels\\/\\d+\\/\\d+\\/\\d+"}}

{{$quoteURL := reFind $quoteURLRegex .Cmd}}

{{$quoteIDs := reFindAll "\\d+" $quoteURL}}

{{$quoteMessageID := index $quoteIDs (sub (len $quoteIDs) 1)}}
{{$quoteChannelID := index $quoteIDs (sub (len $quoteIDs) 2)}}

{{$quoteMessage := getMessage $quoteChannelID $quoteMessageID}}
{{$quoteUser := $quoteMessage.Author}}

{{$rawEmbed := sdict
    "description" (joinStr "" "**[Message](" $quoteURL ") from <#" $quoteMessage.ChannelID ">:**\n\n" $quoteMessage.Content)
    "author" (sdict "name" $quoteUser.Username "icon_url" ($quoteUser.AvatarURL "256"))
    "footer" (sdict "text" (joinStr "" "Quoted by " .Message.Author.Username) "icon_url" (.Message.Author.AvatarURL "256"))
    "timestamp" $quoteMessage.Timestamp
}}

{{$files := ""}}

{{$quoteAttachmentImage := false}}
{{if gt (len $quoteMessage.Attachments) 0}}
    {{$quoteAttachment := index $quoteMessage.Attachments 0}}
    {{if (and $quoteAttachment.Height (not $quoteAttachmentImage))}}
        {{$rawEmbed.Set "image" (sdict "url" $quoteAttachment.URL)}}
        {{$quoteAttachmentImage = true}}
    {{else}}
        {{$files = joinStr "" $files "\n`" $quoteAttachment.Filename "`: " $quoteAttachment.URL}}
    {{end}}
{{end}}

{{if gt (len $files) 0}}
    {{if not ($rawEmbed.Get "fields")}}
        {{$rawEmbed.Set "fields" cslice}}
    {{end}}
    {{$rawEmbed.Set "fields" (($rawEmbed.Get "fields").Append (sdict "name" "Files" "value" $files "inline" true))}}
{{end}}

{{$messageEmbed := cembed $rawEmbed}}

{{$message := complexMessage "embed" $messageEmbed}}

{{sendMessage nil $message}}
```
