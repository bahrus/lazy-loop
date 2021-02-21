# lazy-loop

Let's assume we are back in the pre [content-visibility](https://web.dev/content-visibility/) era -- actually, I'm not yet convinced content-visibility fixes all issues -- I've looked at some scenarios (like a server-rendered, fully expanded details/summary tree), and I'm just not able to replicate the savings from this setting (I checked several chrome versions back).  Anyway.  

With custom elements / repeating templates, what is a viable strategy for displaying a large amount of repeating content quickly, that could come within a stone's throw of server-rendered, if not beat it in some cases?

It seems likely that above the fold, often (but I suspect not always?) we want pre-rendered HTML.  But below the fold?

lazy-loop attempts to suggest markup that can accommodate different (but most-likely-to-keep-performance-on-par-with-server-rendered) scenarios.

```html
<ul>
    <li>header</li>
    <!--Initial Page rendered from Server -->
    <li>Item 1</li>
    <li>Item 2</li>
    ...
    <li>Item 99</li>
    <!--Lazy Loaded content -->
    <laissez-dom>
        <template>
            <ib-id init-count=100></ib-id>
            <li>Item 100</li>
            <li>Item 101</li>
            ...
            <li>Item 199</li>
        </template>
    </laissez-dom>
    <laissez-dom>
        <template>
            <ib-id init-count=100></ib-id>
            <li>Item 200</li>
            <li>Item 201</li>
            ...
            <li>Item 299</li>
        </template>
    </laissez-dom>
    <li>footer</li>
</ul>
<style>
laissez-dom {
    min-height: 1000px;
}
</style>
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
    <lazy-loop id=lazyLoop page-size=100 tag=li></lazy-loop>
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
