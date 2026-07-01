---
layout: article
objectid: grp-be-001
title: "Test Article: 1st Brussels Group"
type: Group
permalink: /articles/test-grp-be-001.html
---

## About This Group

This is a **test article page** for Phase 1 verification. The metadata sidebar on the right should automatically populate with data from the Google Sheet for the item with `objectid: grp-be-001`.

If you see a loading spinner that never resolves, check:
1. That `grp-be-001` exists in your Google Sheet
2. That the Sheet is accessible (the metadata CSV URL in `_config.yml` is correct)
3. That your browser has network access to fetch the CSV

## Testing Checklist

- [ ] Page loads with two-column layout
- [ ] Loading spinner appears initially in sidebar
- [ ] Spinner is replaced by metadata card
- [ ] Metadata fields match what's in the Sheet for `grp-be-001`
- [ ] "View Full Metadata Record" button links to `/item.html?id=grp-be-001`
- [ ] Breadcrumb shows: Home → Groups → "Test Article: 1st Brussels Group"

## Sample Content

### History

This section demonstrates how markdown content will be rendered in the article body.

### Features

- **Bold text** and *italic text* work
- Lists work:
  - Item one
  - Item two
  - Item three

### Code Example

```
This is a code block to test the pre formatting.
```

> This is a blockquote to test styling.

## Next Steps

Once this test article works correctly, we'll proceed to Phase 2: updating `browse-js.html` so that browse cards link to article pages when `article_url` is present in the Sheet.
