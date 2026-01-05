---
uid: Uno.HotDesign.Troubleshooting
---

# Hot Design Troubleshooting

This guide lists known issues and troubleshooting steps for **Hot Design**.
If you encounter a problem not listed here, please [file a bug report](https://github.com/unoplatform/uno.studio/issues).

## Known issues

### All Platforms

#### Configuring Automatic Hot Design Launch for Automation

- **Description:** By default, Hot Design launches automatically in Debug mode. For CI, App MCP, and other automation scenarios, this behavior needs to be disabled.
- **Solution:** You can control launch behavior using either an environment variable or an MSBuild property. When set, these take precedence over the `launchHotDesignOnStart` parameter in code.

  **Option 1: Environment Variable** (highest priority, set at runtime)
  
  Set the environment variable `UNO_HOTDESIGN_LAUNCH` to control launch behavior. Set it to `false` or `0` to disable automatic launch, or `true` or `1` to enable it.
  
  ```bash
  # Disable automatic launch (recommended for automation)
  # On Windows (PowerShell)
  $env:UNO_HOTDESIGN_LAUNCH = "false"
  
  # On Windows (Command Prompt)
  set UNO_HOTDESIGN_LAUNCH=false
  
  # On macOS/Linux
  export UNO_HOTDESIGN_LAUNCH=false
  
  # Enable automatic launch
  export UNO_HOTDESIGN_LAUNCH=true  # or "1"
  ```

  **Option 2: MSBuild Property** (set in project file)
  
  Add the `UnoHotDesignLaunch` property to your project file (`.csproj`):
  
  ```xml
  <PropertyGroup>
    <!-- Disable Hot Design automatic launch -->
    <UnoHotDesignLaunch>false</UnoHotDesignLaunch>
  </PropertyGroup>
  ```
  
  Or enable it:
  
  ```xml
  <PropertyGroup>
    <!-- Enable Hot Design automatic launch -->
    <UnoHotDesignLaunch>true</UnoHotDesignLaunch>
  </PropertyGroup>
  ```

  **Priority Order:**
  1. Environment variable `UNO_HOTDESIGN_LAUNCH` (highest priority)
  2. MSBuild property `UnoHotDesignLaunch`
  3. Code parameter `launchHotDesignOnStart` in `UseStudio()` call

#### Multi-Window Support Unavailable

- **Description:** Hot Design does not currently support multiple windows.
- **Workaround:** Define the UI for additional windows manually in code.

#### Editing Flyouts (Limited Support)

- **Description:** Editing Flyouts is supported but limited. Nested Flyouts are not supported.
- **Related Discussion:** [#7](https://github.com/unoplatform/studio/discussions/7)
- **Workaround:** For nested Flyouts, define them manually in code or use the **Complex Editor** in the Properties panel.

#### Editing ContentDialogs

- **Description:** Editing `ContentDialog` is not currently supported.
- **Related Discussion:** [#7](https://github.com/unoplatform/studio/discussions/7#discussioncomment-11589999)
- **Workaround:** Define dialogs manually in code.

#### Inline Elements in TextBlock Not Recognized

- **Description:** Inline elements in `<TextBlock>` are only recognized when declared inside `<TextBlock.Inlines>`.
- **Workaround:** Always wrap inline content inside `<TextBlock.Inlines>…</TextBlock.Inlines>`.

#### Markup Extensions (Basic Support Only)

- **Description:** Markup Extension values can be viewed and cleared in the Properties panel, but editing is limited.
- **Workaround:**
  - Use **Reset** to clear and reopen the property flyout before editing.
  - Enter **Interactive Mode** or restart **Hot Design** to preview changes.
  - For advanced editing, modify directly in XAML.

### WebAssembly

#### Performance When Debugging

- **Description:** When debugging on WebAssembly, Hot Design may perform more slowly.
- **Workaround:** Run the app **without the debugger attached** for smoother performance.

### Android

#### Emulator Session Switching

- **Description:**
  If Hot Design is used in Application A and the app is closed in the emulator, launching Application B afterward will cause Hot Design to not function correctly.
- **Workaround:**
  1. Open Solution B in your IDE (e.g., Visual Studio).
  2. Run the app again in the emulator.
  3. If the issue persists, fully close and restart the emulator.

## Contributions and Feedback

If you encounter recurring issues or specific scenarios not covered in this guide, please consider the following:

- [File a bug report](https://github.com/unoplatform/studio/issues).
- Contribute improvements to this guide through pull requests.
- For details on how to share feedback and suggestions, see [Providing Feedback for Uno Platform Studio](xref:Uno.Platform.Studio.Feedback).

Your input matters, it helps us continuously improve the **Hot Design** experience for everyone.
