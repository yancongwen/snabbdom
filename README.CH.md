<img alt="Snabbdom" src="readme-title.svg" width="356px">

一个专注于简单、模块化、功能丰富、极致性能的虚拟 DOM 库。

---

[![License: MIT](https://img.shields.io/badge/License-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![Build Status](https://travis-ci.org/snabbdom/snabbdom.svg?branch=master)](https://travis-ci.org/snabbdom/snabbdom)
[![npm version](https://badge.fury.io/js/snabbdom.svg)](https://badge.fury.io/js/snabbdom)
[![npm downloads](https://img.shields.io/npm/dm/snabbdom.svg)](https://www.npmjs.com/package/snabbdom)
[![Join the chat at https://gitter.im/snabbdom/snabbdom](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/snabbdom/snabbdom?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![Donate to our collective](https://opencollective.com/snabbdom/donate/button@2x.png?color=blue)](https://opencollective.com/snabbdom#section-contribute)

Thanks to [Browserstack](https://www.browserstack.com/) for providing access to
their great cross-browser testing tools.

---

## 简介

虚拟 DOM 是了不起的。它允许我们使用函数和状态表示应用视图。但是现有的解决方案太过臃肿，性能较差，或者缺少某些我需要的特性。

Snabbdom 做到了极简，核心代码只有约200行，并且支持以模块化的形式进行拓展，为了简化核心部分代码，所有不必要的功能都通过模块实现。

你可以随心所欲的使用 Snabbdom，选择或者自定义你想要的功能。或者仅仅使用默认的拓展以获得高性能的虚拟 DOM。Snabbdom 支持的特性在下面列出。

## 特性

* 核心特性：
    * 200 行核心代码，你可以轻松阅读源码，并理解它的工作原理；
    * 支持模块化拓展；
    * 丰富的钩子函数，帮助你控制 `patch` 和 `diff` 的各个过程，你可以设置全局钩子，也可以在单个节点上设置钩子;
    * 性能最优，Snabbdom 几乎是性能最强的虚拟 DOM 库；
    * 可以轻松地与FRP库集成；

* 模块特性：
    * 提供 `h` 帮助方法以方便生成虚拟 DOM；
    * [支持 SVG](#svg);
    * 支持 CSS 动画；
    * 强大的事件监听机制；
    * 提供 [Thanks](#thunks) 进一步优化 diff 和 patch 过程；
    * 支持 `JSX`，支持 `TypeScript`，[snabbdom-pragma](https://github.com/Swizz/snabbdom-pragma)；

* 支持第三方模块
  * 服务端渲染，[snabbdom-to-html](https://github.com/acstll/snabbdom-to-html);
  * 简化虚拟DOM生成方式，[snabbdom-helpers](https://github.com/krainboltgreene/snabbdom-helpers)；
  * 字符串模板，[snabby](https://github.com/jamen/snabby)；
  * 虚拟DOM断言，[snabbdom-looks-like](https://github.com/jvanbruegge/snabbdom-looks-like)；


## 示例

```mjs
import {
  init,
  classModule,
  propsModule,
  styleModule,
  eventListenersModule,
  h,
} from "snabbdom";

const patch = init([
  // Init patch function with chosen modules
  classModule, // makes it easy to toggle classes
  propsModule, // for setting properties on DOM elements
  styleModule, // handles styling on elements with support for animations
  eventListenersModule, // attaches event listeners
]);

const container = document.getElementById("container");

const vnode = h("div#container.two.classes", { on: { click: someFn } }, [
  h("span", { style: { fontWeight: "bold" } }, "This is bold"),
  " and this is just normal text",
  h("a", { props: { href: "/foo" } }, "I'll take you places!"),
]);
// Patch into empty DOM element – this modifies the DOM as a side effect
patch(container, vnode);

const newVnode = h(
  "div#container.two.classes",
  { on: { click: anotherEventHandler } },
  [
    h(
      "span",
      { style: { fontWeight: "normal", fontStyle: "italic" } },
      "This is now italic type"
    ),
    " and this is still just normal text",
    h("a", { props: { href: "/bar" } }, "I'll take you places!"),
  ]
);
// Second `patch` invocation
patch(vnode, newVnode); // Snabbdom efficiently updates the old view to the new state
```

## 更多示例

- [Animated reordering of elements](http://snabbdom.github.io/snabbdom/examples/reorder-animation/)
- [Hero transitions](http://snabbdom.github.io/snabbdom/examples/hero/)
- [SVG Carousel](http://snabbdom.github.io/snabbdom/examples/carousel-svg/)

---

## 目录

- [Core documentation](#core-documentation)
  - [`init`](#init)
  - [`patch`](#patch)
    - [Unmounting](#unmounting)
  - [`h`](#h)
  - [`tovnode`](#tovnode)
  - [Hooks](#hooks)
    - [Overview](#overview)
    - [Usage](#usage)
    - [The `init` hook](#the-init-hook)
    - [The `insert` hook](#the-insert-hook)
    - [The `remove` hook](#the-remove-hook)
    - [The `destroy` hook](#the-destroy-hook)
  - [Creating modules](#creating-modules)
- [Modules documentation](#modules-documentation)
  - [The class module](#the-class-module)
  - [The props module](#the-props-module)
  - [The attributes module](#the-attributes-module)
  - [The dataset module](#the-dataset-module)
  - [The style module](#the-style-module)
    - [Custom properties (CSS variables)](#custom-properties-css-variables)
    - [Delayed properties](#delayed-properties)
    - [Set properties on `remove`](#set-properties-on-remove)
    - [Set properties on `destroy`](#set-properties-on-destroy)
  - [The eventlisteners module](#the-eventlisteners-module)
- [SVG](#svg)
  - [Classes in SVG Elements](#classes-in-svg-elements)
- [Thunks](#thunks)
- [JSX](#jsx)
  - [TypeScript](#typescript)
  - [Babel](#babel)
- [Virtual Node](#virtual-node)
  - [sel : String](#sel--string)
  - [data : Object](#data--object)
  - [children : Array<vnode>](#children--arrayvnode)
  - [text : string](#text--string)
  - [elm : Element](#elm--element)
  - [key : string | number](#key--string--number)
- [Structuring applications](#structuring-applications)
- [Common errors](#common-errors)
- [Opportunity for community feedback](#opportunity-for-community-feedback)

## 核心文档

Snabbdom 核心部分仅提供核心功能，在保证高效和可拓展的同时，做了尽可能的简化。

### `init`

核心部分仅暴露出一个方法 `init`，传入参数为模块列表，返回一个 `patch` 方法。

```mjs
import { classModule, styleModule } from "snabbdom";

const patch = init([classModule, styleModule]);
```

### `patch`

`patch` 方法由 `init` 方法返回，需要两个参数，第一个为 DOM 元素或者表示当前视图的虚拟 DOM 对象，第二个参数为代表新的视图的虚拟 DOM 对象。

如果第一个参数`oldVnode`是一个 DOM 元素，`newVnode`将转换为真实 DOM 元素，并替换`oldVnode`；如果第一个参数为虚拟DOM对象，Snabbdom 将根据 `newVnode`对 `oldVnode`高效更新。

传入的 `oldVnode`是上一轮执行`patch`后返回的虚拟 DOM 对象，这是必要的，因为Snabbdom将信息存储于虚拟DOM中。这样可以减小下次`patch`开销，提升性能，避免了再次创建虚拟DOM。

```mjs
patch(oldVnode, newVnode);
```

#### 卸载

Snabbdom 并没有提供卸载方法，但是这里有一个 hack 的方式去实现卸载：将注释节点作为 `patch` 的第二个参数，示例：

```mjs
patch(
  oldVnode,
  h("!", {
    hooks: {
      post: () => {
        /* patch complete */
      },
    },
  })
);
```

当然，这里仍然会存在一个注释节点。

### `h`

推荐使用`h`创建虚拟节点。它接受三个参数：第一个参数为标签或者其他选择器（string）；第二个参数为配置对象（object，可选）；第三个参数为字符串或者子节点数组。

```mjs
import { h } from "snabbdom";

const vnode = h("div", { style: { color: "#000" } }, [
  h("h1", "Headline"),
  h("p", "A paragraph"),
]);
```

### `tovnode`

`tovnode` 用于将真实DOM节点转换为虚拟节点对象。

```mjs
import {
  init,
  classModule,
  propsModule,
  styleModule,
  eventListenersModule,
  h,
  toVNode,
} from "snabbdom";

const patch = init([
  // Init patch function with chosen modules
  classModule, // makes it easy to toggle classes
  propsModule, // for setting properties on DOM elements
  styleModule, // handles styling on elements with support for animations
  eventListenersModule, // attaches event listeners
]);

const newVNode = h("div", { style: { color: "#000" } }, [
  h("h1", "Headline"),
  h("p", "A paragraph"),
]);

patch(toVNode(document.querySelector(".container")), newVNode);
```

### Hooks

Snabbdom 提供了丰富的 hooks，用于监听和侵入 DOM 的各个生命周期。在模块中可以借助 hooks 可以灵活拓展 Snabbdom；普通代码中也可以借助 hooks 在虚拟 DOM 的各个声明周期执行任意代码。

#### Hooks 概览

| Name        | Triggered when                                     | Arguments to callback   |
| ----------- | -------------------------------------------------- | ----------------------- |
| `pre`       | the patch process begins                           | none                    |
| `init`      | a vnode has been added                             | `vnode`                 |
| `create`    | a DOM element has been created based on a vnode    | `emptyVnode, vnode`     |
| `insert`    | an element has been inserted into the DOM          | `vnode`                 |
| `prepatch`  | an element is about to be patched                  | `oldVnode, vnode`       |
| `update`    | an element is being updated                        | `oldVnode, vnode`       |
| `postpatch` | an element has been patched                        | `oldVnode, vnode`       |
| `destroy`   | an element is directly or indirectly being removed | `vnode`                 |
| `remove`    | an element is directly being removed from the DOM  | `vnode, removeCallback` |
| `post`      | the patch process is done                          | none                    |

下面的钩子可以在模块中使用: `pre`, `create`,
`update`, `destroy`, `remove`, `post`.

下面的钩子可以在单个元素的 hook 属性中使用: `init`, `create`, `insert`, `prepatch`, `update`,
`postpatch`, `destroy`, `remove`.

#### 使用

直接看代码：

```mjs
h("div.row", {
  key: movie.rank,
  hook: {
    insert: (vnode) => {
      movie.elmHeight = vnode.elm.offsetHeight;
    },
  },
});
```

#### The `init` hook

找到新的虚拟节点后，将在 patch 过程中调用此钩子。
在 Snabbdom 以任何方式处理该节点之前，将调用该挂钩。也就是说，在创建真实 DOM 节点之前。

#### The `insert` hook

一旦将 vnode 插入到文档中，便会调用此钩子，并在其余的补丁周期中完成此操作。这意味着您可以安全地在此挂钩中执行 DOM 测量，因为知道之后不会更改任何元素，而这可能会影响所插入元素的位置。

#### The `remove` hook

Allows you to hook into the removal of an element. The hook is called
once a vnode is to be removed from the DOM. The handling function
receives both the vnode and a callback. You can control and delay the
removal with the callback. The callback should be invoked once the
hook is done doing its business, and the element will only be removed
once all `remove` hooks have invoked their callback.

The hook is only triggered when an element is to be removed from its
parent – not if it is the child of an element that is removed. For
that, see the `destroy` hook.

#### The `destroy` hook

This hook is invoked on a virtual node when its DOM element is removed
from the DOM or if its parent is being removed from the DOM.

To see the difference between this hook and the `remove` hook,
consider an example.

```mjs
const vnode1 = h("div", [h("div", [h("span", "Hello")])]);
const vnode2 = h("div", []);
patch(container, vnode1);
patch(vnode1, vnode2);
```

Here `destroy` is triggered for both the inner `div` element _and_ the
`span` element it contains. `remove`, on the other hand, is only
triggered on the `div` element because it is the only element being
detached from its parent.

You can, for instance, use `remove` to trigger an animation when an
element is being removed and use the `destroy` hook to additionally
animate the disappearance of the removed element's children.

### Creating modules

Modules works by registering global listeners for [hooks](#hooks). A module is simply a dictionary mapping hook names to functions.

```mjs
const myModule = {
  create: function (oldVnode, vnode) {
    // invoked whenever a new virtual node is created
  },
  update: function (oldVnode, vnode) {
    // invoked whenever a virtual node is updated
  },
};
```

With this mechanism you can easily augment the behaviour of Snabbdom.
For demonstration, take a look at the implementations of the default
modules.

## Modules documentation

This describes the core modules. All modules are optional.

### The class module

The class module provides an easy way to dynamically toggle classes on
elements. It expects an object in the `class` data property. The
object should map class names to booleans that indicates whether or
not the class should stay or go on the vnode.

```mjs
h("a", { class: { active: true, selected: false } }, "Toggle");
```

### The props module

Allows you to set properties on DOM elements.

```mjs
h("a", { props: { href: "/foo" } }, "Go to Foo");
```

Properties can only be set. Not removed. Even though browsers allow addition and
deletion of custom properties, deletion will not be attempted by this module.
This makes sense, because native DOM properties cannot be removed. And
if you are using custom properties for storing values or referencing
objects on the DOM, then please consider using
[data-\* attributes](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)
instead. Perhaps via [the dataset module](#the-dataset-module).

### The attributes module

Same as props, but set attributes instead of properties on DOM elements.

```mjs
h("a", { attrs: { href: "/foo" } }, "Go to Foo");
```

Attributes are added and updated using `setAttribute`. In case of an
attribute that had been previously added/set and is no longer present
in the `attrs` object, it is removed from the DOM element's attribute
list using `removeAttribute`.

In the case of boolean attributes (e.g. `disabled`, `hidden`,
`selected` ...), the meaning doesn't depend on the attribute value
(`true` or `false`) but depends instead on the presence/absence of the
attribute itself in the DOM element. Those attributes are handled
differently by the module: if a boolean attribute is set to a
[falsy value](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
(`0`, `-0`, `null`, `false`,`NaN`, `undefined`, or the empty string
(`""`)), then the attribute will be removed from the attribute list of
the DOM element.

### The dataset module

Allows you to set custom data attributes (`data-*`) on DOM elements. These can then be accessed with the [HTMLElement.dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset) property.

```mjs
h("button", { dataset: { action: "reset" } }, "Reset");
```

### The style module

The style module is for making your HTML look slick and animate smoothly. At
its core it allows you to set CSS properties on elements.

```mjs
h(
  "span",
  {
    style: {
      border: "1px solid #bada55",
      color: "#c0ffee",
      fontWeight: "bold",
    },
  },
  "Say my name, and every colour illuminates"
);
```

Note that the style module does not remove style attributes if they
are removed as properties from the style object. To remove a style,
you should instead set it to the empty string.

```mjs
h(
  "div",
  {
    style: { position: shouldFollow ? "fixed" : "" },
  },
  "I, I follow, I follow you"
);
```

#### Custom properties (CSS variables)

CSS custom properties (aka CSS variables) are supported, they must be prefixed
with `--`

```mjs
h(
  "div",
  {
    style: { "--warnColor": "yellow" },
  },
  "Warning"
);
```

#### Delayed properties

You can specify properties as being delayed. Whenever these properties
change, the change is not applied until after the next frame.

```mjs
h(
  "span",
  {
    style: {
      opacity: "0",
      transition: "opacity 1s",
      delayed: { opacity: "1" },
    },
  },
  "Imma fade right in!"
);
```

This makes it easy to declaratively animate the entry of elements.

The `all` value of `transition-property` is not supported.

#### Set properties on `remove`

Styles set in the `remove` property will take effect once the element
is about to be removed from the DOM. The applied styles should be
animated with CSS transitions. Only once all the styles are done
animating will the element be removed from the DOM.

```mjs
h(
  "span",
  {
    style: {
      opacity: "1",
      transition: "opacity 1s",
      remove: { opacity: "0" },
    },
  },
  "It's better to fade out than to burn away"
);
```

This makes it easy to declaratively animate the removal of elements.

The `all` value of `transition-property` is not supported.

#### Set properties on `destroy`

```mjs
h(
  "span",
  {
    style: {
      opacity: "1",
      transition: "opacity 1s",
      destroy: { opacity: "0" },
    },
  },
  "It's better to fade out than to burn away"
);
```

The `all` value of `transition-property` is not supported.

### The eventlisteners module

The event listeners module gives powerful capabilities for attaching
event listeners.

You can attach a function to an event on a vnode by supplying an
object at `on` with a property corresponding to the name of the event
you want to listen to. The function will be called when the event
happens and will be passed the event object that belongs to it.

```mjs
function clickHandler(ev) {
  console.log("got clicked");
}
h("div", { on: { click: clickHandler } });
```

Very often, however, you're not really interested in the event object
itself. Often you have some data associated with the element that
triggers an event and you want that data passed along instead.

Consider a counter application with three buttons, one to increment
the counter by 1, one to increment the counter by 2 and one to
increment the counter by 3. You don't really care exactly which button
was pressed. Instead you're interested in what number was associated
with the clicked button. The event listeners module allows one to
express that by supplying an array at the named event property. The
first element in the array should be a function that will be invoked
with the value in the second element once the event occurs.

```mjs
function clickHandler(number) {
  console.log("button " + number + " was clicked!");
}
h("div", [
  h("a", { on: { click: [clickHandler, 1] } }),
  h("a", { on: { click: [clickHandler, 2] } }),
  h("a", { on: { click: [clickHandler, 3] } }),
]);
```

Each handler is called not only with the given arguments but also with the current event and vnode appended to the argument list. It also supports using multiple listeners per event by specifying an array of handlers:

```mjs
stopPropagation = function (ev) {
  ev.stopPropagation();
};
sendValue = function (func, ev, vnode) {
  func(vnode.elm.value);
};

h("a", { on: { click: [[sendValue, console.log], stopPropagation] } });
```

Snabbdom allows swapping event handlers between renders. This happens without
actually touching the event handlers attached to the DOM.

Note, however, that **you should be careful when sharing event
handlers between vnodes**, because of the technique this module uses
to avoid re-binding event handlers to the DOM. (And in general,
sharing data between vnodes is not guaranteed to work, because modules
are allowed to mutate the given data).

In particular, you should **not** do something like this:

```mjs
// Does not work
const sharedHandler = {
  change: function (e) {
    console.log("you chose: " + e.target.value);
  },
};
h("div", [
  h("input", {
    props: { type: "radio", name: "test", value: "0" },
    on: sharedHandler,
  }),
  h("input", {
    props: { type: "radio", name: "test", value: "1" },
    on: sharedHandler,
  }),
  h("input", {
    props: { type: "radio", name: "test", value: "2" },
    on: sharedHandler,
  }),
]);
```

For many such cases, you can use array-based handlers instead (described above).
Alternatively, simply make sure each node is passed unique `on` values:

```mjs
// Works
const sharedHandler = function (e) {
  console.log("you chose: " + e.target.value);
};
h("div", [
  h("input", {
    props: { type: "radio", name: "test", value: "0" },
    on: { change: sharedHandler },
  }),
  h("input", {
    props: { type: "radio", name: "test", value: "1" },
    on: { change: sharedHandler },
  }),
  h("input", {
    props: { type: "radio", name: "test", value: "2" },
    on: { change: sharedHandler },
  }),
]);
```

## SVG

SVG just works when using the `h` function for creating virtual
nodes. SVG elements are automatically created with the appropriate
namespaces.

```mjs
const vnode = h("div", [
  h("svg", { attrs: { width: 100, height: 100 } }, [
    h("circle", {
      attrs: {
        cx: 50,
        cy: 50,
        r: 40,
        stroke: "green",
        "stroke-width": 4,
        fill: "yellow",
      },
    }),
  ]),
]);
```

See also the [SVG example](./examples/svg) and the [SVG Carousel example](./examples/carousel-svg/).

### Classes in SVG Elements

Certain browsers (like IE &lt;=11) [do not support `classList` property in SVG elements](http://caniuse.com/#feat=classlist).
Because the _class_ module internally uses `classList`, it will not work in this case unless you use a [classList polyfill](https://www.npmjs.com/package/classlist-polyfill).
(If you don't want to use a polyfill, you can use the `class` attribute with the _attributes_ module).

## Thunks

The `thunk` function takes a selector, a key for identifying a thunk,
a function that returns a vnode and a variable amount of state
parameters. If invoked, the render function will receive the state
arguments.

`thunk(selector, key, renderFn, [stateArguments])`

The `renderFn` is invoked only if the `renderFn` is changed or `[stateArguments]` array length or it's elements are changed.

The `key` is optional. It should be supplied when the `selector` is
not unique among the thunks siblings. This ensures that the thunk is
always matched correctly when diffing.

Thunks are an optimization strategy that can be used when one is
dealing with immutable data.

Consider a simple function for creating a virtual node based on a number.

```mjs
function numberView(n) {
  return h("div", "Number is: " + n);
}
```

The view depends only on `n`. This means that if `n` is unchanged,
then creating the virtual DOM node and patching it against the old
vnode is wasteful. To avoid the overhead we can use the `thunk` helper
function.

```mjs
function render(state) {
  return thunk("num", numberView, [state.number]);
}
```

Instead of actually invoking the `numberView` function this will only
place a dummy vnode in the virtual tree. When Snabbdom patches this
dummy vnode against a previous vnode, it will compare the value of
`n`. If `n` is unchanged it will simply reuse the old vnode. This
avoids recreating the number view and the diff process altogether.

The view function here is only an example. In practice thunks are only
relevant if you are rendering a complicated view that takes
significant computational time to generate.

## JSX

### TypeScript

Add the following options to your `tsconfig.json`:

```json
{
  "compilerOptions": {
    "jsx": "react",
    "jsxFactory": "jsx"
  }
}
```

Then make sure that you use the `.tsx` file extension and import the `jsx` function at the top of the file:

```tsx
import { jsx, VNode } from "snabbdom";

const node: VNode = (
  <div>
    <span>I was created with JSX</span>
  </div>
);
```

### Babel

Add the following options to your babel configuration:

```json
{
  "plugins": [
    [
      "@babel/plugin-transform-react-jsx",
      {
        "pragma": "jsx"
      }
    ]
  ]
}
```

Then make sure that you use the `.jsx` file extension and import the `jsx` function at the top of the file:

```jsx
import { jsx } from "snabbdom";

const node = (
  <div>
    <span>I was created with JSX</span>
  </div>
);
```

## Virtual Node

**Properties**

- [sel](#sel--string)
- [data](#data--object)
- [children](#children--array)
- [text](#text--string)
- [elm](#elm--element)
- [key](#key--string--number)

### sel : String

The `.sel` property of a virtual node is the CSS selector passed to
[`h()`](#snabbdomh) during creation. For example: `h('div#container', {}, [...])` will create a a virtual node which has `div#container` as
its `.sel` property.

### data : Object

The `.data` property of a virtual node is the place to add information
for [modules](#modules-documentation) to access and manipulate the
real DOM element when it is created; Add styles, CSS classes,
attributes, etc.

The data object is the (optional) second parameter to [`h()`](#snabbdomh)

For example `h('div', {props: {className: 'container'}}, [...])` will produce a virtual node with

```mjs
({
  props: {
    className: "container",
  },
});
```

as its `.data` object.

### children : Array<vnode>

The `.children` property of a virtual node is the third (optional)
parameter to [`h()`](#snabbdomh) during creation. `.children` is
simply an Array of virtual nodes that should be added as children of
the parent DOM node upon creation.

For example `h('div', {}, [ h('h1', {}, 'Hello, World') ])` will
create a virtual node with

```mjs
[
  {
    sel: "h1",
    data: {},
    children: undefined,
    text: "Hello, World",
    elm: Element,
    key: undefined,
  },
];
```

as its `.children` property.

### text : string

The `.text` property is created when a virtual node is created with
only a single child that possesses text and only requires
`document.createTextNode()` to be used.

For example: `h('h1', {}, 'Hello')` will create a virtual node with
`Hello` as its `.text` property.

### elm : Element

The `.elm` property of a virtual node is a pointer to the real DOM
node created by snabbdom. This property is very useful to do
calculations in [hooks](#hooks) as well as
[modules](#modules-documentation).

### key : string | number

The `.key` property is created when a key is provided inside of your
[`.data`](#data--object) object. The `.key` property is used to keep
pointers to DOM nodes that existed previously to avoid recreating them
if it is unnecessary. This is very useful for things like list
reordering. A key must be either a string or a number to allow for
proper lookup as it is stored internally as a key/value pair inside of
an object, where `.key` is the key and the value is the
[`.elm`](#elm--element) property created.

If provided, the `.key` property must be unique among sibling elements.

For example: `h('div', {key: 1}, [])` will create a virtual node
object with a `.key` property with the value of `1`.

## Structuring applications

Snabbdom is a low-level virtual DOM library. It is unopinionated with
regards to how you should structure your application.

Here are some approaches to building applications with Snabbdom.

- [functional-frontend-architecture](https://github.com/paldepind/functional-frontend-architecture) –
  a repository containing several example applications that
  demonstrates an architecture that uses Snabbdom.
- [Cycle.js](https://cycle.js.org/) –
  "A functional and reactive JavaScript framework for cleaner code"
  uses Snabbdom
- [Vue.js](http://vuejs.org/) use a fork of snabbdom.
- [scheme-todomvc](https://github.com/amirouche/scheme-todomvc/) build
  redux-like architecture on top of snabbdom bindings.
- [kaiju](https://github.com/AlexGalays/kaiju) -
  Stateful components and observables on top of snabbdom
- [Tweed](https://tweedjs.github.io) –
  An Object Oriented approach to reactive interfaces.
- [Cyclow](http://cyclow.js.org) -
  "A reactive frontend framework for JavaScript"
  uses Snabbdom
- [Tung](https://github.com/Reon90/tung) –
  A JavaScript library for rendering html. Tung helps to divide html and JavaScript development.
- [sprotty](https://github.com/theia-ide/sprotty) - "A web-based diagramming framework" uses Snabbdom.
- [Mark Text](https://github.com/marktext/marktext) - "Realtime preview Markdown Editor" build on Snabbdom.
- [puddles](https://github.com/flintinatux/puddles) -
  "Tiny vdom app framework. Pure Redux. No boilerplate." - Built with :heart: on Snabbdom.
- [Backbone.VDOMView](https://github.com/jcbrand/backbone.vdomview) - A [Backbone](http://backbonejs.org/) View with VirtualDOM capability via Snabbdom.
- [Rosmaro Snabbdom starter](https://github.com/lukaszmakuch/rosmaro-snabbdom-starter) - Building user interfaces with state machines and Snabbdom.
- [Pureact](https://github.com/irony/pureact) - "65 lines implementation of React incl Redux and hooks with only one dependency - Snabbdom"
- [Snabberb](https://github.com/tobymao/snabberb) - A minimalistic Ruby framework using [Opal](https://github.com/opal/opal) and Snabbdom for building reactive views.
- [WebCell](https://github.com/EasyWebApp/WebCell) - Web Components engine based on JSX & TypeScript

Be sure to share it if you're building an application in another way
using Snabbdom.

## Common errors

```text
Uncaught NotFoundError: Failed to execute 'insertBefore' on 'Node':
    The node before which the new node is to be inserted is not a child of this node.
```

The reason for this error is reusing of vnodes between patches (see code example), snabbdom stores actual dom nodes inside the virtual dom nodes passed to it as performance improvement, so reusing nodes between patches is not supported.

```mjs
const sharedNode = h("div", {}, "Selected");
const vnode1 = h("div", [
  h("div", {}, ["One"]),
  h("div", {}, ["Two"]),
  h("div", {}, [sharedNode]),
]);
const vnode2 = h("div", [
  h("div", {}, ["One"]),
  h("div", {}, [sharedNode]),
  h("div", {}, ["Three"]),
]);
patch(container, vnode1);
patch(vnode1, vnode2);
```

You can fix this issue by creating a shallow copy of the object (here with object spread syntax):

```mjs
const vnode2 = h("div", [
  h("div", {}, ["One"]),
  h("div", {}, [{ ...sharedNode }]),
  h("div", {}, ["Three"]),
]);
```

Another solution would be to wrap shared vnodes in a factory function:

```mjs
const sharedNode = () => h("div", {}, "Selected");
const vnode1 = h("div", [
  h("div", {}, ["One"]),
  h("div", {}, ["Two"]),
  h("div", {}, [sharedNode()]),
]);
```

## Opportunity for community feedback

Pull requests that the community may care to provide feedback on should be
merged after such opportunity of a few days was provided.
