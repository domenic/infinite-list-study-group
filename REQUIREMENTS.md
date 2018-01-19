# Infinite List Requirements
This document gathers the various use cases and features we've identified for infinite list along with their priority. If you disagree with these priorities,
or would like to add additional use cases/features, please file an issue or submit a PR.

# Use Cases
- News feeds
- Product listings
- Incremental rendering (i.e. each paragraph on your page is an element. Items are rendered as the user scrolls)

We plan to address all use cases with the MVP, though news feeds and product listings are higher priority.

# Features
__P0__ means that the MVP will not ship without the feature. __P1__ means that we plan to include the feature in the MVP, but can drop
it if it proves impossible, overly complex, or that it might delay shipping timeline. __P2__ will not be included in the MVP and may
never be included.

## P0
- Accurate scrollbar position for elements that have been loaded
- Discoverable by screen readers for elements that have been loaded
- Heterogeneous elements
- Fallback element UI
- Grid layout
- List headers/footers
- Configurable layout and style for elements in the list
- Clickable list elements

## P1
- Find in page (ctl+F) highlights elements that have been loaded
- Sub groups within the list (i.e. letter groupings for a contact list)
- Maintain scroll position across page load
- "Load more" button

## P2
- Backed by remote data source
- Selectable list elements
- Rearrangeable list
- Find in page (ctl+F) highlights all elements in the list that haven't been loaded
- Accurate scrollbar position including elements that haven't been loaded
- Pull to refresh
- Discoverable by screen readers for elements in the list that haven't been loaded
- Horizontal scrolling
