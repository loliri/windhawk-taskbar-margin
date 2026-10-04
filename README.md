# Taskbar Margin

A [Windhawk](https://windhawk.net/) mod for Windows 11 which shifts the taskbar
content to the right by a configurable amount, leaving an empty margin on the
left. The taskbar background stays full width, and the taskbar context menu
follows the content.

## What it does

- Moves the taskbar's buttons, icons and tray area to the right by a configurable
  number of pixels.
- Keeps the taskbar background full width.
- Moves the taskbar context menu (the jump list) by the same amount, so it stays
  aligned with the taskbar buttons.

## Settings

| Setting | Default | Description |
| --- | --- | --- |
| Left margin (pixels) | 220 | How much empty space to leave on the left of the taskbar content. |
| Follow display DPI | on | Scale the margin with the display DPI, so it keeps the same visual size on high-DPI displays. Turn off to keep it at a constant physical pixel size. |
| Displays | All | Which displays the margin applies to: all, only the primary one, or only the secondary ones. Each display has its own taskbar and its own DPI, so the margin is calculated per display. |

## Notes

Requires Windows 11.

The taskbar must be **left-aligned** (Settings → Personalization → Taskbar →
Taskbar alignment). With centered alignment, the buttons move by only part of
the margin while the context menu moves by all of it, so the two no longer line
up.

The taskbar is not mirrored correctly on right-to-left display languages: the
margin is applied to the physical left regardless of the taskbar's flow
direction, while the context menu is moved to the right.

## Compatibility

- **Windows 11 Taskbar Styler** can be used alongside this mod. This mod reads
  the taskbar's XAML tree directly instead of going through XAML diagnostics, so
  it does not compete with the Styler for the single XAML diagnostics consumer
  slot that Explorer allows. The Styler can also set the same padding itself,
  but it cannot move the context menu, which is why this mod exists. Note that
  some Styler themes set the same two properties this mod does, such as DockLike
  (`RootGrid` padding) and Surface (the taskbar background margin). With one of
  those themes, whichever of the two mods writes last wins, and disabling this
  mod clears the value the theme had set.
- **TranslucentTB** is confirmed compatible and can be used alongside this mod.
- Tested on Windows 11 26H2.

## Suggested use

Together with [FluentFlyout](https://github.com/unchihugo/FluentFlyout): enable
the taskbar widget there, set its position to the bottom left corner, and turn on
the fixed widget width. The taskbar elements then tile linearly instead of
overlapping each other.

## How the context menu is positioned

The taskbar context menu is not laid out by XAML. Its anchor point is computed in
`explorer.exe` by `CTaskListWnd::_ComputeJumpViewPosition` in `taskbar.dll`, and
handed to the process that draws the menu. The point is in physical screen
pixels.

Because of that, shifting the taskbar's XAML content does not move the menu — the
menu is not placed relative to it. The mod adjusts the anchor point instead,
which is why it hooks that function. Moving the menu's content, its window or its
popup directly does not work: the content is its own visual tree, and the window
it is drawn into does not correspond to the menu's visible area.

## License

GPL-3.0. The taskbar XAML access is based on the
[Start button always on the left](https://github.com/m417z/my-windhawk-mods) mod
by m417z, which is also licensed under GPL-3.0.

## Feedback

Bug reports and feature requests are welcome in
[Issues](https://github.com/loliri/windhawk-taskbar-margin/issues).
