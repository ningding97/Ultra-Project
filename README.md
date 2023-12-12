
## Setup

```
git clone https://github.com/ningding97/ultrtaproject_website.git
cd ultrtaproject_website
bundle
bundle exec rake bootstrap
bundle exec rake build
```

## License

The code in this repository is released under the MIT license. The contents of
the blog itself (ie: the contents of the `_posts` directory) are released
under +[Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

## Running the OSS Site / Blog locally

Running `rake serve` will _not_ generate category pages. They take a _long_ time
to generate. No one wants that when working on the site.

```
  bundle exec rake serve
```

Categories are generated when the ENV var `PRODUCTION` = `"YES"`.


## Adding an Author

Authors are key-value stored, so you will need to give yourself a key inside
[\_config.yml](_config.yml) - for example:

```yaml
joey:
  name: Joey Aghion
  github: joeyAghion
  twitter: joeyAghion
  site: http://joey.aghion.com
```

Everything but name is optional.

## Authoring an Article

Note: we now have some templates to help get you started writing a blog post.
Check out the [`Post-Templates` directory](Post-Templates).

TLDR _To generate a new post, create a new file in the `_posts` directory. Be
sure to add your name as the author of the post and include several categories
to file the post under. Here is a sample header YAML:_

Note: categories are aggregated from the individual posts, so adding one is as
easy as adding it to your post!

```yaml
---
layout: post
title: "Responsive Layouts with CSS3"
date: 2012-01-17 11:03
comments: true
author: Matt McNierney
github-url: https://www.github.com/mmcnierney14
twitter-url: http://twitter.com/mmcnierney
blog-url: http://mattmcnierney.wordpress.com
categories: [Design, CSS, HTML5]
---
```

More info can be found in the [Jekyll docs](http://jekyllrb.com/docs/posts/).

When you have authored an article, `git add` and `git commit` it, then push to a
named branch with `git push origin [branch]`, and create a pull request to the
`source` branch, it will be deployed to the site by travis when merged.

After you have authored an article, consider re-generating the related articles
data, so that we can surface other articles related to the one you just added.
See **Generating related articles** section below.

## Enabling Comments

Comments for articles are managed with
[Issues](https://github.com/artsy/artsy.github.io/issues) in this GitHub
repository.

#### [Create an issue](https://github.com/artsy/artsy.github.io/issues/new) for the article

Quote the opening paragraph(s) of the post as the body of the issue, and name it
something like "Comments: My Fantastic New Post".

#### Add the `Comment Thread` label to the issue

#### Attach the issue to your article

Copy the created issue ID; add it to the frontmatter YAML of your post, as the
`comment_id` attribute:

`comment_id: 1234`

## After Deploying an Article

Every article on our blog needs one more thing: a snappy tweet! You can ask Ash
or Orta to do this for you, but you're also welcome to log into the
[@ArtsyOpenSource](https://twitter.com/ArtsyOpenSource) twitter account and
tweet yourself (credentials are in the Engineering 1Password vault). Tweets
usually follow the following format:

```
[pithy observation] [description of problem] [@ the article author's twitter handle]

📝 [link to blog post]
💻 [link to GitHub repo, if applicable]
📷 [attach a screenshot of the first few paragraphs of the post]
```

We attach screenshots of the post because tweets with images get more traction.
But! Images aren't accessible to screen readers, so make sure to use the
twitter.com web interface and add a description to the image when posting:

> Screenshot of the title and first two paragraphs of the linked-to blog post.

You can look at previous tweets from our account to get a feel for these. If
you'd like help, just ask in Slack.

## Generating related articles

Generating the content for the "related articles" section at the bottom of an
article is an offline & manual process that makes use of our staging vector
database.

Any developer can run this at any time and commit the resulting changes to
`related-articles.json`.

There are a few simple prerequisite steps required for this task specifically:

1. `gem install foreman`, if you haven't already.

2. `cp .env.example .env`, if you haven't already.

3. Connect to the staging VPN in order to access the staging instance of
   Weaviate, our vector database.

After that it is just:

```sh
foreman run bundle exec rake related_articles
```

## UltraProject

We have made some new features based on Artsy website.

### About selection

`css/screen.scss`

```css
::selection {
  background: #50157d; 
  color: #ffffff; 
}
```

### About affiliation color on Team page

`css/screen.scss`

```css
.affiliation {
  color: #907890; /* 灰紫色的颜色代码 */
}
```

### Navigation & header

`_includes/header.html`

### Footer

`_includes/footer.html`

### open source project layout

`_includes/oss/oss_page_project.html`

### blog index page

`blog/index.html`

### blog archive page

`blog/archives/index.html`

### Frontpage layout

`index.html`

### Team page layout

`team/index.html`

### Team members configuration

`_config.yml`

```yml
student_members:
  - name: XX XX
    github: xxxx
    website: https://xxx.xxx/
    affiliation: XXXXX
```

### Projects on frontpage configuration

`_config.yml`

```yml
oss_projects:
  - title: Enhancing Chat Language Models by Scaling High-quality Instructional Conversations (EMNLP 2023)
    # svg: "_includes/svg/force.svg"
    description:
      We introduce UltraChat, a large-scale dataset designed for training AI assistants, 
      featuring 1.5 million high-quality, diverse multi-turn dialogues without human queries. 
      Leveraging UltraChat, we fine-tuned a LLaMA model to create UltraLM, a powerful conversational model. 

    category: project
    github: https://github.com/thunlp/UltraChat
    arxiv: https://arxiv.org/abs/2305.14233
    created: Apr 3rd, 2023
```

### Github Pages

Modify `.github/workflows/jekyll.yml`.

```yml
on:
  push:
    branches:
      - xubk  # Your branch name, once push to this branch, build process on Github server will be invoked.

```

After build on Github server, automatically deploy build file to `gh-pages` branch.

```yml

  - name: Deploy to GitHub Pages
    uses: peaceiris/actions-gh-pages@v3
    with:
      github_token: ${{ secrets.GITHUB_TOKEN }}
      publish_dir: ./_gh-pages  # Jekyll build output directory
      publish_branch: gh-pages  # branch name to publish
      keep_files: false 

```

In Github pages, set Github Pages publish branch to `gh-pages`.



## About Artsy

<a href="https://www.artsy.net/">
  <img align="left" src="https://avatars2.githubusercontent.com/u/546231?s=200&v=4"/>
</a>

This project is the work of engineers at [Artsy][footer_website], the world's
leading and largest online art marketplace and platform for discovering art. One
of our core [Engineering Principles][footer_principles] is being [Open Source by
Default][footer_open] which means we strive to share as many details of our work
as possible.

You can learn more about this work from [our blog][footer_blog] and by following
[@ArtsyOpenSource][footer_twitter] or explore our public data by checking out
[our API][footer_api]. If you're interested in a career at Artsy, read through
our [job postings][footer_jobs]!

[footer_website]: https://www.artsy.net/
[footer_principles]:
  https://github.com/artsy/README/blob/master/culture/engineering-principles.md
[footer_open]:
  https://github.com/artsy/README/blob/master/culture/engineering-principles.md#open-source-by-default
[footer_blog]: https://artsy.github.io/
[footer_twitter]: https://twitter.com/ArtsyOpenSource
[footer_api]: https://developers.artsy.net/
[footer_jobs]: https://www.artsy.net/jobs