<!-- Please do not edit this file. Edit the `blah` field in the `package.json` instead. If in doubt, open an issue. -->








[![git-stats](http://i.imgur.com/Q7TQYHx.png)](#)











# `$ git-stats`

 [![Support me on Patreon][badge_patreon]][patreon] [![Buy me a book][badge_amazon]][amazon] [![PayPal][badge_paypal_donate]][paypal-donations] [![Ask me anything](https://img.shields.io/badge/ask%20me-anything-1abc9c.svg)](https://github.com/IonicaBizau/ama) [![Version](https://img.shields.io/npm/v/git-stats.svg)](https://www.npmjs.com/package/git-stats) [![Downloads](https://img.shields.io/npm/dt/git-stats.svg)](https://www.npmjs.com/package/git-stats) [![Get help on Codementor](https://cdn.codementor.io/badges/get_help_github.svg)](https://www.codementor.io/johnnyb?utm_source=github&utm_medium=button&utm_term=johnnyb&utm_campaign=github)

<a href="https://www.buymeacoffee.com/H96WwChMy" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/yellow_img.png" alt="Buy Me A Coffee"></a>







> Local git statistics including GitHub-like contributions calendars.







I'd be curious to see your calendar with all your commits. Ping me on Twitter ([**@IonicaBizau**](https://twitter.com/IonicaBizau)). :smile: Until then, here's my calendar:

![](http://i.imgur.com/PpM0i3v.png)

## Contents


 - [Installation](#cloud-installation)
 - [Usage](#usage)

     - [Importing and deleting commits](#importing-and-deleting-commits)
     - [Importing all the commits from GitHub and BitBucket](#importing-all-the-commits-from-github-and-bitbucket)
     - [What about the GitHub Contributions calendar?](#what-about-the-github-contributions-calendar)

 - [Documentation](#memo-documentation)
 - [How to contribute](#yum-how-to-contribute)













## :cloud: Installation

You can install the package globally and use it as command line tool:


```sh
# Install the package globally
npm i -g git-stats

# Initialize git hooks
# This is for tracking the new commits
curl -s https://raw.githubusercontent.com/IonicaBizau/git-stats/master/scripts/init-git-post-commit | bash
```


Then, run `git-stats --help` and see what the CLI tool can do.


```
$ git-stats --help
Usage: git-stats [options]

Local git statistics including GitHub-like contributions calendars.

Options:
  -r, --raw              Outputs a dump of the raw JSON data.
  -g, --global-activity  Shows global activity calendar in the current
                         repository.
  -d, --data <path>      Sets a custom data store file.
  -l, --light            Enables the light theme.
  -n, --disable-ansi     Forces the tool not to use ANSI styles.
  -A, --author           Filter author related contributions in the current
                         repository.
  -a, --authors          Shows a pie chart with the author related
                         contributions in the current repository.
  -u, --until <date>     Optional end date.
  -s, --since <date>     Optional start date.
  --record <data>        Records a new commit. Don't use this unless you are
                         a mad scientist. If you are a developer just use
                         this option as part of the module.
  -h, --help             Displays this help.
  -v, --version          Displays version information.

Examples:
  $ git-stats # Default behavior (stats in the last year)
  $ git-stats -l # Light mode
  $ git-stats -s '1 January 2012' # All the commits from 1 January 2012 to now
  $ git-stats -s '1 January 2012' -u '31 December 2012' # All the commits from 2012

Your commit history is kept in ~/.git-stats by default. You can create
~/.git-stats-config.js to specify different defaults.

Documentation can be found at https://github.com/IonicaBizau/git-stats.
```







## Usage

### Importing and deleting commits


I know it's not nice to start your git commit calendar from scratch. That's why I created [`git-stats-importer`](https://github.com/IonicaBizau/git-stats-importer)–a tool which imports or deletes the commits from selected repositories.

Check it out here: https://github.com/IonicaBizau/git-stats-importer

The usage is simple:

```sh
# Install the importer tool
$ npm install -g git-stats-importer

# Go to the repository you want to import
$ cd path/to/my-repository

# Import the commits
$ git-stats-importer

# ...or delete them if that's a dummy repository
$ git-stats-importer --delete
```

### Importing all the commits from GitHub and BitBucket


Yes, that's also possible. I [built a tool which downloads and then imports all the commits you have pushed to GitHub and BitBucket](https://github.com/IonicaBizau/repository-downloader)!

```sh
# Download the repository downloader
$ git clone https://github.com/IonicaBizau/repository-downloader.git

# Go to repository downloader
$ cd repository-downloader

# Install the dependencies
$ npm install

# Start downloading and importing
$ ./start
```

### What about the GitHub Contributions calendar?


If you want to visualize the calendars that appear on GitHub profiles, you can do that using [`ghcal`](https://github.com/IonicaBizau/ghcal).

```sh
# Install ghcal
$ npm install -g ghcal

# Check out @alysonla's contributions
$ ghcal -u alysonla
```


For more detailed documentation, check out the repository: https://github.com/IonicaBizau/ghcal.

If want to get even more GitHub stats in your terminal, you may want to try [`github-stats`](https://github.com/IonicaBizau/github-stats)--this is like `git-stats` but with data taken from GitHub.

## Using the configuration file


You can tweak the git-stats behavior using a configuration file in your home directory: `~/.git-stats-config.js`.

This file should export an object, like below (defaults are listed):

```js
module.exports = {
    // "DARK", "LIGHT" or an object interpreted by IonicaBizau/node-git-stats-colors
    "theme": "DARK"

    // The file where the commit hashes will be stored
  , "path": "~/.git-stats"

    // First day of the week
  , first_day: "Sun"

    // This defaults to *one year ago*
    // It can be any parsable date
  , since: undefined

    // This defaults to *now*
    // It can be any parsable date
  , until: undefined

    // Don't show authors by default
    // If true, this will enable the authors pie
  , authors: false

    // No global activity by default
    // If true, this will enable the global activity calendar in the current project
  , global_activity: false
};
```


Since it's a js file, you can `require` any other modules there.

## Saving the data as HTML and images


`git-stats --raw` outputs raw JSON format which can be consumed by other tools to generate results such as HTML files or images.

[`git-stats-html`](https://github.com/IonicaBizau/git-stats-html) interprets the JSON data and generates an HTML file. Example:

```sh

# Install git-stats-html

npm install -g git-stats-html



# Export the data from the last year (generate out.html)

git-stats --raw | git-stats-html -o out.html



# Export data since 2015 (save the results in out.html)

git-stats --since '1 January 2015' --raw | ./bin/git-stats-html -o out.html --big

```



After we have the HTML file, we can generate an image file using [`pageres`](https://github.com/sindresorhus/pageres) by [**@sindresorhus**](https://github.com/sindresorhus/):

```sh

# Install pageres

npm install -g pageres-cli



# Generate the image from HTML

pageres out.html 775x250

```



## Cross-platform compatibility


`git-stats` is working fine in terminal emulators supporting ANSI styles. It should work fine on Linux and OS X.

If you run `git-stats` to display graph on Windows, please use a terminal that can properly display ANSI colors.

Cygwin Terminal is known to work, while Windows Command Prompt and Git Bash do not. Improvements are more than welcome! :dizzy:








## :clipboard: Example



Here is an example how to use this package as library. To install it locally, as library, you can use `npm install git-stats` (or `yarn add git-stats`):



```js
// Dependencies
var GitStats = require("git-stats");

// Create the GitStats instance
var g1 = new GitStats();

// Display the ansi calendar
g1.ansiCalendar({
    theme: "DARK"
}, function (err, data) {
    console.log(err || data);
});
```











## :question: Get Help

There are few ways to get help:



 1. Please [post questions on Stack Overflow](https://stackoverflow.com/questions/ask). You can open issues with questions, as long you add a link to your Stack Overflow question.
 2. For bug reports and feature requests, open issues. :bug:
 3. For direct and quick help, you can [use Codementor](https://www.codementor.io/johnnyb). :rocket:





## :memo: Documentation

For full API reference, see the [DOCUMENTATION.md][docs] file.






## :newspaper: Press Highlights

 - [*A GitHub-like contributions calendar, but locally, with all your git commits*, The Changelog](https://changelog.com/github-like-contributions-calendar-locally-git-commits/)








## :yum: How to contribute
Have an idea? Found a bug? See [how to contribute][contributing].


## :sparkling_heart: Support my projects
I open-source almost everything I can, and I try to reply to everyone needing help using these projects. Obviously,
this takes time. You can integrate and use these projects in your applications *for free*! You can even change the source code and redistribute (even resell it).

However, if you get some profit from this or just want to encourage me to continue creating stuff, there are few ways you can do it:


 - Starring and sharing the projects you like :rocket:
 - [![Buy me a book][badge_amazon]][amazon]—I love books! I will remember you after years if you buy me one. :grin: :book:
 - [![PayPal][badge_paypal]][paypal-donations]—You can make one-time donations via PayPal. I'll probably buy a ~~coffee~~ tea. :tea:
 - [![Support me on Patreon][badge_patreon]][patreon]—Set up a recurring monthly donation and you will get interesting news about what I'm doing (things that I don't share with everyone).
 - **Bitcoin**—You can send me bitcoins at this address (or scanning the code below): `1P9BRsmazNQcuyTxEqveUsnf5CERdq35V6`

    ![](https://i.imgur.com/z6OQI95.png)


Thanks! :heart:
















## :dizzy: Where is this library used?
If you are using this library in one of your projects, add it in this list. :sparkles:

 - `git-stats-importer`
 - `git-stats-fcc-importer`











## :scroll: License

[MIT][license] © [Ionică Bizău][website]






[license]: /LICENSE
[website]: https://ionicabizau.net
[contributing]: /CONTRIBUTING.md
[docs]: /DOCUMENTATION.md
[badge_patreon]: https://ionicabizau.github.io/badges/patreon.svg
[badge_amazon]: https://ionicabizau.github.io/badges/amazon.svg
[badge_paypal]: https://ionicabizau.github.io/badges/paypal.svg
[badge_paypal_donate]: https://ionicabizau.github.io/badges/paypal_donate.svg
[patreon]: https://www.patreon.com/ionicabizau
[amazon]: http://amzn.eu/hRo9sIZ
[paypal-donations]: https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=RVXDDLKKLQRJW
## Git statistics as the measurements that can be extracted from a Git repository. That wouldn't typically include metrics about pull requests (PRs), for example, since they aren't native features of Git but come from repository management services such as GitHub or GitLab
## document about git stats
[Vagdevi]: https://git-quick-stats.sh/
