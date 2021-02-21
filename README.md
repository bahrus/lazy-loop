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
            <i-bid init-count=100></i-bid>
            <li>Item 100</li>
            <li>Item 101</li>
            ...
            <li>Item 199</li>
        </template>
    </laissez-dom>
    <laissez-dom>
        <template>
            <i-bid init-count=100></i-bid>
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
```

So how would yet another web component, in addition to laissez-dom, and i-bid, help?

In cases where it makes sense to generate this markup in the browser -- maybe in an [element worklet?](https://jasonformat.com/element-worklet/), lazy-loop can (maybe) help:

```html
<!-- p-et-alia notation -->
<my-lazy-loop -list></my-lazy-loop>
```

produces:

```html
<ul>
    <li>header</li>
        <!--Initial Page rendered from Server -->
    <li>Item 1</li>
    <li>Item 2</li>
    ...
    <li>Item 99</li>
    <!--Lazy Loaded content -->
    <my-lazy-loop -list></my-lazy-loop>
    <!--Lazy Loaded content -->
    <laissez-dom>
        <template>
            <i-bid init-count=100></i-bid>
            <li>Item 100</li>
            <li>Item 101</li>
            ...
            <li>Item 199</li>
        </template>
    </laissez-dom>
    <laissez-dom>
        <template>
            <i-bid init-count=100></i-bid>
            <li>Item 200</li>
            <li>Item 201</li>
            ...
            <li>Item 299</li>
        </template>
    </laissez-dom>
```

where my-lazy-loop:

1.  Extends abstract class lazyLoop
2.  Must implement some rendering method which generates the custom content.
3.  Can use, for example, tagged template libraries (how well do existing ones with template markup?)
4.  What value does the base class provide?
5.  Passing new list "pages".
6.  Or should laissez-dom simply be enhanced to provide "hooks" for passing new data, no benefit from this web component?
