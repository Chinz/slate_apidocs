Skip to content
Search or jump to…

Pull requests
Issues
Marketplace
Explore
 
@Chinz 
Learn Git and GitHub without any code!
Using the Hello World guide, you’ll start a branch, write comments, and open a pull request.


508
28.3k17.9kslatedocs/slate
 Code Issues 34 Pull requests 7 Wiki Security Insights
Beautiful static documentation for your API https://spectrum.chat/slate
slate
api-documentation
api
static-site-generator
 555 commits
 4 branches
 15 releases
 106 contributors
 View license
@MasterOdin
@lord
MasterOdin and lord Cut version 2.4.0
Latest commit
2019c63
22 days ago
Type	Name	Latest commit message	Commit time
.github	Change default value of base fork in PR template (#1137)	20 days ago
lib	Fix ToC issues, fix #995	last year
source	Fix ToC issues, fix #995	last year
.editorconfig	Update link in .editorconfig to https (#1049)	20 days ago
.gitignore	Ignore .sass-cache	4 years ago
.travis.yml	Update to bundler 2.0 in travis.yml	20 days ago
CHANGELOG.md	Cut version 2.4.0	20 days ago
CODE_OF_CONDUCT.md	Create CODE_OF_CONDUCT.md	2 years ago
Gemfile	[Security] Upgrade nokogiri to 1.10.4 (#1150)	20 days ago
Gemfile.lock	[Security] Upgrade nokogiri to 1.10.4 (#1150)	20 days ago
LICENSE	Update copyright year	6 years ago
README.md	Update url for travis badge	20 days ago
Vagrantfile	Increase memory of Vagrant, fixes #601 hopefully	2 years ago
config.rb	updated config.rb to activate asset_hash (#1076)	20 days ago
deploy.sh	Rename OS X to macOS in deploy.sh comment (#951)	2 years ago
font-selection.json	Remove unused icons from the font	5 years ago
 README.md
Slate: API Documentation Generator
Build Status

Slate helps you create beautiful, intelligent, responsive API documentation.

Screenshot of Example Documentation created with Slate

The example above was created with Slate. Check it out at slatedocs.github.io/slate.

Features
Clean, intuitive design — With Slate, the description of your API is on the left side of your documentation, and all the code examples are on the right side. Inspired by Stripe's and PayPal's API docs. Slate is responsive, so it looks great on tablets, phones, and even in print.

Everything on a single page — Gone are the days when your users had to search through a million pages to find what they wanted. Slate puts the entire documentation on a single page. We haven't sacrificed linkability, though. As you scroll, your browser's hash will update to the nearest header, so linking to a particular point in the documentation is still natural and easy.

Slate is just Markdown — When you write docs with Slate, you're just writing Markdown, which makes it simple to edit and understand. Everything is written in Markdown — even the code samples are just Markdown code blocks.

Write code samples in multiple languages — If your API has bindings in multiple programming languages, you can easily put in tabs to switch between them. In your document, you'll distinguish different languages by specifying the language name at the top of each code block, just like with GitHub Flavored Markdown.

Out-of-the-box syntax highlighting for over 100 languages, no configuration required.

Automatic, smoothly scrolling table of contents on the far left of the page. As you scroll, it displays your current position in the document. It's fast, too. We're using Slate at TripIt to build documentation for our new API, where our table of contents has over 180 entries. We've made sure that the performance remains excellent, even for larger documents.

Let your users update your documentation for you — By default, your Slate-generated documentation is hosted in a public GitHub repository. Not only does this mean you get free hosting for your docs with GitHub Pages, but it also makes it simple for other developers to make pull requests to your docs if they find typos or other problems. Of course, if you don't want to use GitHub, you're also welcome to host your docs elsewhere.

RTL Support Full right-to-left layout for RTL languages such as Arabic, Persian (Farsi), Hebrew etc.

Getting started with Slate is super easy! Simply fork this repository and follow the instructions below. Or, if you'd like to check out what Slate is capable of, take a look at the sample docs.

Getting Started with Slate
Prerequisites
You're going to need:

Linux or macOS — Windows may work, but is unsupported.
Ruby, version 2.3.1 or newer
Bundler — If Ruby is already installed, but the bundle command doesn't work, just run gem install bundler in a terminal.
Getting Set Up
Fork this repository on GitHub.
Clone your forked repository (not our original one) to your hard drive with git clone https://github.com/YOURUSERNAME/slate.git
cd slate
Initialize and start Slate. You can either do this locally, or with Vagrant:
# either run this to run locally
bundle install
bundle exec middleman server

# OR run this to run with vagrant
vagrant up
You can now see the docs at http://localhost:4567. Whoa! That was fast!

Now that Slate is all set up on your machine, you'll probably want to learn more about editing Slate markdown, or how to publish your docs.

If you'd prefer to use Docker, instructions are available in the wiki.

Note on JavaScript Runtime
For those who don't have JavaScript runtime or are experiencing JavaScript runtime issues with ExecJS, it is recommended to add the rubyracer gem to your gemfile and run bundle again.

Companies Using Slate
NASA
Sony
Best Buy
Travis-CI
Greenhouse
WooCommerce
Dwolla
Clearbit
Coinbase
Parrot Drones
Scale
You can view more in the list on the wiki.

Questions? Need Help? Found a bug?
If you've got questions about setup, deploying, special feature implementation in your fork, or just want to chat with the developer, please feel free to start a thread in our Spectrum community!

Found a bug with upstream Slate? Go ahead and submit an issue. And, of course, feel free to submit pull requests with bug fixes or changes to the dev branch.

Contributors
Slate was built by Robert Lord while interning at TripIt.

Thanks to the following people who have submitted major pull requests:

@chrissrogers
@bootstraponline
@realityking
@cvkef
Also, thanks to Sauce Labs for sponsoring the development of the responsive styles.

Special Thanks
Middleman
jquery.tocify.js
middleman-syntax
middleman-gh-pages
Font Awesome
© 2019 GitHub, Inc.
Terms
Privacy
Security
Status
Help
Contact GitHub
Pricing
API
Training
Blog
About
