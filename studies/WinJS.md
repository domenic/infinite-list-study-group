# WinJS `ListView`

- Study Author: [@domenic](https://github.com/domenic)
- Platform: web

## Background on WinJS

[WinJS](http://try.buildwinjs.com/) is a Microsoft project originally designed to allow writing Windows 8 "Metro" applications in JavaScript and HTML. It has since grown into a cross-platform JavaScript library, [maintained on GitHub](https://github.com/winjs/winjs).

The project appears to be [in maintenance mode](https://github.com/winjs/winjs#status). However, I think it's especially worth investigating as it's a recent attempt to build an operating system's worth of UI controls using web technologies.

## Essential sample code

### With imperative rendering

```js
const items = getTonsAndTonsOfItems();
window.itemList = new WinJS.Binding.List(items);

window.templateProcessor = async itemPromise => {
  const item = await itemPromise;
  // item has various other useful properties: e.g. index, isOnScreen

  const div = document.createElement("div");
  div.textContent = item.data.text;
  return div;
};

// Some sort of security precaution:
// http://try.buildwinjs.com/tutorial/1Basics/markSupport/
WinJS.Utilities.markSupportedForProcessing(window.templateProcessor);

```

```html
<div data-win-control="WinJS.UI.ListView"
     data-win-options="{
       itemDataSource: itemsList.dataSource,
       itemTemplate: templateProcessor,
       layout: { type: WinJS.UI.ListLayout }
     }">
</div>
```

[Live demo, including all the incidental supporting code](https://jsbin.com/kuyemit/edit?html,output)

### With declarative template stamping

```js
const items = getTonsAndTonsOfItems();
window.itemList = new WinJS.Binding.List(items);
```

```html
<div id="template" data-win-control="WinJS.Binding.Template" style="display: none">
  <div data-win-bind="textContent: text"></div>
</div>

<div data-win-control="WinJS.UI.ListView"
     data-win-options="{
       itemDataSource: itemsList.dataSource,
       itemTemplate: select('#template'),
       layout: { type: WinJS.UI.ListLayout }
     }">
</div>
```

[Live demo, including all the incidental supporting code](https://jsbin.com/xofabemolo/edit?html,output)

## Resulting DOM structure

```html
<div data-win-control="WinJS.UI.ListView"
     data-win-options="... as given ..."
     class="win-disposable win-listview win-element-resize"
     role="listbox" aria-multiselectable="true" tabindex="-1"
     style="position: relative;">
  <div tabindex="0" aria-hidden="true"></div>
  <div tabindex="-1" role="group" class="win-viewport win-vertical" style="opacity: 1;" aria-label="Scrolling Container">
    <div id="element__86" aria-flowto="element__2"></div>
    <div class="win-headercontainer" style="opacity: 1;"></div>
    <div class="win-surface win-listlayout win-nocssgrid _win-dynamic-containersize-0 win-structuralnodes win-single-itemsblock" style="width: 682px; opacity: 1;">
      <div class="_win-proxy"></div>
      <div class="win-itemscontainer win-uniformlistlayout" style="height: 8671px;">
        <div class="win-itemscontainer-padder"></div>
        <div class="win-itemsblock">
          <div class="win-container win-container-even">
            <div class="win-itembox">
              <div tabindex="0" class="win-item" id="element__2" x-ms-aria-flowfrom="element__86" role="option" aria-setsize="299" aria-posinset="1" aria-flowto="element__16">1</div>
            </div>
          </div>
          <div class="win-container win-container-odd">
            <div class="win-itembox">
              <div tabindex="0" class="win-item" id="element__16" x-ms-aria-flowfrom="element__2" role="option" aria-setsize="299" aria-posinset="2" aria-flowto="element__17">2</div>
            </div>
          </div>
          ... repeats for 44 total items ...
          <div class="win-container win-container-even win-backdrop"></div>
          <div class="win-container win-container-odd win-backdrop"></div>
          ... repeats for 256 total placeholders ...
        </div>
      </div>
    </div>
    <div class="win-footercontainer" style="opacity: 1; min-height: 0px;"></div>
    <div id="element__87" x-ms-aria-flowfrom="element__28"></div>
  </div>
  <div tabindex="0" aria-hidden="true"></div>
  <div aria-hidden="true" style="position:absolute;left:50%;top:50%;width:0px;height:0px;" tabindex="-1"></div>
  <div style="animation-name: WinJS-node-inserted; animation-duration: 0.01s; position: absolute;"></div>
</div>
```

## Customization options

## Miscellaneous takeaways

- The idea of requiring the data source to be wrapped into a custom class is interesting and perhaps useful.
- The reliance on global variables to hook things up is very sad.
- The way in which it modifies your markup, to insert classes, ARIA, `tabindex`, etc., is a little sad. We may be able to avoid some of this with shadow DOM?
- I'm not sure what, if anything, they're doing to make the placeholder items be the right height.

## Resources

- [Comprehensive API documentation](https://docs.microsoft.com/en-us/previous-versions/windows/apps/br211837(v=win.10))
- [Optimizing `ListView` item rendering](https://blogs.msdn.microsoft.com/windowsappdev/2013/06/17/optimizing-listview-item-rendering/) blog post
- Relevant tutorial sections:
  - [Binding lists](http://try.buildwinjs.com/tutorial/2WinJS_Binding/bindingList/) shows how they expect you to wrap your data in a `WinJS.Binding.List`. To introduce this concept it uses a separate control, `Repeater`, which is much simpler than `ListView`.
  - [Projections](http://try.buildwinjs.com/tutorial/3Control_Manipulation/projections/) talks about how you can create new `WinJS.Binding.List` instances "projected" from the original, e.g. a sorted one.
  - [Binding templates](http://try.buildwinjs.com/tutorial/2WinJS_Binding/bindingTemplates/) shows how to use a `ListView` to stamp out templates
  - [Function rendering](http://try.buildwinjs.com/tutorial/3Control_Manipulation/functionRend/) (i.e. imperative rendering) is the lower-level technology that underlies the template binding
- Demos:
  - [The new playground](http://try.buildwinjs.com/playground/) has three relevant demos, each of which demonstrate different ways to customize the `ListView`
  - [The former playground](http://winjs.azurewebsites.net/) has the same demos with different presentation.
