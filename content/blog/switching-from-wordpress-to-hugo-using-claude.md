---
title: Switching from WordPress to Hugo Using Claude
date: 2026-10-22T06:38:03
draft: true
slug: switching-from-wordpress-to-hugo-using-claude
categories:
  - Uncategorized
  - General
tags:
  - hugo
  - wordpress
  - claude
---
I was recently thinking about switching my personal site from Wordpress to a static site, like Hugo. In the migration, I would move from my old host to using GitHub pages. The goal was to reduce the overall footprint for a low traffic site and remove all the overhead that Wordpress brought. Don't get me wrong, Wordpress has been good, but I like the idea of just static pages. 

I decided to go with Hugo as my static generator and this would be my first real project trying out using Claude to do the majority of the legwork. 

This post walks through what I did, in order, and the problems I hit along the way and how they were solved. If you are thinking about the same move, hopefully it saves you some time.

Disclaimer: I used Claude to create the majority of this post based on our interactions and have reviewed all output. 

## The Goal and the Ground Rules

I gave Claude a short list of requirements:

- Recreate the site as a Hugo site using the [Anatole](https://github.com/lxndrblz/anatole) theme.
- Keep the **same menu and the same pages**, without changing the content.
- Build the site with GitHub Actions and host it on GitHub Pages.

Original Prompt:
> I want to create a hugo version of the wordpress site at https://www.jamesgaryjardine.com. I want to use the Hugo theme found at https://github.com/lxndrblz/anatole. The new hugo site should keep the same menu and pages without changing any of the content. The goal will be to deploy this new hugo site in github pages using git actions to build it.

## Step 1: Plan Before Touching Anything

Claude started in a planning mode, where it can read and research but not change anything. Before writing a line of config it inventoried the existing site:

- The site title, tagline and the navigation menu (Home, Blog, Portfolio with three sub-pages, and Resume).
- The page list, from the WordPress sitemap: 7 pages and 23 blog posts.
- The sidebar items (search, recent posts, archives and the Follow Me links).

Two details from that research mattered later. The post URLs lived at the root of the site (`/some-post-title/`, not `/blog/some-post-title/`), and the Portfolio sub-pages lived under `/portfolio/`. If I wanted old links to keep working, the Hugo site needed to produce exactly the same URLs.

It then asked me a few questions that I could not leave to a default: should all the posts be migrated (yes), what would the site address be (my custom domain), and how should the theme be installed (as a git submodule). Having those answered up front kept the rest of the work moving.

## Step 2: Get the Content Out of WordPress

I expected to export the content from the WordPress admin screen. Claude took a different route: the WordPress REST API. Every WordPress site exposes its pages and posts as JSON at `/wp-json/wp/v2/pages` and `/wp-json/wp/v2/posts`, so one request each pulled down everything, including titles, dates, slugs, categories, tags and the rendered HTML.

A small script turned each item into a Hugo content file with front matter (title, date, slug, categories and tags). It also downloaded the images and the resume PDF into Hugo's `static/` folder, using the same paths as WordPress (`/wp-content/uploads/...`), so nothing depended on the old host any more.

## Step 3: Configure Hugo and the Theme

The theme went in as a git submodule (`themes/anatole`), and the configuration went into `config/_default/`:

- **`hugo.toml`** has the base URL, the site title and the permalink rule that keeps posts at `/<slug>/`.
- **`menus.toml`** recreates the menu, including the Portfolio drop-down. Anatole supports sub-menus through a `parent` setting on each entry.
- **`params.toml`** has the tagline, the sidebar profile picture and the social links.

A few structure choices came out of how Anatole works:

- Posts live in a `blog` section, so the Blog menu item shows a list of posts grouped by year. A permalink rule makes each post's URL just `/<slug>/`, exactly as it was on WordPress.
- The Portfolio pages are plain pages with a fixed URL set in their front matter. The theme has its own "portfolio" layout, and having a real `portfolio/` folder would have triggered it and replaced my page content.
- The home page is the About Me page. Anatole normally lists recent posts on the home page, so I set the main sections to an empty list to keep the home page the way it was on WordPress.

Checking the results was straightforward. Claude compared every URL in the WordPress sitemaps against the folders Hugo produced. All 30 pages matched.

## Step 4: The GitHub Actions Workflow

The workflow follows the standard Hugo-on-Pages pattern. On every push to `main` it installs Hugo, checks out the repo (including the theme submodule), builds the site with `hugo --gc --minify`, and uploads the result to GitHub Pages. A `static/CNAME` file carries the custom domain through to the published site.

## The Problems We Ran Into

Everything above sounds smooth. These are the places where it was not.

### Problem 1: Dart Sass

**The error:**

```text
TOCSS-DART: failed to transform "/scss/main.scss" (text/x-scss).
You need to install Dart Sass
```

The Anatole theme compiles its stylesheets with Dart Sass, which is a separate tool from Hugo. Hugo itself was installed and was the "extended" version, but that was not enough. Interestingly, a production build had worked, while the development build (which is what `hugo server` runs) failed, so the problem was not obvious at first. Claude reproduced it by building in development mode.

**The fix:** install Dart Sass on my machine. Homebrew's route needed a third-party tap, and I decided to install it myself rather than have it added for me. This was also a reminder that the same dependency is missing on the GitHub runner, so the workflow got an extra step to install it (`sudo snap install dart-sass`) before the build. Without that, the site would have built locally and then failed in the cloud.

### Problem 2: The Tools Were Not on Claude's PATH

Claude kept reporting `command not found` for Homebrew and then Hugo, even after I had installed them. The tools were installed, but the terminal session Claude was using had been started before the installation and did not see the updated PATH. Once the PATH was refreshed (a new session picked it up), `brew` and `hugo` both resolved and we could carry on. A small thing, but worth remembering: when an assistant says a tool is missing right after you install it, check which shell it is using.

### Problem 3: Blog Posts That Did Not Look Like Markdown

For speed, and to guarantee the content did not change, the first conversion left the WordPress HTML as it was inside the `.md` files. Hugo renders that fine, so the site looked right. The files themselves, though, were full of `<p>` tags, `&#8211;` entities and inline styles. They were not pleasant to edit, and I want to write future posts in Markdown.

**The fix:** convert everything to real Markdown with [pandoc](https://pandoc.org/). Since "don't change the content" was still the rule, the conversion was checked rather than trusted. Before converting, Claude captured the visible text of every page as a baseline, then compared it with the output after conversion. A word-by-word check showed no missing words in any page.

The comparison also caught several things that a quick look would have missed:

- **Embedded videos were dropped.** Pandoc discards raw HTML by default, so the two YouTube embeds on the presentations page vanished. The converter now holds embeds aside and puts them back as raw HTML after the conversion.
- **Links lost their "open in a new tab" behavior.** WordPress had `target="_blank"` on external links, and Markdown has no equivalent. A small Hugo link render hook now adds it back for external links.
- **Bare web addresses became links.** Hugo's Markdown engine turns plain URLs and email addresses into links automatically, which the original did not do. Turning off that setting kept the text exactly as written.
- **A broken link in the source.** One entry on the presentations page had an unclosed `<Strong>` tag in the original WordPress HTML. Browsers tolerated it, but pandoc dropped the link, so I restored it by hand.

### Problem 4: Dark Mode Text Stayed Black on Blog Posts

After I turned on dark mode I noticed that every page switched correctly except the blog posts. The text stayed black on a dark background.

The cause was the content, not the theme. About twenty elements across the older posts carried an inline `style="color: #000000"`. An inline style beats the theme's dark mode colors. A quick CSS rule that told post content to inherit its color fixed it without touching the posts.

Later, when the posts were converted to Markdown (Problem 3), the inline styles disappeared on their own, so the CSS workaround was removed again. Doing the proper cleanup made the temporary patch unnecessary.

### Problem 5: Details That Only Showed Up After Deploying

A few small issues only appeared once the site was live on GitHub Pages. They are visible in the commit history:

- **Base URL.** The first workflow passed the base URL to Hugo from the GitHub Pages settings, and the config had the `www` address. After deploying, I changed the site's `baseURL` to the address without `www` and removed the override from the build command, so Hugo uses the value from the config file. If you use a custom domain, decide which version (with or without `www`) is the main one, and keep the config, the `CNAME` file and your DNS settings consistent with that choice.
- **The resume PDF link.** The "click here to download as PDF" link on the resume page took a couple of attempts to get right. The file is served from `/documents/`, so a root-relative link (`/documents/jardinejamesRes.pdf`) is what a custom domain needs. A link that includes the repository name is only right for a `username.github.io/repo` project site.

Both of these come down to the same lesson: paths and base URLs behave differently for a custom domain than for a `username.github.io/repo` address.

## What Did Not Carry Over

I think it is fair to be clear about what changed:

- **The WordPress sidebar widgets.** The search box, recent posts and archives list have no equivalent in the Anatole theme. The blog page lists every post grouped by year, which covers the archive need. I added the RSS, Twitter and LinkedIn links to the sidebar from my old "Follow Me" section.
- **The RSS address.** WordPress served the feed at `/feed`. Hugo publishes it at `/index.xml`, so anyone subscribed to the old address will need the new one.
- **Two hidden embeds.** One post had two invisible WordPress embed frames. They only worked with WordPress's own scripts and were hidden anyway, so I dropped them. The visible links beside them are still in the post.

## Working with Claude on This

This was my first real project where I let Claude do most of the work, so here are a few honest observations.

**What worked well**

- **Planning first.** Having it research the site and propose a plan before changing anything meant the main decisions were made early, and the work after that had few surprises.
- **Verification.** The checks (matching every URL, comparing text before and after conversion, rebuilding after each change) caught real problems before I did. It would have been difficult and time consuming to try to compare everything word for word.
- **Finding the real cause.** The Dart Sass problem looked like a Hugo problem, and it took reproducing it in a different build mode to see what was actually missing.

**What needed me**

- **Decisions that were mine.** Installing software on my machine, adding a third-party Homebrew tap, whether to convert the content and which web address to use all needed my input.
- **The things you only see live.** I had to check the real site in a browser, in both light and dark mode, to catch the dark mode text and the link problems. I would still look at every page myself before switching a DNS record.

## Wrapping Up

The site is now plain files in a repository that builds in well under a second and deploys from GitHub on every push. Writing a new post means adding a Markdown file and pushing it.

If you are considering the same move, my short list would be:

1. Pull the content through the WordPress REST API instead of fighting with exports.
2. Keep the URLs identical, and verify them against your sitemap.
3. Install Dart Sass, both locally and in your workflow, if your theme needs it.
4. Convert to real Markdown, and compare the text before and after.
5. Decide on `www` versus the bare domain early, and keep your config, `CNAME` and DNS aligned.
