---
layout: docs
title: Drag Element
permalink: /documentation/test-api/actions/drag-element.html
---
# Drag Element

There are two ways to drag a page element.

* [Drag an Element by an Offset](#drag-an-element-by-an-offset)
* [Drag an Element onto Another One](#drag-an-element-onto-another-one)

> The `t.drag` or `t.dragToElement` actions will not invoke integrated browser actions such as selecting text.
> Use them to perform drag-and-drop actions that are processed by webpage elements, not the browser.
> To select text, use [t.selectText](select-text.md).

## Drag an Element by an Offset

```text
t.drag( selector, dragOffsetX, dragOffsetY [, options] )
```

Parameter              | Type                                              | Description
---------------------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------
`selector`             | Function &#124; String &#124; Selector &#124; Snapshot &#124; Promise | Identifies the webpage element being dragged. See [Selecting Target Elements](index.md#selecting-target-elements).
`dragOffsetX`          | Number                                            | An X-offset of the drop coordinates from the mouse pointer's initial position.  
`dragOffsetY`          | Number                                            | An Y-offset of the drop coordinates from the mouse pointer's initial position.
`options` *(optional)* | Object                                            | A set of options that provide additional parameters for the action. See [Mouse Action Options](action-options.md#mouse-action-options).

The following example demonstrates how to use the `t.drag` action with a slider.

```js
import { expect } from 'chai';
import { Selector } from 'testcafe';

fixture `My fixture`
    .page('http://www.example.com/');

const getSlider = Selector(id => document.getElementById('developer-rating'));

test('Drag slider', async t => {
    await t.click('#i-tried-testcafe');

    const slider = await getSlider();

    expect(slider.value).to.equal(1);

    await t.drag('.ui-slider-handle', 360, 0, { offsetX: 10, offsetY: 10 });

    slider = await getSlider();

    expect(slider.value).to.equal(7);
});
```

## Drag an Element onto Another One

```text
t.dragToElement( selector, destinationSelector [, options] )
```

Parameter              | Type                                              | Description
---------------------- | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------
`selector`             | Function &#124; String &#124; Selector &#124; Snapshot &#124; Promise | Identifies the webpage element being dragged. See [Selecting Target Elements](index.md#selecting-target-elements).
`destinationSelector`  | Function &#124; String &#124; Selector &#124; Snapshot &#124; Promise | Identifies the webpage element that serves as the drop location. See [Selecting Target Elements](index.md#selecting-target-elements).  
`options` *(optional)* | Object                                            | A set of options that provide additional parameters for the action. See [Mouse Action Options](action-options.md#mouse-action-options).

This sample shows how to drop an element into an area using the `t.dragToElement` action.

```js
import { expect } from 'chai';
import { ClientFunction } from 'testcafe';

fixture `My fixture`
    .page('http://www.example.com/');

const isDesignSurfaceEmpty = ClientFunction(() => !getDesignSurfaceElements().length);

test('Drag an item from the toolbox', async t => {
    await t.dragToElement('.toolbox-item.text-input', '.design-surface');

    expect(await isDesignSurfaceEmpty()).to.be.false;
});
```