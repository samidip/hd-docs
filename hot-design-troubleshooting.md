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

## Contributions and Feedback

If you encounter recurring issues or specific scenarios not covered in this guide, please consider the following:

- [File a bug report](https://github.com/unoplatform/studio/issues).
- For details on how to share feedback and suggestions, see [Providing Feedback for Uno Platform Studio](xref:Uno.Platform.Studio.Feedback).

Your input matters, it helps us continuously improve the **Hot Design** experience for everyone.
