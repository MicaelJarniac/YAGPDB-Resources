```jsx
{{if (hasRoleID 274328749217153024)}}
  {{print "You have already accepted the rules."}}
  {{deleteTrigger 1}}
{{else}}
  {{addReactions "accept:560961075613401088"}}
{{end}}

{{addRoleID 274328749217153024}}
{{deleteTrigger 3}}
{{deleteResponse 3}}
```