# lazy-loop

[content-visibility](https://web.dev/content-visibility/) may solve some problems effectively, but I'm not yet convinced content-visibility fixes all issues -- I've looked at some scenarios (like a server-rendered, fully expanded details/summary tree as well as a simple ui/li list), and I'm just not able to replicate the savings from content-visibility.  


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
<ul>
    <li>header</li>
        <!--Initial Page rendered from Server -->
    <li>Item 1</li>
    <li>Item 2</li>
    ...
    <li>Item 99</li>
    <my-lazy-loop -list></my-lazy-loop>
</ul>
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
