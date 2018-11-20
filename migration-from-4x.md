---
layout: default
title: Migrating from TinyMCE 4.x to TinyMCE 5.0.
title_nav: Migrating from 4.x
description: Instructions for migrating from TinyMCE 4.x to TinyMCE 5.0.
keywords: migration considerations premigration pre-migration
---

## Migrating from 4.x

The new TinyMCE 5.0 editor comes with significant changes to the previous versions. Most configuration changes in TinyMCE 5.0 only affect complex use cases like adding custom components. Setting up a basic TinyMCE 5.0 instance should be the same as TinyMCE 4.x.

Our team at Tiny has worked on creating a more configurable, more modern, and less cumbersome editor while retaining the familiarity of the user interface from the older versions.

This chapter describes the migration process and workarounds if you are using an older version of TinyMCE. It includes tasks that you must perform before the migration can begin, and different workaround procedures for deprecated, deleted, and updated features.

## Editor-Core

### Initialization

The initialization process of TinyMCE 5.0 is the same as TinyMCE 4.x. The bootstrap process and initialization events all remain the same. Please see the [Quick Start]({{site.baseurl}}/quick-start) section for more information on setup. The main differences are in the `init` configuration, specifically the configuration items for UI components. It still retains a familiar JSON structure. However, many of the configuration options have been simplified.

### Settings

In TinyMCE 5.0, some configurations have been removed because they are no longer necessary or an improved solution has been introduced.  All inline style configurations have been removed in TinyMCE 5.0 in favor of modern CSS.  This affects all TinyMCE 4.x UI component configurations.

#### Added Settings

**custom_colors** is `true` by default. When it is set to `false`, it will turn off the custom color picker in the color swatch.

#### Removed Settings

##### Removed Editor Core Settings

**fixed_toolbar_container**: Owing to the enhancements to the new inline toolbar behaviour, `fixed_toolbar_container` is not required in TinyMCE 5.0.

##### Removed Component Configuration Settings

| **Old Settings** | **Description** | **Alternative** |
| ---------------- | --------------- | --------------- |
| flex | Sets an inline css value for the component | Use CSS stylesheets for custom styling |
| border | Sets an inline css border for the component | Use CSS stylesheets for custom styling |
| layout | Defines a layout | Use the new TinyMCE 5 UI components to compose your custom layout |
| spacing | Sets spacing for the component | Use CSS stylesheets for custom styling |
| padding | Sets an inline css padding value for the component | Use CSS stylesheets for custom styling |
| align | Sets an inline css align property for the component | Use CSS stylesheets for custom styling |
| pack | Emulates flex pack | Use CSS stylesheets for custom styling |

Please see the [TinyMCE 4.x docs](https://www.tiny.cloud/docs/) for more information on the above settings.

##### Removed Plugin Settings

**textcolor_rows**: The textcolor plugin was removed and this setting is not required in TinyMCE 5.0.

#### Changed Settings

##### Changed Editor Core Settings

| **Setting** | **TinyMCE 4.x** | **TinyMCE 5.0** |
| ----------- | --------------- | --------------- |
| `height` | Set the height of the editable area of the editor | Sets the overall height of the editor, including the menubar, toolbars, and statusbar |
| `height` | Only supported number values | Supports numbers and strings, and assume a string is a valid CSS value for height |
| `width` | Only supported number values | Supports numbers and strings, and assume a string is a valid CSS value for width |
| `resize` | `true` by default | `true` by default if the `autoresize` plugin isn't enabled, `false` by default if the `autoresize` plugin is enabled |

##### Changed Plugin Settings

| **TinyMCE 4.x** | **TinyMCE 5.0** |
| `autoresize_min_height` | `min_height` |
| `autoresize_max_height` | `max_height` |
| `textcolor_cols` | `color_cols` |
| `textcolor_map` | `color_map` |


### Methods

* All TinyMCE 4.x methods for creating UI components have been removed. New methods have been added for TinyMCE 5.0. For more information, see the [docs]({{site.baseurl}}/ui-elements/).
* No core editor methods were removed (tinymce, editor, selection, on(), etc remain the same).

## Themes

In TinyMCE 5.0, some themes have been removed and are now combined in a new single responsive theme called **Silver**.

| **Removed Theme** | **Replaced by** |
| ----------------- | --------------  |
| [Modern](https://www.tiny.cloud/docs/themes/modern/) | Silver |
| [Modern inline](https://www.tiny.cloud/docs/general-configuration-guide/use-tinymce-inline/) | Silver Inline |
| [Inlite(Distraction-free Editor)](https://www.tiny.cloud/docs/themes/inlite/) | Silver (distraction free configuration) |
| [Mobile](https://www.tiny.cloud/docs/themes/mobile/) | Silver (responsive to small screen touch devices)  |

### Removed Themes

#### Inlite

The Inlite theme is no longer supported in TinyMCE 5.0. The features that the Inlite theme used to provide are now available as a plugin. For a workaround, use the following configuration:
```
{
  theme: 'silver',
  inline: true,
  toolbar: false,
  plugins: [ 'inlite' ]
}
 ```
This will provide a similar but improved distraction free experience in TinyMCE 5.0.

#### Modern

The Modern theme is no longer supported in TinyMCE 5.0.  The modern themes Ui library `tinymce.ui.*` has also been deleted. This change may impact integrations depending upon the level of customization.

For changes required, refer to the following table of examples:

| Customization Level | Description | Impact |
| ------------------- | ----------- | ------ |
| Minor | Some custom buttons | No UI fixes required. Updating the `addButton` configuration to TinyMCE 5.0 format. |
| Moderate | A [dialog]({{site.baseurl}}/ui-elements/dialog/#dialoginstanceapi) created using `editor.windowManager.open` configuration objects | Convert TinyMCE 4.x config to TinyMCE 5.0 config. |
| Major | Completely custom dialogs and extended use of the Modern UI framework | The new TinyMCE 5.0 components may not  cover all API use cases. However, we will strive to create supported workarounds or if there are sufficient requests and add a new component to resolve the use case. |

> Note: Please provide feedback on your use case and your Tinymce 4.x configuration containing only the UI component that you wish to be supported or need a workaround. You can also track developer preview issues on GitHub, [here](https://github.com/tinymce/tinymce/labels/dev%20preview).

> Note: In TinyMCE 5.0 `Silver` is enabled by default. If you do not specify a theme, it will default to Silver. The Silver theme contains a set of configurable UI components that could be used to replace the current customizations (modern, inline, and inlite theme).

### Events

#### Removed Events

| **Removed Event** | **Description**|
| ----------------- | -------------- |
| BeforeRenderUi | Fired before the UI was rendered. |


## Components

### Method Namespacing Changes

The methods for registering components have moved to a different part of the editor API. They are now in the UI Registry part of the editor API. For example, `editor.addButton(identifier, configuration)` is now `editor.ui.registry.addButton(identifier, configuration)`. See the relevant sections below for more information.

### Custom Toolbar Buttons

The methods for adding [Custom Toolbar]({{site.baseurl}}/ui-elements/toolbarbuttons/#howtocreatecustomtoolbarbuttons) buttons have changed significantly between TinyMCE 4.x and TinyMCE 5.0.

* Toolbar button types have changed from -  basic, split, listbox, and menu,  to -  basic, toggle, split, and menu.
* Methods for creating toolbar buttons have been moved from `editor.*` to `editor.ui.registry.*`.
* Explicit methods have been added for creating each type of toolbar button.

#### Changed Methods

Some of the methods from TinyMCE 4.x for creating custom toolbar buttons have been moved and re-implemented to use the TinyMCE 5.0 style of configuration. For more information on how these methods have changed, see [docs]({{site.baseurl}}/ui-elements/toolbarbuttons/).

| **Old Method** | **New Method** |
| -------------- | -------------- |
| editor.addButton() | editor.ui.registry.addButton() |
| editor.addMenuItem() | editor.ui.registry.addMenuItem() |


#### New Methods

New methods have been added for creating other common types of toolbar buttons. For example, to create a toggle button In TinyMCE 5.0 we would use `editor.ui.registry.addToggleButton()`. Each of these methods takes configurations specific to their type, to simplify implementation.

| **New Method** | **Description** |
| -------------- | --------------- |
| editor.ui.registry.addToggleButton() | Adds a custom toolbar toggle button. |
| editor.ui.registry.addSplitButton() | Adds a custom toolbar split button. |
| editor.ui.registry.addMenuButton() | Adds a custom toolbar menu button. |

For more information on how these methods have changed, see [docs]({{site.baseurl}}/ui-elements/typesoftoolbarbuttons/).

### Removed Toolbar Button Types

**Listbox** is no longer a supported toolbar button type in TinyMCE 5.0. Though listbox has been removed, any functionality provided by custom listbox toolbar buttons can be retained by switching to a different type of toolbar button.

Any custom listbox toolbar buttons can be converted to a different type of toolbar button using the new methods. The recommended toolbar button type to switch to is the**Split** button.

### Configuration Differences between TinyMCE 4.x and TinyMCE 5.0:

| Old Method | New Method | Description |
| -----------| ---------- | ----------- |
| `onclick` | `onAction` | `onclick` is now `onAction`. `onAction` now has an API to provide some helper functions to the user. |
| `cmd` | `onAction` | `cmd` has been removed as a configuration option. Commands should be executed through `onAction` now. |
| `onpostrender` | `onSetup` | `onPostRender` has been removed, and instead you should use `onSetup`.  |

#### onclick → onAction

`onclick` is now `onAction`. The callback function given to onAction should take a `buttonApi` argument that's passed to the onAction callback is an object that contains some helper functions. Each type of toolbar button has a different set of API functions. See [docs]({{site.baseurl}}/ui-elements/typesoftoolbarbuttons/) for more information.

 Example:

##### TinyMCE 4.x:

```js
editor.addButton('mybutton', {
  text: "My Button",
  onclick: () => alert("My Button clicked!")
});
```
###### TinyMCE 5.0:

```js
editor.ui.registry.addButton('myButton', {
  text: 'My Button',
  onAction: (buttonApi) => alert('My Button clicked!')
});
```
#### cmd → onAction

`cmd: string` has been removed as a configuration options, and commands should be executed in `onAction` instead. Example:

##### TinyMCE 4.x:

```js
editor.addButton('mybutton', {
  text: "My Button",
  cmd: 'mceSave'
});
```
##### TinyMCE 5.0:

```js
editor.ui.registry.addButton('myButton', {
  text: 'My Button',
  onAction: (_) => editor.execCommand('mceSave')
});
```
#### onpostrender → onSetup

`onPostRender` has been removed, and instead, you should use `onSetup`. There are 3 major changes:

* While [`onpostrender`](https://www.tiny.cloud/docs/advanced/creating-a-custom-button/#togglebutton) only ran once, when the editor was created, [`onSetup`]({{site.baseurl}}/ui-elements/typesoftoolbarbuttons/#basicbuttonexampleandexplanation) runs every time a component is rendered, e.g. for a menu item, every time its menu becomes visible.  `onSetup` now has an API containing some helper functions. Each type of toolbar button has a different API.
* You can configure `onSetup` to return a function, which will be automatically be called `onTeardown` of the component, e.g. when a menu item's menu is closed.
  * If `onSetup` listens to any events using editor.on(eventName, callback) it should return a editor.off(eventName, callback) callback to unbind the event on teardown. This is particularly important if `onSetup` listens to any events using `editor.on(eventName, callback)`. Unless the event was `'init'`, `onSetup` should return `(buttonApi) => ed.off(eventName, callback)`. The **teardown** callback function will automatically be called by the editor when necessary.
  * If some functionality should only run when the editor is first initialized, it should be passed to `editor.on('init', callback)` as the callback function.

Example:

#####  TinyMCE 4.x:

```js
editor.addButton('currentdate', {
  icon: 'insertdatetime',
  tooltip: "Insert Current Date",
  onclick: insertDate,
  onpostrender: function monitorNodeChange() {
    var btn = this;
    editor.on('NodeChange', function(e) {
      btn.disabled(e.element.nodeName.toLowerCase() == 'time');
    });
  }
});
```
#####  TinyMCE 5.0:

In this example, the button's API contains `isDisabled: () => boolean` and `setDisabled: (state: boolean) => void`.

```js
editor.ui.registry.addButton('customDateButton', {
  icon: 'insert-time',
  tooltip: 'Insert Current Date',
  disabled: true,
  onAction: (_) => editor.insertContent(toTimeHtml(new Date())),
  onSetup: (buttonApi) => {
    const editorEventCallback = (eventApi) => buttonApi.setDisabled(eventApi.element.nodeName.toLowerCase() === 'time');
    editor.on('NodeChange', editorEventCallback);
    return (buttonApi) => editor.off('NodeChange', editorEventCallback);
  }
});
```
> Note: The callback function given to `onSetup` should take a `buttonApi` argument which is an object that contains some helper functions.

### Custom Menu Items

##### New methods:

| **New Method** | **Description** |
| ----------- | -------------- |
| editor.ui.registry.addMenuItem() | Adds a custom basic menu item. |
| editor.ui.registry.addToggleMenuItem() | Adds a custom toggle menu item. |

Docs are coming soon!

### Custom Dialogs

Dialogs no longer have `height` or `width` settings. The dialog component now uses CSS3 and a predefined `small`, `medium`, and `large` template to specify the dimensions. See [docs]({{site.baseurl}}/ui-elements/dialog/), for more information.

### Custom Sidebars

Docs are coming soon!

### Custom Context Toolbars

The Context Toolbar takes its buttons the registry of toolbar buttons added using `addButton`, `addToggleButton`, `addSplitButton` or `addMenuButton`. The method for creating custom context toolbars has also been moved from `editor.addContextToolbar()` to `editor.ui.registry.addContextToolbar()`.

For more information on Context Toolbars, see [docs]({{site.baseurl}}/components/contexttoolbar).

#### Changes between TinyMCE 4.x and TinyMCE 5.0:

* Buttons go before and after the input in TinyMCE 4.x.
* The `Ctrl+K` shortcut does nothing until the context toolbar is visible in TinyMCE 4.x.
* In TinyMCE 5.0, the pop animates to its new width.
* In TinyMCE 4.x., it is a URL input, so you get a popup and a browse button.

### Custom Context Menus

The default [Context Menu](https://www.tiny.cloud/docs/plugins/contextmenu/) is no longer a plugin, it is part of the core and always enabled. Where TinyMCE 4.x only supported adding registered menu items, the new context menu also allows plugins to register "sections" of the context menu. These sections are dynamic and may show or hide depending on the cursor position when the context menu is opened. For example, the default context menu config is now `'link image editimage table spelling'` which are all plugin section references, not menu items. See [docs]({{site.baseurl}}/components/contextmenu/)

##### New methods:

| **New Method** | **Description** |
| ----------- | -------------- |
| editor.ui.registry.addContextMenu() | Adds a custom context menu. |

For more information on Context Menus, see the [docs]({{site.baseurl}}/components/contextmenu).

### Custom Context Forms

A ContextForm consists of an input field, and a series of related buttons. Context forms can be shown wherever a context toolbar can be shown. Also, when a context form is registered containing a `launch` configuration, a special context toolbar button with name `form:${name}` is registered which will launch that particular context form. ContextForms are a generalisation of the `Insert Link` form that existed in the original [inlite](https://www.tiny.cloud/docs/themes/inlite/#quicklink) theme from TinyMCE 4.

## Plugins

Each release of TinyMCE adds new features and functionality. We also occasionally remove features and functionality, usually because we've added a better option.
Here are the details about the features and functionalities that we removed in TinyMCE 5.0.

### Removed Features

Plugins with dialogs no longer have `height` or `width` settings for their dialogs. The dialog component now uses CSS3 and a predefined `small`, `medium`, and `large` template to specify the dimensions.

The following plugins from TinyMCE 4.x do not require height or width options to be specified in TinyMCE 5.0:

* [Code]({{site.baseurl}}/plugins/code/)
* [Codesample]({{site.baseurl}}plugins/codesample/)
* [Preview]({{site.baseurl}}plugins/preview/)
* [Template]({{site.baseurl}}plugins/template/)

Please read the [docs]({{site.baseurl}}/plugins/) if you need more information on the Tiny 4.x configuration options.

### Changed Features

These features have either changed or have been deleted in TinyMCE 5.0.
Each release of TinyMCE adds new features and functionality. We also occasionally remove features and functionality, usually because we’ve added a better option. Here are the details about the features and functionalities that we removed in TinyMCE 5.0.

#### Moved Plugins

| **Plugin Name** | **Description** |
| --------------- |  -------------- |
| [ContextMenu](https://www.tiny.cloud/docs/plugins/contextmenu/) | New API. See [docs]({{site.baseurl}}/components/contextmenu/). |
| [ColorPicker](https://www.tiny.cloud/docs/plugins/colorpicker/) | Moved to the core. See [docs]({{site.baseurl}}/configure/content-appearance/#color_picker}}). |

### Table

Changes between TinyMCE 4.x and TinyMCE 5.0:

* Styles text field has been removed from the advanced table of the dialogs. This simplifies the dialogs for users and gives the editor stricter control over the table styles which means we are better able to ensure the styles are correct.
* Improved how styles are set and retrieved from tables, rows, and cells, so this should be more reliable now.
* Shifted to using CSS more for styling, and therefore removed a few legacy data attributes that were set on tables/rows/cells which are no longer the good practice to use. This makes the output HTML cleaner and more modern.
* When opening a properties dialog with a single table/row/cell selected, the dialog will autofill with the relevant existing values. If you select multiple rows or cells and open the relevant properties dialog, TinyMCE 4.x will leave all the dialog fields blank. In TinyMCE 5.0, any fields which have the same values for all the selected rows or cells will autofill, and the fields which have no existing value or have differing values will be empty.
* `Border` input field in the `tableprops` dialog is now called `Border width`, for better clarity.


## Mobile Support
* TinyMCE 4.x introduced mobile support, bundled with a new theme and configuration settings.
* TinyMCE 5.0 makes this process seamless, where mobile support comes out of the box without additional configurations.  The new theme is now responsive on tablets, where the classic desktop theme will be displayed, and on smaller devices like phones, a touch Ui will be displayed. TinyMCE 5.0 mobile will be an exciting space to watch.
