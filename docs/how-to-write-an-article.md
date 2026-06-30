# How to Write an Article for the BSO Heritage Website

This guide explains how volunteers can create rich markdown articles for collection items (Groups, Badges, Documents, and Far & Wide issues).

---

## When to Write an Article

**Not every item needs an article.** Articles are best suited for items with a story worth telling beyond what the metadata fields can capture.

**Good candidates for articles:**
- Groups with rich histories, notable members, or significant achievements
- Badges with interesting design stories or special significance
- Documents that provide unique historical insight
- Far & Wide issues covering important events or themes

**If the item's story is fully captured by its metadata fields (title, description, dates, location, etc.), it probably doesn't need an article.**

---

## Step-by-Step: Create the Article File

### 1. Navigate to the articles folder

In the GitHub repository:
- Go to **`pages/articles/`**
- Click **"Add file"** → **"Create new file"**

### 2. Name your file

Name the file using the item's `objectid` from the Google Sheet, with the `.md` extension:

```
pages/articles/grp-be-001.md
```

**Important:** The filename doesn't affect the URL — the URL is controlled by the `permalink` field in the front matter (see below). But using the `objectid` as the filename helps keep things organized.

### 3. Copy the template

Open `_template.md` in the `pages/articles/` folder. Copy its entire contents and paste into your new file.

### 4. Fill in the front matter

At the top of your file, you'll see:

```yaml
---
layout: article
objectid: REPLACE-WITH-OBJECTID
title: REPLACE-WITH-TITLE
type: Group
permalink: /articles/REPLACE-WITH-OBJECTID.html
---
```

Replace the placeholder values:

| Field | What to enter | Example |
|-------|---------------|---------|
| `objectid` | Copy **exactly** from the Google Sheet. This is case-sensitive. | `grp-be-001` |
| `title` | The display name of the item | `1st Brussels Group` |
| `type` | Must match the Sheet's `type` column **exactly**, including capitalization. One of: `Group`, `Badge`, `Document`, `Far & Wide` | `Group` |
| `permalink` | The URL where the article will live. Replace `REPLACE-WITH-OBJECTID` with the same `objectid` value. | `/articles/grp-be-001.html` |

### 5. Write your article content

Below the front matter (after the second `---`), write your article in markdown.

**Markdown quick reference:**
- Headings: `# Heading 1`, `## Heading 2`, `### Heading 3`
- Bold: `**text**` or `__text__`
- Italic: `*text*` or `_text_`
- Lists: `- item` or `1. item`
- Links: `[link text](https://url.com)`
- Images: `![alt text](/objects/filename.jpg)`

**For images:** Use the path `/objects/filename.jpg` if the image is already uploaded to the `objects/` folder in the repository. The image must exist in the repo before you reference it.

For a full markdown guide, see: [GitHub Markdown Guide](https://guides.github.com/features/mastering-markdown/)

---

## Important: The Commit-First Rule

**CRITICAL:** You must **commit your markdown file to GitHub BEFORE** updating the Google Sheet.

### Why this matters

When a browse card is clicked, it looks for the `article_url` value in the Sheet and tries to go to that page. If you update the Sheet before committing the file, visitors will click the card and get a **404 error** (page not found).

### Correct order:

1. ✅ Create the `.md` file in `pages/articles/`
2. ✅ Fill in the content and front matter
3. ✅ **Commit the file to GitHub**
4. ✅ **Then** open the Google Sheet and add the `article_url`

---

## Update the Google Sheet

After committing your file:

1. Open the **BSO Heritage Google Sheet**
2. Find the row for your item (match by `objectid`)
3. In the `article_url` column, enter the exact permalink from your file's front matter:
   
   ```
   /articles/grp-be-001.html
   ```

4. **Publish the Sheet** (File → Publish to web → CSV → Republish)

---

## Wait for Site Rebuild

The site rebuilds automatically via GitHub Actions at **02:00 UTC** (approximately 3 AM BST / 4 AM CEST).

After this time:
- Browse cards for your item will link to your new article page
- The article page will show your markdown content with a live metadata sidebar

**To test immediately:** You can run `bundle exec jekyll serve` locally to preview your article before the live site updates.

---

## Article Page Features

Your article page includes:

- **Breadcrumb navigation** back to the appropriate category (Groups, Badges, Documents, Far & Wide)
- **Two-column layout** with article content on the left and metadata on the right
- **Live metadata sidebar** that automatically displays all fields from the Google Sheet for that item
- **"View Full Metadata Record" button** that links to the standard item page (so visitors can see all technical details)
- **Responsive design** that works on mobile and desktop

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Article page shows "Metadata not found" | Check that `objectid` in your file matches the Sheet exactly (case-sensitive) |
| Browse card links to 404 | You updated the Sheet before committing the file. Commit the file first, then update the Sheet. |
| Browse card still links to old item page | The site hasn't rebuilt yet. Wait for the 02:00 UTC rebuild, or test locally with `jekyll serve`. |
| Metadata sidebar shows wrong data | The `objectid` in your file doesn't match any row in the Sheet. Verify the spelling. |
| Images not displaying | Make sure the image exists in `/objects/` and the path in your markdown is correct. |

---

## Need Help?

If you run into issues, check:
1. The `objectid` matches **exactly** (including uppercase/lowercase)
2. The `type` field matches the Sheet's `type` column exactly
3. You committed the file **before** updating the Sheet
4. The Google Sheet is published to CSV

Ask in the team channel if you're still stuck!
