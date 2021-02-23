# lazy-loop

[content-visibility](https://web.dev/content-visibility/) may solve some problems effectively, but I'm not yet convinced content-visibility fixes all issues -- I've looked at some scenarios (like a server-rendered, fully expanded details/summary tree as well as a simple ui/li list), and I'm just not able to replicate the savings from content-visibility.  


lazy-loop generates a lazy list of non-nested (custom) elements.  To provide nested content, the user of lazy-loop will need to encapsulate that nested content via a custom element (with or without ShadowDOM).  It combines two other custom elements, i-bid and laissez-dom.  It essentially creates a facade of i-bid, but with support for lazy loading.

Like i-bid, lazy-loop can work in tandem with server-rendered HTML.  However, it is more client-side centric, in that the content in generates (can be) generated from a [streamed](https://web.dev/streams/) source of data, which can be partitioned into "pages" of grouped data.  These pages are lazy loaded into the DOM when scrolled into view.

```html
<!-- p-et-alia notation -->
<ul>
    <li>header</li>
        <!--Initial Page rendered from Server -->
    <li>Item 1</li>
    <li>Item 2</li>
    ...
    <li>Item 99</li>
    <lazy-loop -paged-list -new-page></lazy-loop>
</ul>
```

produces, after passing in a corresponding list to lazy-loop:

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
            <i-bid></i-bid>
        </template>
    </laissez-dom>
    <laissez-dom>
        <template>
            <i-bid></i-bid>
        </template>
    </laissez-dom>
    <li>footer</li>
</ul>
<style>
laissez-dom {
    min-height: 1000px;
}
</style>
```

lazy-loop passes the "page" of data to i-bid after laissez-dom becomes visible (and exposes the template contents).
