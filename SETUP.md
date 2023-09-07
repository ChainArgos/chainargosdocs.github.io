# Notes on setting up github pages with jekyll

starting with the instructions [here](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll) is fine

but there are a few things to remember:
1. the eventual url will be github_organization.github.io/repo_name. do not name the repo the full url you want like a lot of docs say.
2. set baseurl in _config.yml to /repo_name
3. set domain in _config.yml  to github_organization.github.io
4. set url in _config.yml to http://[domain]
5. do not put things under a docs or any other directory (even though the docs tell you too!). everything goes in the root directory of the repo: _posts, _config.yml, Gemfile etc
6. deploy from / (root) on the github pages config page. there is probably a way to get this to work -- except i was unable to get the post links to work when using anything other than /. they always 404ed if this isnt root.

local testing with
```
bundle exec jekyll serve
```
will produce a working site even if you have all of that set incorrectly!
but the github deploy will not work. and it will often not generate any error messages.

## images

images are also weird. put images under /assets/images in the repo. reference them as
```
![](/docs/assets/images/image_file_name)
```
the name should include .png or .jpg or whatever.

note this will not work on the local test. it's broken locally but works on the public site.

## notes
1. if a post has a timestamp in the future it will not get published