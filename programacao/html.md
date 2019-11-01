# CSS Tricks

## Como esconder textos com `...` quando forem muito grandes

[https://stackoverflow.com/questions/25282269/html-large-text-with-dotted-end](https://stackoverflow.com/questions/25282269/html-large-text-with-dotted-end)

```css
#personName {
    white-space: nowrap;
    width: 300px;
    text-overflow: ellipsis;
    overflow: hidden;
}
```

