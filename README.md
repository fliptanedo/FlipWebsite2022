# Tanedo Group Website 2022

Hosted at  [particle.ucr.edu](https://particle.ucr.edu)

Flip Tanedo
October 2022

See `AcademicTheme_Readme.md` for the Hugo Academic Resumé template information. I periodically re-do my personal website from scratch using the latest Academic template. Most of the material in this document copied from earlier websites. This `README.md` file is a personal reminder of how I edited the page. 

Old versions: [2021](https://github.com/fliptanedo/tanedo-website-2021/blob/master/README.md) | [2020](https://github.com/fliptanedo/flip-www-2020).

## Oct 9, where I stopped:

Tried to copy `layouts` from the 2021 site and it didn't work. On the one hand, I believe that the template lookup order is working properly now (unlike in 2021). On the other hand, I think I have to start from scratch with this shit. 

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

## Notes from last time 

* I backed up some old sites in `./static/archived/`. These show up under `./archived/` when uploaded. Should I link to them? These archived sites are a huge pain. They take up a large amount of space. I think it's from the saved pdfs of talks. I think there should be a better way of archiving these in the future.
* There are a bunch of image files that are much larger than they need to be. The media folder is around 50 mb. I should make small versions of the images.

## Useful References

* The [Hugo Directory Structure](https://gohugo.io/getting-started/directory-structure/) 

  * All paths are relative to the project root

* [Extending Wowchemy](https://wowchemy.com/docs/hugo-tutorials/extending-wowchemy/)

  ```
  To override a template in the theme, you simply copy the file you are interested in from the version of the Wowchemy module your site uses and paste it in your site folder using a similar path. To choose your version, select its tag from GitHub’s dropdown master box on the upper left and press enter.
  ```

  


## Transferring assets

All paths in this document are relative to the project root, `FlipWebsite2022/` 

### Background 

All paths are supposed to be relative to the project root. We want to create a `./layouts/` folder and so that Hugo will look here for templates and shortcodes first. This over-rides any files from the theme. Last time, this did not quite work, see [Modifying Modules](https://gohugo.io/hugo-modules/use-modules/#make-and-test-changes-in-a-module). It seems, though, that this is now fixed. 

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

Fixing the themes.

[Wowchemy themes](https://github.com/wowchemy/wowchemy-hugo-themes/tree/main/modules/wowchemy/data/themes)



[Theme customization](https://wowchemy.com/docs/getting-started/customization/#custom-theme)



### TO DO NEXT

Transfer over bits of the layouts. But first I should transfer the assets. 

#### Widget Content

Transfer `./content/home` . This *will* break the page because you're calling . You can use the line `active = false` in the markdown files to turn widgets off for now.

### 
