# lazy-loop

```html
<ul>
    <li>header</li>
    <lazy-loop id=lazyLoop page-size=100 min-height=400 tag=li></lazy-loop>
    <li>footer</li>
</ul>
<script>
    const list = [];
    for(let i = 0; i < 1000000; i++){
        list.push({
            textContent:`hello ${i}`,
        });
    }
    lazyLoop.items = list;
</script>
```

produces:

```html
<ul>
    <li>header</li>
    <lazy-loop id=lazyLoop page-size=100 min-height=400 tag=li></lazy-loop>
    <laissez-dom>
        <template>
            <ib-id tag=li>
        </template>
    </laissez-dom>
        ...
    <laissez-dom>
        <template>
            <ib-id tag=li>
        </template>
    </laissez-dom>
```
