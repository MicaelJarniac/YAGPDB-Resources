# Customizing Emojis

In multiple custom commands, there's the possibility to customize the emojis that'll be used on it.

To do so, you have to use it's name and ID combo, like the following:  
`"ss:671134486666280988"`

Notice how it's all inside quotes (`"`), as it's a string variable, and also notice there's only one colon (`:`), and it's between the name and the ID of the emoji.

To obtain this information from an emoji, all you have to do is insert the emoji on the text input field as normal, but prefixing it with a `\` before sending, like so:  
`\:ss:`

After sending it, Discord will break it into the name and ID combo, but note that Discord is including a colon before the emoji name, as well as wrapping it inside `<>`s, like this:  
`<:ss:671134486666280988>`

You can then copy it, paste it in the configuration, delete the first colon and the surrounding `<>`s, wrap it in quotes (`"`), and it should be good to go.
