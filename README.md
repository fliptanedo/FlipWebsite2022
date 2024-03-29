# Tanedo Group Website 2022

Hosted at  [particle.ucr.edu](https://particle.ucr.edu)

Flip Tanedo
October 2022

See `AcademicTheme_Readme.md` for the Hugo Academic Resumé template information. I periodically re-do my personal website from scratch using the latest Academic template. Most of the material in this document copied from earlier websites. This `README.md` file is a personal reminder of how I edited the page. 

**NEWER** version: [2023](https://github.com/fliptanedo/FlipWebsite2023 

Old versions: [2021](https://github.com/fliptanedo/tanedo-website-2021/blob/master/README.md) | [2020](https://github.com/fliptanedo/flip-www-2020).

## Useful References

* The [Hugo Directory Structure](https://gohugo.io/getting-started/directory-structure/) 

  * All paths are relative to the project root

* [Extending Wowchemy](https://wowchemy.com/docs/hugo-tutorials/extending-wowchemy/)

  ```
  To override a template in the theme, you simply copy the file you are interested in from the version of the Wowchemy module your site uses and paste it in your site folder using a similar path. To choose your version, select its tag from GitHub’s dropdown master box on the upper left and press enter.
  ```

* [TOML vs YAML formatting for markdown files](https://gohugo.io/content-management/front-matter/)

* [Wowchemy Page Builder](https://wowchemy.com/docs/getting-started/page-builder/)

* [Wowchemy theme customization](https://wowchemy.com/docs/getting-started/customization/#custom-theme)

* [Convert TOML to YAML](https://www.convertsimple.com/convert-toml-to-yaml/)

* [Emoji in Hugo](https://www.webfx.com/tools/emoji-cheat-sheet/)

## Notes for next time

* The default widget template is `demo-links.md`. This is written in TOML, while the other widgets are in YAML. It may be nice to convert them all to YAML. The difference is the format of the [header material](https://gohugo.io/content-management/front-matter/). [Conversion tool](https://www.convertsimple.com/convert-toml-to-yaml/).
* I may want to properly upgrade my custom css to [scss](https://www.geeksforgeeks.org/what-is-the-difference-between-css-and-scss/).
* Teaching section should have an "old classes" part. Currently the number of icons is getting a bit long. Would like a short list of older classes that doesn't take up much room. Could even use a bit of Bootstrap CSS to make it a two column list. 
* The 2021 Readme has a note about making a local copy of wowchemy and making a replacement:
  ```
  replace github.com/wowchemy/wowchemy-hugo-modules/wowchemy => /Users/flip/Documents/Website/wowchemy-hugo-modules-main/wowchemy
  ```

  I have not had to use this hack this time around, but wanted to make a note of it in case I will need to get back to this in the future. I believe it had to do with [Modifying modules.](https://gohugo.io/hugo-modules/use-modules/#make-and-test-changes-in-a-module) 

### From 2021 "Notes for next time"

* I backed up some old sites in `./static/archived/`. These show up under `./archived/` when uploaded. I probably should link to them.
  * These archived sites are a huge pain. They take up a large amount of space. I think it's from the saved pdfs of talks. I think there should be a better way of archiving these in the future.
* ~~There are a bunch of image files that are much larger than they need to be. The media folder is around 50 mb. I should make small versions of the images.~~

## Quick Start from Scratch

I do not use the Netlify-based default deployment.

1. Use the Wowchemy [Academic Resumé template](https://github.com/wowchemy/starter-hugo-academic); there is a green button to `use this template` that will create a repository on your GitHub account.

2. Pull the code locally, e.g. `Code` > `GitHub CLI` 

3. Start hacking the template by copying over bits from the old site. The steps for this are summarized below. I read the previous steps and write what I did this time around. 

4. Make sure you have Hugo e.g. through Homebrew. If it's been a while, you may want to update (up**grade**) hugo using `brew upgrade hugo` at the terminal. 

   `hugo server -D` 

   The output will include a URL: `Web Server is available at //localhost:1313/`, navigate your browser to this URL to view the page.

### If there's an error... 

I end up getting the following error: 

```
Error: failed to download modules: exec: "go": executable file not found in $PATH
```

This means we have to [install some Go dependences](https://wowchemy.com/docs/getting-started/install-hugo-extended/):

```
brew install git golang hugo
```

Then open `~/.zshrc` and add the following line

``` 
export PATH=$PATH:/usr/local/go/bin
```

You have to restart the Terminal app.


## Transferring assets

All paths in this document are relative to the project root, `FlipWebsite2022/` 

### Background 

All paths are supposed to be relative to the project root. We want to create a `./layouts/` folder and so that Hugo will look here for templates and shortcodes first. This over-rides any files from the theme. Last time, this did not quite work, see [Modifying Modules](https://gohugo.io/hugo-modules/use-modules/#make-and-test-changes-in-a-module). It seems, though, that this is now fixed. 

... except for the fact that the [path to the homepage template html files](https://github.com/wowchemy/wowchemy-hugo-themes/tree/main/modules/wowchemy/layouts/partials/blocks) has changed. 

### Default layout templates

I believe the [wowchemy-hugo-themes/modules/wowchemy](https://github.com/wowchemy/wowchemy-hugo-themes/tree/main/modules/wowchemy) repository is the one we want.

* Download the [`wowchemy-hugo-themes`](https://github.com/wowchemy/wowchemy-hugo-themes)
* Copy `[downloadfolder]wowchemy-hugo-themes-main/modules/wowchemy/layouts/` somewhere for the pristine version of the default layouts.  I went ahead and saved it as `./layouts_templates` in my project root folder. That way I can copyu from the `./layouts_templates` folder into my `layouts` folder as I tweak things. 

### Possible issue with `.Path`. 

There's a bigger issue with my 2021 layout files.

```
WARN 2022/10/09 11:50:43 .Path when the page is backed by a file is deprecated and will be removed in a future release. We plan to use Path for a canonical source path and you probably want to check the source is a file. To get the current behaviour, you can use a construct similar to the one below:
  {{ $path := "" }}
  {{ with .File }}
	{{ $path = .Path }}
  {{ else }}
	{{ $path = .Path }}
  {{ end }}
```

Discussed [here](https://discourse.gohugo.io/t/path-when-the-page-is-backed-by-a-file-is-deprecated/36842/6) and [here (wowchemy)](https://github.com/wowchemy/wowchemy-hugo-themes/issues/2592). Looks like the offending piece is my `widget_page.html` hack, since that's the page that contains calls to `.Path`. 

Ugh, ok, this is going to be a much more delicate update

### Pre-Transfer

In order to save the default widgets, I copied the `./content/home` folder into `./content/home_copy` . 

* I guess you do not actually have to do this. You could also deactivate the default widgets using: `active = false`. 

Now try it out. Run `hugo server -D` in the terminal of the project directory and navigate to the locally-hosted draft site. Then delete the `./content/demo_hero.md` file and see that the website has updated by removing the stupid advertisement for the Academic theme.

Go ahead and delete most of the `./content/home ` widgets. I just kept the `about`, `gallery`, and `index` widgets in `./content/home/`. 

### Basic Customization

* To personalize Wowchemy, you can **choose a colour theme and font set** in `config/_default/params.yaml`. [[source](https://wowchemy.com/docs/hugo-tutorials/extending-wowchemy/)]

### Transferring Assets

#### Style Sheets

Adding custom style sheets: [[source](https://wowchemy.com/docs/hugo-tutorials/extending-wowchemy/)]

1. Create the `./assets/scss/` folder if it doesn’t exist
2. Create a file named `custom.scss` in the `./assets/scss/` folder
3. Add your custom CSS code to the file you created and re-run Hugo to view changes

We just copy over the folder from `./assets/` in the 2021 version.

#### Media

1. Transfer over the favicon, `./assets/media/icon.png`. [[source](https://wowchemy.com/docs/getting-started/customization/#website-icon)]
2. Transfer over the `./assets/photos/` folder. This is a good time to make sure none of the files are too big. 
3. Transfer over the `./assets/research/` folder. This is a good time to make sure none of the files are too big. 

#### Fonts and color theme

Copy `./data/fonts/flipfont.toml`

Copy `./data/themes/fliptheme.toml`

#### Static Folder

Transfer the `./static` folder. This one has lots of potentially large files. It is a good time to do housekeeping to remove anything that is no longer being used. 

**Remark:** the archived sites should not be in `./static` . There needs to be a better way to archive them elsewhere. 

#### Shortcodes

Copy:`./layouts/shortcodes/`

These give shotcodes for twitter and for email.

#### Content

Transfer the folders from `./content/post` which contain the pages beyond the front page. I use these "blog posts" for my design portfolio.

### Personal information

Your basic information is stored here:`./content/authors/admin/`

Copy over the `avatar.jpg` image and go through `_index.md` to make sure nothing has changed in the format. This data is used by the main home page widget.

#### Not yet transferred

* `./content/home` contains the homepage widgets. Those depend on layout files that have not yet been transferred due to a recent Hugo update. We need to update those layout files by hand. Hopefully the next time you have to do this no manual intervention will be needed.

### Checkpoint

At this point, you have transferred most of the assets from the previous site. It is worth it to do this by hand so that you do not miss anything new.

## Set up

Go to `./config/_defaut/` and manually transfer the configuration data from the previous site. 

Comment: if the light-mode page looks weird, it's likely because of settings in `./assets/scss/custom.scss`. 

### Comment: light/dark mode toggle 

I'm curious about the light/dark toggle. These show up in `./config/_default/params.yaml`

```
appearance:
  theme_day: minimal
  theme_night: minimal
  font: minimal
  font_size: L
```

```
show_day_night: true
```

Here's how to make [your own theme via Wowchemy](https://wowchemy.com/docs/getting-started/customization/#custom-theme). I currently have a `fliptheme.toml` in there... but there are a lot of edits needed in `./assets/scss/custom.css`. 

It looks like one can use [hugo code in the css file](https://discourse.gohugo.io/t/how-to-use-hugo-template-variables-in-css/4464), though the source is old. (Update: [better discussion here](https://discourse.gohugo.io/t/trying-to-make-theme-colors-configurable-in-css-with-hugo-pipes-but-getting-a-css-syntax-error/26739); looks like the thing to search for is CSS and Hugo Pipes.)

* Some discussion about [lightmode/darkmode images](lightmode/darkmode images), including a nice hack for SVG images (of which I have none).
* Could also have two copies of the css file, one for light/dark. But now this doubles the work of updating the css. What would be better is if we used scss and had some Hugo code at the top that defines the colors. 
* Most likely I should leave this to a future revision. 
* Can probably use `invert()` to deal with images using css? See [this discussion](https://developer.mozilla.org/en-US/docs/Web/CSS/filter-function/invert). 

### Widgets

Let's go through each widget one at a time. We'll fix the layouts and the widgets simultaneously.

General changes since 2022:

* Holy shit, the widget tempalte htmls moved. They are now in `wowchemy-hugo-themes/modules/wowchemy/layouts/partials/blocks/` (link to [GitHub](https://github.com/wowchemy/wowchemy-hugo-themes/tree/main/modules/wowchemy/layouts/partials/blocks)). What this means for us: I'm going to go ahead and copy the above folder into `./layouts/partials/blocks/`. We will edit these copies directly. Information about editing the [Wowchemy blocks](https://wowchemy.com/blocks/).
* When editing the layout files, note that `$page.Content` is now `$block.Content`.
* The blank template layout (previously `./layouts/partials/block.html`) is now `./layouts/partials/blocks/markdown.html`.
  * **However**: tweaking this template is annoying because the `markdown` widget is one of a small number of official widgets that gets to access the two-column layout in `./layouts/partials/functions/parse_block.html`. See discussion below on two-column widgets. I'm going to make a new template.

* The replacement for `blank.md` is `demo-links.md`. That's a handy one to keep in the `./content/home` folder. Remember to toggle the `active` option. Unlike the other template markdown files, `demo-links.md` uses TOML rather than YAML. [Hugo help page on this](https://gohugo.io/content-management/front-matter/).

I can now go through each of the old widgets and adapt any edits I made. Because I learned my lesson in the past, I marked any edits with comments along the lines of `<!-- BEGIN FLIP EDIT -->` and `<!-- END FLIP EDIT -->`.

#### Two column widget: new template

There's something weird going on with havin two column widgets/blocks. Make a copy of `./layouts/partials/blocks/markdown.html` and rename it, say to `test.html` in the same diretory. If I make a new block (`./content/home/test.md`)  and have it call the `markdown` widget, there are no problems: I get a two-column widget with the block title and subtitle printed in the left column. 

However, if I call the `test` widget instead of `markdown` in `test.md`, the left column completely disappears. This appears to be because `markdown.html` knows nothing about the first column. That appears to be in `./layouts/partials/functions/parse_block.html`. ([Source here](https://github.com/wowchemy/wowchemy-hugo-themes/blob/main/modules/wowchemy/layouts/partials/functions/parse_block.html).)

The issue seems to be a  specific line that only allows certain classes of blocks to use columns. This is silly:

```
{{ $use_cols := in (slice "collection" "experience" "accomplishments" "contact" "markdown" "tag_cloud" "portfolio") $block_type }}
```

The `$use_cols` flag is used to print the damn left column. This is stupid. I could override the `parse_block.html` file. However, that's another layer of tweaking the original code that I would rather not get into. 

Instead, I'll make a new template file for sections. It is straightforward to copy the relevant code from `parse_block.html` into a copy of `markdown.html`. 

#### flipportfolio widget

I think the portfolio widget was updated, so I hacked together a `./layouts/partials/blocks/flipportfolio.html` widget using the older style.

#### contact widget

Copy over edits. 

#### Tips

* You can use the line `active = false` in the markdown files to turn widgets off for now.
* Save often and check the demo page (you are running `hugo server -D`, right?) to make sure nothing broke. 
* Sometimes you have to restart the server if Hugo has beocme hopelessly confused.
* Slider page doesn't have a demo. See [Wowchemy demo](https://github.com/wowchemy/wowchemy-hugo-themes/blob/main/starters/research-group/content/tour/slider.md?plain=1) or [info page](https://wowchemy.com/blocks/slider/). Don't forget the hack on the slider.html layout to include the image credit. Make sure the `item.credit` attribute is indented properly.
* The intended default tempalte is **`demo-links.md`** as your default template. I should make my own that uses my revised template that defaults to allowing the two column option.
* Wowchemy has a new **people widget** which is a cute way of doing what I was previously doing with custom code. I can learn a lot from the template code. 

### Layout

I have now transferred the content over. At the moment the page looks like this:
![image-20221113172532974](/Users/flip/Documents/Website/FlipWebsite2022/README.assets/image-20221113172532974.png)

#### Font

[Instructions from Wowchemy](https://wowchemy.com/docs/getting-started/customization/#custom-font).

There is a font file in `./data/fonts/flipfont.toml` with the information. 

In `./config/_default/params.yaml` we activate the font:

```
appearance:
  theme_day: minimal
  theme_night: minimal
  font: 'flipfont'
  font_size: L
```

#### Light/Dark Mode

Turn off the navbar light/dark toggle in `./config/_default/params.yaml`:

```
header:
  navbar:
    enable: true
    align: r
    show_logo: true
    show_language: false
    show_day_night: false
    show_search: false
    highlight_active_link: true
```

Furtherfrom [Wowchemy personalization page](https://wowchemy.com/docs/getting-started/customization/#fonts):

> Otherwise, to force your visitors to see either a light or dark theme, set only a day *or* night theme, but *not* both.

That is: comment out the `theme_night` option in `params.yaml`. This overrides the `show_day_night` toggle and forces the page to only give light mode.

Make sure we've trasnsferred over `./data/themes/fliptheme.toml` which contains color information. I had to revise this due to a change in how Wowchemy does light and dark themes. The easiest way to do this is to start from the new [`minimal.toml` template](https://github.com/wowchemy/wowchemy-hugo-themes/blob/main/modules/wowchemy/data/themes/minimal.toml).

```
# Theme metadata
name = "fliptheme"

# Is theme light or dark?
light = true

# Primary
primary = "#4caf50"
primary_light = "#6ec6ff"
primary_dark = "#0069c0"

# Menu
menu_primary = "#333333"
menu_text = "#fff"
menu_text_active = "#4caf50"
menu_title = "#fff"

# Home sections
home_section_odd = "rgb(255, 255, 255)"
home_section_even = "rgb(247, 247, 247)"
```

And then update the themes in `./config/_default/params.yaml`:

```
appearance:
  theme_day: fliptheme
  theme_night: fliptheme
  font: 'flipfont'
  font_size: L
```



#### Fixing the colors:

Somehow the background color for odd-numbered setions is very dark. I believe this is just the background color that I set somewhere. 

![image-20221114093832357](/Users/flip/Documents/Website/FlipWebsite2022/README.assets/image-20221114093832357.png)

Here's what I wrote in the 2021 readme:

> There are two major design edits here. They're a bit of a pain to implement, but that's my hubris for you. Note: the copy of `./layouts/_default/baseof.html` that we put in here is now outdated. Let's start by placing the most updated version, which we will shortly edit.  
>
> Make sure you have included `/layouts/_default/baseof.html` from the previous site. 
>
> 1. **We want the background of the `<body>` to be dark gray.** Aesthetically we want the background to be white. However, the navigation bar and footer will be dark gray. What this means is that if one over-scrolls (pulls above or below the main page by a little) then you get a bit of white pulling from under the footer or from above the navigation bar. This looks distracting, so we're going to jump through some hoops. This involves:
>
>    a) setting the `<body>` background to be dark gray. If you've copied over the style files, this should already be active.
>
>    b) creating a new `<div id="THECONTENT">` that has a white background. All of the sections will be inside this division. Over scrolling, however, will pull more space from *outside* this division, which will be dark gray.
>
> 2. **We want the fancy footer**. I have a neat Feynman diagram footer graphic that I like.  Observe that the `<div class="container">` that encloses the footer does not spread across the entire navigator width.  The `container` class is something inherited from Bootstrap, the responsive design grid system that *Academic* is built upon.
>
>    We have to place additional `<div>`s just around and above the footer. 
>
> Bonus: at this stage you can also put in the `baseof.html` code for a watermark.
>
> In earlier steps we defined the locations of the graphics in `params.yaml`

The file we'd like to edit is `./layouts/_default/baseof.html`. As I was hacking this together, what was useful for me was to have a clean copy of the Wowchemy template files, see *Default Layout Templates* above. That way I can just transfer files as needed. 

The relevant edits are:

```
  {{ partial "search" . }}

  {{ if .Params.announcement.text -}}
    {{ partial "components/announcement_bar" . }}
  {{ end -}}

<!-- FLIP: FOR WATERMARK -->
<div id="watermark" style="background-image:url('{{ $.Site.BaseURL }}/img/{{ .Site.Params.watermark }}');"></div>
<!-- /FLIP --> 

  <div class="page-header header--fixed">
    {{ partial "navbar" . }}
  </div>

<!-- FLIP: opened a new div for framing -->
<div id="THECONTENT">
<!-- Closed below; see flip2019.css -->
<!-- /FLIP -->

  <div class="page-body">
    {{/* Breadcrumb */}}
    {{/* Don't apply to Book layout as that has different breadcrumb placement. */}}
    {{ if .Params.show_breadcrumb | and (ne .Type "book") }}
      <!-- Use page bg color rather than any color applied to article-container. -->
      {{ $class := cond .IsSection "universal-wrapper" "article-container" }}
      <div class="{{$class}} py-1" style="background: initial">
        {{ partial "components/breadcrumb" . }}
      </div>
    {{ end }}

    {{ block "main" . }}{{ end }}
  </div>

  <div class="page-footer">
    {{/* Docs and Updates layouts include the site footer in a different location. */}}
    {{ if not (in (slice "book" "docs" "updates") .Type) }}
<!-- FLIP -->
    <div style="position: relative; width: 0; height: 0">
      <div id="feynmanfoot" style="background-image:url('{{ $.Site.BaseURL }}/img/{{ .Site.Params.footmark }}');"></div>
    </div>
    <div id="botbar1"></div>
    <!-- -- -->
    <div id="FOOTERBAR"> 
    <!-- closed after container -->
<!-- /FLIP -->
    <div class="container">
      {{ partial "site_footer" . }}
    </div>
<!-- FLIP -->
    </div> <!-- closes div FOOTERBAR -->
<!-- /FLIP -->
    {{ end }}
  </div>

<!-- FLIP -->
</div> <!-- closes id="THECONTENT" -->
<!-- /FLIP -->

  {{ partial "site_js" . }}

</body>
</html>

```

This is a good checkpoint: things are starting to look whole except for some calibration:
![image-20221114173452231](/Users/flip/Documents/Website/FlipWebsite2022/README.assets/image-20221114173452231.png)

Let's go ahead and fix this height. Edit `./assets/scss/custom.scss` and fiddle with the `top` padding.

```
#botbar1{
  /* This is the horizontal line */
	background-color:#C1BBAB;
	margin: 0;
	padding: 0;
  	position:relative;
  	top: 70px;
	left: 0px;
	z-index: 9;
	height: 7px;
	width: 100%;
}
```

You also have to fix `#feynmanfoot` just below in the same file.

#### Navbar

If I haven't yet, now is a good time to update the menu items.

One relatively new problem: There was a weird problem where the **hamburger menu** was giving me white text on a white background for light mode. 

Copying the Bootstrap 4 update note from the 2021 Readme:

> The **breakpoint** of a navigation bar is the window width at which the bar collapses into the familiar "three horizontal bars" symbol for an expanding menu. Bootstrap 4 makes this easy. [Here's how it works](https://stackoverflow.com/a/36289507/4812646):
>
> > Changing the navbar breakpoint is easier in Bootstrap 4 using the navbar-expand-* classes:
>
> ```html
> <nav class="navbar fixed-top navbar-expand-sm">..</nav>
> ```
>
> The options are
>
> - `navbar-expand-sm`: mobile menu on xs screens <576px
> - `navbar-expand-md`: mobile menu on sm screens <768px
> - `navbar-expand-lg`: mobile menu on md screens <992px
> - `navbar-expand-xl`: mobile menu on lg screens <1200px
> - `navbar-expand`: never use mobile menu
> - *(no expand class)* = always use mobile menu
>
> The relevant line shows up at the top of `themes/academic/layouts/partials/navbar.html`. You know what that means: make a copy of that file in `/layouts/partials/` and change 
>
> ```html
> <nav class="navbar navbar-light fixed-top navbar-expand-lg py-0 compensate-for-scrollbar" id="navbar-main">
> ```
>
> to
>
> ```html
> <nav class="navbar navbar-light fixed-top navbar-expand-md py-0 compensate-for-scrollbar" id="navbar-main">
> ```
>
> You can use `navbar-expand-sm` if your menu is particularly brief. 
>
> By the way, this is also *way* easier than it used to be in Bootstrap 3, remember to be grateful.

Copy `./layouts/partials/navbar.html` (e.g. copy from `./layouts_templates/...`)

Fixing the weird dropdown menu styling may need direct hacking of the style file. Here's [the source on GitHub](https://github.com/wowchemy/wowchemy-hugo-themes/blob/main/modules/wowchemy/assets/scss/main.scss).

... I'm not actually sure what fixed this. Restarting Hugo helped. The behavior is still a little different in Safari versus Chrome. I think this is some residual cache thing.

#### Footer

Copy over `./layouts/partials/site_footer.html` from the template. We are commenting out everything in the `footer` tag and replacing it with new stuff.

We insert the following 3-column code:

```
<div class="row" id="footer-columns">
  <div class="col-md-4" id="footer-col-1">
    <img src="{{ $.Site.BaseURL }}img/{{ $.Site.Params.mylogo }}" class="center-me">
  </div>
  <div class="col-md-4" id="footer-col-1">
    <img src="{{ $.Site.BaseURL }}img/{{ $.Site.Params.midlogo }}" class="center-me">
  </div>
  <div class="col-md-4" id="footer-col-1">
    <p class="powered-by">
    {{ with site.Copyright }}{{ replace . "{year}" now.Year | markdownify}} &middot; {{ end }}

    Published with G. Cushen's
    <a href="https://github.com/wowchemy/wowchemy-hugo-modules" target="_blank" rel="noopener">wowchemy</a> for
    <a href="https://gohugo.io" target="_blank" rel="noopener">Hugo</a>.

    {{ if ne .Type "docs" }}
    <span class="float-right" aria-hidden="true">
      <a href="#" id="back_to_top">
        <span class="button_icon">
          <i class="fas fa-chevron-up fa-2x"></i>
        </span>
      </a>
    </span>
    {{ end }}
  </p>
  </div>
</div>
```



# Deployment

The most straightforward deployment is to run `hugo` and then upload the `./public/` folder via SSH or SFTP. 

Wowchemy recommends deploying with Netlify: this syncs your local directory with GitHub, and then deploys the `./public/` folder to a netlify web address. I have found it tricky to figure ot how to sync my university address to the netlify web address. 

My current setup is that my university gives me web space on an AWS server. 

* To explore: I may be able to still [sync to GitHub with AWS](https://aws.amazon.com/blogs/devops/integrating-with-github-actions-ci-cd-pipeline-to-deploy-a-web-app-to-amazon-ec2/). [Tutorial from AWS](https://aws.amazon.com/getting-started/hands-on/host-static-website/).
* [2020 unofficial tutorial](https://samwelek.co.uk/2020/10/05/how-to-host-your-static-website-on-aws-with-github-actions/)

# Troubleshooting

[Wowchemy troubleshooting page](https://wowchemy.com/docs/hugo-tutorials/troubleshooting/).

* `Error: from config: failed to resolve output format "headers" from site config`
  * [Wowchemy docs on this](https://wowchemy.com/docs/hugo-tutorials/troubleshooting/#error-failed-to-resolve-output-format); for me `hugo mod clean --all` ended up solving the problem
  * Discussions: [GitHub](https://github.com/wowchemy/wowchemy-hugo-themes/discussions/2800), [Nov 2021](https://user.it.uu.se/~justin/Hugo/post/hugo_module_fail/), [in Mandarin](https://zenn.dev/meihei/articles/32bb275f71e938), [Hugo issue and CTA](https://github.com/gohugoio/hugo/issues/10208), 
