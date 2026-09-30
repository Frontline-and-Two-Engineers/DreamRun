# TODO List

This project isn't valuable enough to warrant using Jira or similar task-tracking services.
The basic tasks that need to be addressed in the future are outlined here.

## 2026-09-30

1. Change the `dr-hide-active` class to `hide` for dynamically created `image` tag objects.
2. Fix save-game screenshots.
3. Remove the backslash used for single-line tags.
4. Make script error debugging more explicit.
5. Implement a fallback for the `[image hide]` tag modification in case the developer hasn't defined the `hide` (or `dr-hide-active`) CSS class for the object.
6. Preload tag objects before displaying the rest of the game content.
7. Send game script variable packets to the client only if the variable has been used or modified within the script.
8. Revise the CSS usage system for `[image]` tag images.
9. Add `[image show]` modification animation class to the CSS objects of the `[image]` tag (like with `[image hide]` animation).
10. Fix the parsing of CSS object paths containing variables when processing game saves.