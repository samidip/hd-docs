---
uid: Uno.HotDesign.Canvas
---

# Canvas

The **Canvas** is the central design surface where you build and interact with your running app's user interface. It reflects the live visual output of your XAML page and allows you to select, move, edit, and organize controls directly. Actions you take on the Canvas are reflected in real time, making it the most visual and interactive part of **Hot Design**.

Whether you're adjusting layout, editing a `UserControl`, or testing structure with live feedback, the Canvas helps you work faster and more intuitively.

The Canvas shows your running app in **Application** mode. Switching to **Previews** mode on the left navigation bar shows your [previews](xref:Uno.HotDesign.Previews) on the same surface instead.

## Zoom, Scroll, and Pan

### Zoom

To zoom in or out of the **Canvas**:

- **Mouse**: Hold `Ctrl` (or `Cmd` on macOS) and scroll with your mouse wheel. The point under the cursor stays put as the canvas scales.
- **Keyboard**: Press `Ctrl +` / `Ctrl -` (or `Cmd +` / `Cmd -` on macOS) to zoom in or out in steps of 20 percentage points.

<img src="Assets/canvas-zoom.gif" height="600" alt="How to zoom in and out of the Canvas" />

Zoom is clamped to a range of **20% to 340%**. Setting a zoom level by any means — the slider, the percentage box, a preset, or `Ctrl` + wheel — turns **Auto-Fit** off. With Auto-Fit on, the zoom is computed so the whole design surface fits the visible canvas area, and the effective percentage is displayed even when it falls outside the manual range.

### Scroll and Pan

To move around the **Canvas** area:

- Scroll vertically using your mouse wheel.
- Scroll horizontally by holding `Shift` while using the mouse wheel.
- Pan in any direction by pressing the **middle mouse button** and dragging.

<img src="Assets/canvas-scroll.gif" height="600" alt="How to scroll the Canvas" />

Overlay scrollbars appear when the zoomed design surface is larger than the visible canvas area, and are disabled when everything fits.

## Resize the Design Surface

A **resize gripper** sits on each of the four edges of the device frame. Hovering a left or right gripper shows a west-east resize cursor; hovering a top or bottom gripper shows a north-south cursor.

Drag a gripper to resize the design surface continuously — the side grippers change the width only, the top and bottom grippers the height only. Drags map 1:1 to canvas pixels regardless of zoom, so a 10 px pointer move changes the canvas by 5 px at 200% zoom and by 20 px at 50%.

Starting a drag turns **Auto-Fit** off so the resize is not immediately re-fitted. Completing one switches the form factor to **User defined size** (Custom) and remembers the resulting dimensions, so switching to a preset and back to Custom restores them — including after restarting your app.

<!-- TODO(image): Screenshot of the design surface with the four edge resize grippers highlighted, and the west-east resize cursor shown on the right-hand gripper mid-drag. -->

> [!NOTE]
> Grippers are hidden while you are viewing a preview — resizing the application surface is not something a preview honours.

## Select Elements on the Canvas

To select a control or layout element, click it directly on the **Canvas**. The top-most selectable element under the pointer is chosen, and highlighted with a blue border known as a **visual adorner**.

![Canvas Selection](Assets/canvas-selection.png)

- Clicking an element that is **already selected** keeps it selected in preference to others under the pointer.
- Hold `Alt` and click to step to the element immediately **beneath** the current selection. Repeated `Alt` + clicks cycle through overlapping elements.
- The **middle mouse button** never changes the selection — it is reserved for panning.
- Regions with no selectable element — generated template content with no XAML identity, for example — leave the selection unchanged.

Your selection is re-established on the replacement element after a Hot Reload, so you do not have to re-select after a code change.

### Select Multiple Elements

Hold `Ctrl` (or `Cmd` on macOS) and click additional elements to select them together. This is useful for applying shared properties or moving groups of elements.

![Canvas Multi-selection](Assets/canvas-multi-selection.png)

### Hover Feedback

Moving the pointer over a selectable element draws a thin **hover outline** around it, so you can see what a click would select before you commit. Resting over an item generated from a `DataTemplate` additionally draws a dashed, warning-colored outline around that templated item.

Hover feedback is suppressed while a drag is in progress, and removed when the pointer leaves the canvas.

## Visual Adorner Details

When a single element is selected, a solid outline (the *visual adorner*) appears around it, showing:

- The control's name (e.g., `Button`, `TextBlock`)
- The control's size in pixels (width × height)
- The element's **margin and padding**, drawn as bands around and inside the outline

![Visual Adorner on Canvas](Assets/canvas-visual-adorner.png)

Hovering a side of a margin or padding band shows that side's value (for example **16 Padding**), so you can read the spacing without opening the **Properties** panel.

### Adjust Spacing and Size by Dragging

You can change these values on the canvas instead of typing them into the **Properties** panel. Select an element and drag:

- a **margin** or **padding** area to change that side's value
- the **spacing** guide between a container's children to change its `Spacing`, `RowSpacing` or `ColumnSpacing`
- the element's **edge** to change its `Width` or `Height`
- the **border** gripper, at the middle of that edge, to change its `BorderThickness`

Each area carries a small marker at its middle showing where to grab it. An area is a fixed size on screen, so it stays the same target at any zoom level and at any value — including zero, so you can introduce a margin, padding or spacing that is not there yet. A margin area sits outside the element and a padding area inside it, so the two are never confused, and the rest of each band stays available for selecting whatever lies underneath.

The element responds as you drag, so you can see the layout settle; your XAML is written once when you release, giving one change to undo. Values step in fours — `0`, `1`, `2`, `4`, `8`, `12` and so on — because layout values usually want to be multiples of four; hold **Shift** to move a pixel at a time. A badge beside the pointer shows the value and which property you are changing.

Press **Esc** mid-drag to abandon it: the element returns to the value it started at and nothing is written. If a change cannot be written, the element is put back and you are told, so the canvas never shows a value your source does not have.

> [!NOTE]
> Resizing an element whose size its container was deciding sets the size explicitly, so the element stops sizing itself to that container. Hot Design tells you when this happens, because nothing else on the canvas would show it.

When **two or more** elements are selected, each is drawn with an outline only — no label and no margin/padding bands.

Adorners follow the element they decorate: they track its rendered bounds as it moves, resizes, or the canvas zoom changes.

### Layout Guides for Containers

Some containers get affordances specific to them:

- **`Grid`** — dashed separator lines between rows and columns when spacing is zero, spacing indicators where row or column spacing is greater than zero, and a highlight over the usable content area inside the `Grid`'s padding.
- **`StackPanel`** — dashed separators between children when spacing is zero, and spacing indicators between them when it is not. A guide appears between each pair of **visible** children: a collapsed child takes up no space in the panel, so no gap is shown for it.
- **`SwipeControl`** — **Swipe Left** / **Swipe Right** buttons, so you can preview the swipe states without a touch gesture.

Any control with no specific adorner still gets the default outline.

![Canvas Layout Separation](Assets/canvas-layout-separator.png)

## Rearranging Elements with Drag and Drop

You can reposition controls directly in the layout:

1. Click and hold an element.
2. Drag it to a new location on the **Canvas**.
3. Release to drop it.

<img src="Assets/canvas-drag-drop.gif" height="600" alt="How to drag and drop on the Canvas" />

While dragging, a preview outline shows where the element will land and the prospective drop container is outlined with its margin and padding shown. Dropping in a new container or position moves the element in the underlying XAML and confirms the change; dropping it back where it started makes no edit at all. When the pointer is over a region with no valid drop container, the cursor indicates that dropping is not possible there.

A few details:

- Small, involuntary pointer movements do not start a drag — releasing performs a normal selection click instead.
- Holding `Ctrl` while dragging **copies** the element: a new copy of its XAML is inserted at the drop position and the original stays in place.
- An element that is not editable in XAML — one with no XAML source position, or presented content — cannot be dragged.

## Wrap an Element with a Parent Container

To surround an existing element with a new layout container:

1. Right-click the element.
2. Choose **Add parent**.
3. Select a parent control: `Border`, `Canvas`, `Grid`, `RelativePanel`, `ScrollViewer`, `StackPanel`, or `Viewbox`.

The new parent will be added to the visual tree and reflected on the **Canvas**.

![Canvas Add Parent](Assets/canvas-add-parent.png)

## Select an Element's Parent

If you're trying to select a layout container of a specific control:

1. Right-click the child element.
2. Choose **Select parent** from the context menu — or press `Shift + Enter`.

This selects the parent element on both the **Canvas** and in the **Elements** panel.

<img src="Assets/canvas-select-parent.gif" height="450" alt="How to select a parent element on the Canvas" />

## Delete an Element

To remove a control from the **Canvas**:

1. Right-click the element.
2. Choose **Delete [ControlName]** from the menu (e.g., **Delete Button**).

Or select one or more elements and press `Delete`.

The element will be removed from both the **Canvas** and the **Elements** panel.

<img src="Assets/canvas-delete-element.gif" height="450" alt="How to delete an element from the Canvas" />

## The Context Menu

Right-clicking a selectable element opens a context menu for the element under the pointer — an already-selected element under the pointer takes precedence. The menu offers the actions that apply to that element: **Delete** (when the element is editable in XAML), **Add parent**, **Select parent**, **Edit \<UserControl>**, and **Edit \[\<template property>]** for an element inside a data template.

Holding `Ctrl + Shift` while right-clicking adds advanced entries to the standard ones.

Right-clicking never starts an accidental drag — the pointer and drag state are reset when the menu opens.

## Double-Click Actions

Double-clicking on the canvas navigates the editing context:

| Double-click on | Result |
| --- | --- |
| An item generated from a `DataTemplate` | Opens that template in the [Template Editor](xref:Uno.HotDesign.Properties.TemplateEditor) |
| A `UserControl` belonging to your project | Opens it in its own editing scope |
| An area outside the current editor's content | Closes the current editor and returns to the parent scope |

## Edit a UserControl from the Canvas

A `UserControl` is a reusable component that encapsulates a set of UI elements and their associated behavior. It's commonly used to organize parts of your interface into self-contained, maintainable units that can be reused across different parts of your application.

If your layout includes a `UserControl`, you can open it directly from the **Canvas** for editing:

1. Right-click the `UserControl`.
2. Choose **Edit [UserControlName]** — or double-click it, or press `Ctrl + Shift + U` with it selected.

This opens the editor for the `UserControl`, allowing you to modify its internal structure or layout. To return to your previous page edition, click the `../` back icon in the top-left corner of the interface or the **Elements** panel.

<img src="Assets/canvas-edit-user-control.gif" height="600" alt="How to edit a UserControl from the canvas" />

When you close a `UserControl` editor, **every live instance** of that `UserControl` is refreshed — not just the one you were editing — so your change is visible everywhere it is used. Closing an editor in which you changed nothing runs no refresh at all.

## Edit Flyouts from the Canvas

You can edit flyouts in Hot Design in two main ways.

### Using the Property Window

You can locate the `Flyout` property in the **Property Window** and edit it using the **Complex Type Editor**. To learn more about how the Complex Type Editor works, refer to the [Complex Type Editor documentation](xref:Uno.HotDesign.Properties.Editors#complex-types).

### Using the Flyout Editor

The **Flyout Editor** allows you to edit the contents of the flyout directly. You can add, remove, or modify individual elements inside the flyout.
To open the Flyout Editor, follow these steps:

1. In the **Toolbar**, click the **More Options** button (three dots).
2. Hover over the **Window** menu and uncheck the option **Show tools in-app**.
   This will move the Toolbox, Elements window, and Property window to external windows, leaving only the Canvas in the main window.
3. Enable **Interactive Mode** by clicking the **Play** button in the toolbar.
4. Perform the action that opens the flyout (e.g., click the button that has a flyout).
5. With the flyout still opened, disable **Interactive Mode** by clicking the **Pause** button in the Toolbar.
   This will activate the **Flyout Editor**.

Now you can freely make any changes to the flyout.

To exit the **Flyout Editor**, you can:

- Click the **Enter Interactive Mode** button in the Toolbar (Play icon)
- Or click the **Back** icon in the top-left corner of the **Canvas** or **Tree window**

> [!NOTE]
> In external tool windows, zoom features are disabled. This means you won't be able to adjust form factor, zoom, or scroll while editing a Flyout.
> [!TIP]
> **UserControl** and **DataTemplate** editors are fully supported inside the Flyout Editor, so you can edit them directly within the Flyout context.
> [!IMPORTANT]
> Editing *nested flyouts* is not supported at the moment.

## The Device Frame and Canvas Appearance

The design surface is wrapped in a **device frame** whose chrome matches the selected form factor: the phone-class form factors (**Narrowest** and **Narrow**) show a small-device bezel, and every other form factor shows the larger-device frame. The frame is not shown while a nested template editor is open.

The area around the frame is filled with the designer canvas color — `#ECEDEE` in light theme and `#2E3438` in dark. There is no checkered background pattern.

Application content that paints outside its layout bounds — a full-screen background or a particle effect, say — is clipped at the design surface boundary, so it never renders over the device frame chrome. The clip follows the application area as the frame is resized or the form factor changes.

A fixed inset is kept between the design surface and the edge of the canvas viewport, with extra space at the bottom while a preview is open so content never renders under the **Edit** overlay button.

## Interactive Mode

Hot Design's **Selection / Interactive** toggle decides whether canvas clicks select elements or drive your running app.

Turning **Interactive** on (from the toolbar, or with `Ctrl + Shift + I`) passes pointer input straight through to your application: buttons work, navigation works, animations run. Selection chrome is hidden, the current selection is cleared, and any open nested editors are closed first. Turning it off rebuilds the element hierarchy so the design surface reflects wherever you navigated to, and selection resumes.

> [!NOTE]
> While the toggle is on, the **Toolbox**, **Elements**, and **Properties** panels are covered by a dimming overlay — the canvas and the top toolbar stay interactive. Tapping a covered panel returns you to selection mode.

The toggle is not available on the [previews](xref:Uno.HotDesign.Previews) canvas, which has its own interaction handling.

If your page still carries design-time data, turning Interactive on asks you to confirm first.

## Keyboard Shortcuts

The canvas responds to the shortcuts listed in full on the [Shortcuts](xref:Uno.HotDesign.Shortcuts) page. The ones you will reach for most:

| Action | Shortcut |
| --- | --- |
| Delete the selection | `Delete` |
| Select the parent element | `Shift + Enter` |
| Edit the selected `UserControl` | `Ctrl + Shift + U` |
| Undo / redo | `Ctrl + Z` / `Ctrl + Y` |
| Fit content to the canvas | `Ctrl + 0` |
| Zoom to 100% / 200% / 300% | `Ctrl + 1` / `2` / `3` |
| Toggle Auto-Fit | `Ctrl + 9` |
| Toggle Interactive mode | `Ctrl + Shift + I` |

## Helpful Notifications

While designing on the Canvas, notifications may appear:

- When changes are applied, a loading indicator may display briefly. Changes that need the running app to be rebuilt show an **Updating App** overlay while a Hot Reload runs.
- If an error occurs (e.g., invalid XAML), an error notification appears in the bottom-right corner.

These notifications help you track changes and issues in real time.

## Next Step

- [Properties](xref:Uno.HotDesign.Properties)
