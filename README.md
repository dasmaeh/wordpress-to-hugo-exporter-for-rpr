# RecipePress reloaded to Hugo Exporter

Hugo a static site generator written in GoLang: [https://gohugo.io](https://gohugo.io)

This repo is based on [https://github.com/SchumacherFM/wordpress-to-hugo-exporter](https://github.com/benbalter/wordpress-to-jekyll-exporter)

# Why I created this tool
Some years ago I've taken over the development of RecipePress, a recipe plugin for wordpress. I've released this as RecipePress reloaded ([GitHub](https://github.com/dasmaeh/recipepress-reloaded), [wordpress.org](http://wordpress.org/plugins/recipepress-reloaded/) ).
I had primarily done this to run my own blog. But programming the plugin has taken more and more time as well as maintaining my wordpress instances. With a load of work on the horizon due to Gutenberg I've decided to stop working on the plugin.
For me personally I also decided to move my blogs to Hugo so I needed a tool to migrate my recipes. Here it is. It works for me and hopefully also is useful for you.

# What this tool does
* Export all recipes to single markdown files that can be used in hugo
* Add all meta data and taxonomies to the [https://gohugo.io/content-management/front-matter/](front matter) of the recipe file
* Include ingredients to the taxonomies in case you want to create a ingredient based index of your recipes
* Export a recipe's description, ingredients, instructions and notes into the content part
* Export all your media as a `wp_content` directory

# What this tool does not do
* Install hugo for you
* Create a recipe [https://gohugo.io/content-management/types/](type) in hugo
* Create a layout file for recipes in hugo
* Organize your recipes
* Export links in your recipe list other than custom links
* Export regular posts and pages from wordpress. Please use the [https://github.com/SchumacherFM/wordpress-to-hugo-exporter](Wordpress to Hugo Exporter) for this task.

# Design decisions
When typesetting recipes there are always two possibilities, either creating very much structured recipes or allow for a more casual way of writing things down.
To a degree RecipePress reloaded has supported both models of storing recipes. For this exporter I've decided to use the more casual aproach. This has some consequences:
* You can easily write down recipes in markdown almost as any kind of post. They just have a few more meta data
* It's easy to adjust the structure to the specific recipe
  * add groups for ingredients and instructions
  * add pictured wherever you like
  * add comments and notes whereever you like
* However, creation of microdata is hardly possible this way
* The recipe collection is more a collection of notes than a recipe database

If you're not happy with these decisions, feel free to change this exporter's code. You can easily export ingredients, instructions, notes and so on as meta data dields in the front matter and use a layout file in hugo to format the output. That way it should also be possible to use structured data.

# Important
This plugin is using RecipePress reloaded's language files and libraries. RecipePress reloaded needs to be installed and active!

# Usage with a self hosted WordPress installation

1. Place plugin in `/wp-content/plugins/` folder
2. Make sure `extension=zip.so` line is uncommented in your `php.ini`
3. Activate plugin in WordPress dashboard
4. Select `Export to Hugo` from the `Tools` menu

# Usage for wordpress.com or any other hoster without SSH access

(Only tried during development with sample data)

1. Login into the backend.
2. Create an XML export of the whole blog and download the XML file.
3. Setup a local WordPress instance on your machine. You need PHP, MySQL or
MariaDB and Nginx or Apache or Caddy Server.
Easiest will be to use docker [https://github.com/wodby/docker4wordpress](https://github.com/wodby/docker4wordpress) or vagrant [https://github.com/varying-vagrant-vagrants/vvv](https://github.com/varying-vagrant-vagrants/vvv)
4. Install this plugin.
5. Import the XML export. You should take care that the WordPress version of the
export matches the WP version used for the import.
6. In the WP backend run the `Export RPR to Hugo` command. If that fails go to the
command line run the CLI script with `memory_limit=-1`, means unlimited memory
usage.
7. Collect the ZIP via download or the CLI script presents you the current name.
8. Remove WordPress and enjoy Hugo.

# Command-line Usage

If you're having trouble with your web server timing out before the export is
complete, or if you just like terminal better, you may enjoy the command-line
tool.

It works just like the plugin, but produces the zipfile at `/tmp/wp-hugo.zip`:

    php hugo-export-cli.php

Alternatively, if you have [WP-CLI](http://wp-cli.org) installed, you can run:

```
wp hugo-export > export.zip
```

The WP-CLI version will provide greater compatibility for alternate WordPress
environments, such as when `wp-content` isn't in the usual location.

# Changelog

## 0.1
* Initial Release

# License

The project is licensed under the GPLv3 or later
