---
type: Future Consideration
title: Product — Future Considerations
description: Product scope explicitly deferred beyond MVP.
tags: [product, future]
timestamp: 2026-07-02T00:00:00Z
status: reference only
---

⬅ [Product](/product/index.md)

# Future Considerations (Product)

> NOTE: these are not to be worked on now. Included for reference, to ensure the MVP implementation stays extensible.

- Migrate user data from local storage to a backend database
- Progressively enhance the UI to support viewports above mobile
- Allow users to create custom lists
- Introduce a dashboard detailing helpful metrics
- Internationalisation / translation support
- Integrate AI natural language queries against a user's library via a chat interface
- Support offline use, with re-sync when a network connection is available
- Allow a user to see how much storage space their library is using (via the local storage API)
- Validate the OMDb API key live against the API on submit in the [Add API Key Dialog](/design/components/add-api-key-dialog.md), rather than saving it unchecked
- Mask the OMDb API key input (password-style, with a show/hide toggle) in the [Add API Key Dialog](/design/components/add-api-key-dialog.md)
- Surface the date/time a movie was added and its status last changed, somewhere in the UI (moved out of MVP — see [Movie Card](/design/components/movie-card.md)). The underlying data should still be tracked/stored at MVP so this is a UI-only addition later, not a data migration.
- Add a two-step "are you sure?" confirmation before deleting/removing a movie in the [Movie Actions Dialog](/design/components/movie-actions-dialog.md) — MVP performs deletion immediately on tap.
