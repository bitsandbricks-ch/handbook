# UI migration prompt: move an app onto the shared packages

```
Move this app onto the shared BitsAndBricks UI packages from bitsandbricks-ch/ui,
following blocks/ui-and-brand.md in https://github.com/bitsandbricks-ch/handbook.

- React and Mantine apps: use @bitsandbricks/tokens, @bitsandbricks/ui and, for dashboards,
  @bitsandbricks/widgets. Vue, Tailwind and other stacks: tokens only.
1. List this app's components that the packages already provide, those that differ, and
   those that stay app-specific. Report before changing anything.
2. After my go: set the theme with the app's own accent and logo, then replace components
   one area per draft PR (layout, tables and filters, feedback, panels, widgets), with
   before and after screenshots in light and dark.
3. A component the package lacks or gets wrong: propose a PR to bitsandbricks-ch/ui, don't
   fork it here.

Tests and lint pass at every step. Never merge, never push tags, never change versions.
```
