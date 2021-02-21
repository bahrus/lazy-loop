# lazy-loop

Let's assume we were back in the pre content-visibility era -- actually, I'm not yet convinced content-visibility fixes all issues -- I've looked at some scenarios (like a server-rendered, fully expanded details/summary tree), and I'm just not able to replicate the savings from this setting (I checked several chrome versions back).  Anyway.  

With custom elements / repeating templates, what is a viable strategy for display a large amount of repeating content quickly, that could come within a stone's throw of server-rendered, if not beat it in some cases?

It seems likely that above the fold, often (but I suspect not always?) we want pre-rendered HTML.  But below the fold?

lazy-loop attempts to suggest markup that can accommodate different (but most-likely-to-keep-performance-down) scenarios.

```html
<ul>
    <li>header</li>
    <!--Initial Page rendered from Server -->
    <li>Item 1</li>
    <li>Item 2</li>
    ...
    <li>Item 99</li>
    <laissez-dom>
        <template>
            <script type=application/json>
            {
                "ibid": {
                    "list": ["Item 100", "item 101", ... "item 199"]
                }
            }
            </script>
            <ib-id></ib-id>
        </template>
    </laissez-dom>
    <laissez-dom>
        <template>
            <script type=application/json>
            {
                "ibid": {
                    "list": ["Item 100", "item 101", ... "item 199"]
                }
            }
            </script>
            <ib-id></ib-id>
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
