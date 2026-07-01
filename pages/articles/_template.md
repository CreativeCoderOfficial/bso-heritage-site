---
layout: article
objectid: REPLACE-WITH-OBJECTID
title: REPLACE-WITH-TITLE
type: Group
permalink: /articles/REPLACE-WITH-OBJECTID.html
---

<!--
  INSTRUCTIONS FOR VOLUNTEERS
  ============================
  - objectid: copy this EXACTLY from the Google Sheet — it is case-sensitive
  - title: the display name of the item (group name, badge name, etc.)
  - type: must be one of: Group | Badge | Document | Far & Wide
    (must match the Sheet value exactly, including capitalization)
  - permalink: replace REPLACE-WITH-OBJECTID with the same value as the objectid field above

  After saving this file, update the article_url column in the Google Sheet
  row for this item to:  /articles/REPLACE-WITH-OBJECTID.html

  COMMIT THE FILE BEFORE updating the Sheet row, or the browse card
  will link to a page that does not yet exist (404).

  Markdown guide: https://guides.github.com/features/mastering-markdown/
  
  Images: Use Liquid syntax for proper path handling with baseurl:
    ![alt text]({{ '/objects/filename.jpg' | relative_url }})
  Replace 'filename.jpg' with your image filename. The image must be uploaded to the
  /objects/ folder in the repository.
-->

## About This [Group / Badge / Document]

Write a brief introduction here. One or two paragraphs explaining what this item is and why it matters.

### Example Image

To add an image from the /objects/ folder, use:
```markdown
![Image description]({{ '/objects/your-image.jpg' | relative_url }})
```

## History

Describe the history in chronological order. You can use subheadings, bullet lists, and bold text freely.

### Early Years

...

### Key Milestones

...

## Notable Members / Features

...

## Sources and Further Reading

- Source one
- Source two
