# Wraith 

[![Build Status](https://secure.travis-ci.org/BBC-News/wraith.png?branch=master)](http://travis-ci.org/BBC-News/wraith)
[![Code Climate](https://codeclimate.com/github/BBC-News/wraith.png)](https://codeclimate.com/github/BBC-News/wraith)

 * Website: http://responsivenews.co.uk
 * Source: http://github.com/bbc-news/wraith

Wraith is a screenshot comparison tool, created by developers at BBC News.


## What is it?

Wraith uses either [PhantomJS](http://phantomjs.org) or
[SlimerJS](http://slimerjs.org) to create screen-shots of different environments
and then creates a diff of the two images, the affected areas are highlighted in
blue

![Photo of BBC News with a
diff](http://bbc-news.github.io/wraith/images/320_diff.png)


## Requirements

You'll need either PhantomJS or SlimerJS, ImageMagick & Ruby 1.9.3 or greater.
It's up to you to decide which browser engine you want to run it against.

### OSX
On Mac OS X, the best way to install dependencies is if you have [homebrew](http://brew.sh).  It is the quickest way to install PhantomJS & ImageMagick.

    brew install imagemagick
    brew install phantomjs or brew install slimerjs
    
### Windows
On Windows it's important to install ImageMagick from the binary found [here](http://www.imagemagick.org/script/binary-releases.php#windows). 
This will ensure that ImageMagick is listed first in your PATH.

To check your path after installing, open a command line window and type:

    path

This will return a list of executables in you PATH separated by semi-colons ;

    C:\Program Files\ImageMagick-6.8.7-Q16;C:\Windows\system32;C:\Windows...

If ImageMagick is in your path but not listed first you can edit your path by following the instructions [here](http://geekswithblogs.net/renso/archive/2009/10/21/how-to-set-the-windows-path-in-windows-7.aspx)

If ImageMagick is not in your PATH at all but you have it installed you will need to add it in. Follow the instructions from the link above and add in the path to where you have ImageMagick installed. For example C:\Program Files\ImageMagick-6.8.7-Q16; 
Make sure the version number and location are correct for your system.

### Ubuntu
On Ubuntu 12.04, you will need to apt-get the following packages:

* libicu-dev
* imagemagick
* rake

And then download the PhantomJS binary package from
[http://phantomjs.org/](http://phantomjs.org/).

## Installation

Open terminal and run

    gem install wraith

You can then run the following to create a template snap.js and config file:
    
    wraith setup

Alternatively you can clone the repo.    
    
    git clone https://github.com/BBC-News/wraith
    cd wraith
    bundle install    

## Config

All config options will be placed in config.yaml. You can specify the snap file name, this could be used in a situation where you have a large amount of config files.  You can set the headless browser, domains of the 2 sites you are comparing, URL paths, screen widths & HTTP headers.  If you don't want to set paths, you can use the spidering option to check your entire website  Remove/comment out the `path:` entries in `config.yaml`.

```yaml

# Headless browser option
browser:
  webkit: "phantomjs"
  # gecko: "slimerjs"

# If you want to have multiple snapping files, set the file name here
snap_file: "path/to/snap.js"

# Type the name of the directory that shots will be stored in
directory:
  - 'shots'

# Add only 2 domains, key will act as a label
domains:
  english: "http://www.live.bbc.co.uk/news"
  russian: "http://www.live.bbc.co.uk/russian"

#Type screen widths below, here are a couple of examples
screen_widths:
  - 320
  - 600
  - 768
  - 1024
  - 1280

#Type page URL paths below, here are a couple of examples
paths:
  home: /
  uk_index: /uk

# If you don't want to name the paths explicitly you can use a yaml
# collection as follows, and names will be derived by replacing / with _
#
# paths:
#  - /imghp
#  - /maps

#Amount of fuzz ImageMagick will use 
fuzz: '20%' 

#Set the number of days to keep the site spider file
spider_days:
  - 10

```

You can also specify PhantomJS command line options in config.yaml.

For example to tell PhantomJS to ignore SSL errors:

```yaml
phantomjs_options: "--ignore-ssl-errors=true"
```

## Using Wraith
### Wraith Gem
You can type `wraith` into terminal to bring up the list of commands, but the one to start Wraith is 

```sh
wraith capture config_name
```

This assumes that your snap.js and config.yaml are in the folders that were created on setup.  There are other commands also available, these all expect a config_name to be passed as an option.  

```sh
  wraith capture config_name  # A full Wraith job
  wraith compare_images       # compares images to generate diffs
  wraith crop_images          # crops images to the same height
  wraith folders              # create folders for images
  wraith generate_gallery     # create page for viewing images
  wraith generate_thumbnails  # create thumbnails for gallery
  wraith reset_shots          # removes all the files in the shots folder
  wraith save_images          # captures screenshots
  wraith setup                # creates config folder and default config
```

### Wraith Rake tasks

If you want to use the rake task instead of the gem, you can use the following.

There are two ways of using Wraith, the fastest is to simply type rake.

```sh
rake
```

You may want to deal with multiple config files so you can compare different pages, different sites or different viewports.  In order to handle this, you can pass in a different config file by typing the following into terminal.  The default is config.yaml, so even if you don't pass a file in, it will still run.  If you have deleted config.yaml, make sure you are inside the `configs` directory before running the command.

```sh
rake config[config_name]
```

On Windows before running the rake command you will need to make a small edit to the wraith.rb file.
Locate lines 60 and 70 and switch the commenting as described.

If you are new to ruby, rake and PhantomJS, [here is a great screencast](http://www.youtube.com/watch?v=gE_19L0l2q0) about how to use Wraith by [Kevin Lamping](https://twitter.com/klamping)

## Output

After each screenshot is captured, the compare task will run, this will output a diff.png and a data.txt.  The data.txt for each file will show the number of pixels that have changed.  There is a main data.txt which is in the root of the output folder that will combine all of these values to easier view all the pixel changes.

## Gallery

A gallery is available to view each of the images and the respective diff images located in the shots folder once all the images have been compared.

## Grabber

If you just want to see a single domains images and not compare them, perhaps for archiving, remove a domain and run the new rake task.   For default config, use grab, or specify a config using grabber. 

`rake grab` and `rake grabber[config]`

## Contributing

If you want to add functionality to this project, pull requests are welcome.

 * Create a branch based off master and do all of your changes with in it.
 * If you have to pause to add a 'and' anywhere in the title, it should be two pull requests.
 * Make commits of logical units and describe them properly
 * Check for unnecessary whitespace with git diff --check before committing.
 * If possible, submit tests to your patch / new feature so it can be tested easily.
 * Assure nothing is broken by running all the test
 * Please ensure that it complies with coding standards.

**Please raise any issues with this project as a GitHub issue.**

## Changelog - updated 2014-02-09
We have released Wraith as a Ruby Gem!!  There is a new CLI to better interact with Wraith and it's commands.

## License

Wraith is available to everyone under the terms of the Apache 2.0 open source license. 
Take a look at the LICENSE file in the code.

## Credits

 * [Dave Blooman](http://twitter.com/dblooman)
 * [John Cleveley](http://twitter.com/jcleveley)
 * [Simon Thulbourn](http://twitter.com/sthulbourn)

