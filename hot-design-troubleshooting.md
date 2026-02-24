---
uid: Uno.HotDesign.Troubleshooting
---

# Hot Design Troubleshooting

This guide lists known issues and troubleshooting steps for **Hot Design**.
If you encounter a problem not listed here, please [file a bug report](https://github.com/unoplatform/studio/issues).

## Known issues

### All Platforms

#### External Tool Window Lost or Hidden

- **Description:** The external tool window opened but is now off-screen or minimized.
- **Cause:** The window may have been positioned outside the visible screen area or minimized.
- **Solution:**
  - Click **Overflow Menu** > **Window** > **Show Tools In-App** to close the external window.
  - Then toggle it again **Show Tools In-App** to move tools back to the main view.
  - Alternatively, check your taskbar to see if the window is minimized and restore it.

#### Tools and External Window Not Syncing

- **Description:** Changes made in the app aren't reflected in the external tool window, or vice versa.
- **Cause:** Communication between the external window and main app was interrupted.
- **Solution:**
  - Check your connection status in the toolbar (connection icon).
  - If offline or disconnected, reconnect and try the action again.
  - Close the external window and reopen it by toggling: **Overflow Menu** > **Window** > **Show Tools In-App**.
  - If the issue persists, restart Hot Design.

#### Image Attachment Not Working in Chat

- **Description:** When clicking the file picker button in Chat, nothing happens, or images won't attach.
- **Cause:** File picker may not be initialized, or file permissions are blocked.
- **Solution:**
  - Ensure you have read permissions on your file system.
  - Verify that you're using a [supported image format](xref:Uno.HotDesign.ImageAttachment) (.jpg, .jpeg, .png).
  - Close and reopen the Chat panel, then try again.

#### Native Controls Disappear on Hover

- **Description:** Native controls (such as WebView2, MediaPlayerElement, or other platform-native elements) disappear or become invisible when hovering over them with the mouse in Hot Design mode.
- **Cause:** This occurs when non-empty managed overlays (such as visual adorners shown on hover) are displayed on top of native controls. In this scenario, the native embedding clips the entire native surface (including the control itself), causing the control to become invisible.
- **Workaround:** This is a known limitation of how native content is clipped when combined with managed overlays. There is currently no direct solution except to:
  - Avoid hovering over native controls while Hot Design mode is enabled to prevent overlays from being displayed.
  - Temporarily disable Hot Design mode when working with native controls.
  - Use alternative non-native controls where possible (e.g., Skia-rendered controls).
- **Related Issue:** This is the same root cause as [unoplatform/uno#21309](https://github.com/unoplatform/uno/issues/21309).

### Linux / WSL

#### Window Maximize/Restore State Not Preserved

- **Description:** When using Hot Design on Linux or WSL, the window maximize/restore state is not preserved across sessions.
- **Cause:** The underlying windowing API (`OverlappedPresenter.State`) is not reliable on Linux/WSL and causes a native X11 error when accessed, so Hot Design skips reading it.
- **Workaround:** Window size is still tracked and preserved. Only the maximize/restore state is not remembered between sessions. You can manually maximize or restore the window each time you activate Hot Design.
- **Related Issue:** [#6234](https://github.com/unoplatform/uno.hotdesign/issues/6234)

## Contributions and Feedback

If you encounter recurring issues or specific scenarios not covered in this guide, please consider the following:

- [File a bug report](https://github.com/unoplatform/studio/issues).
- For details on how to share feedback and suggestions, see [Providing Feedback for Uno Platform Studio](xref:Uno.Platform.Studio.Feedback).

Your input matters, it helps us continuously improve the **Hot Design** experience for everyone.
