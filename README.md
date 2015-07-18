# Trunk Club Tech Blog

This is where we write about our tech team!

## FAQ
* Does my blog post need to be technical?
  * No! We want to reflect our culture in this blog. Anything TC-related goes!
* How do I publish a post?
  * See directions below
* How do I attach an image to a post?
  * Place the image in the `assets` folder, check it into the repository with your post, and reference it at `/assets/image_name.jpg`.
* How do I explicitly set the cutoff for a blog post preview/excerpt?
  * For an example, see the metadata at the top of `_posts/2015-07-17-pairing-with-product-managers.markdown`. The Jeklly "front-matter" accepts an option for except separator, so you can add `excerpt_separator: <!--more-->` to that configuration. Then simply add `<!--more-->` to your markdown where you'd like the excerpt to be cut off.
    * Without this option, Jekyll will automatically take the first paragraph.
    * If you provide this option but do not include `<!--more-->`, the entire post will be displayed on the front page.

## Writing and Publishing a Blog Post
Thanks for writing a post! Let's walk through how to get it live. It's easy, I promise!
* Clone this repository to your local machine
* Duplicate the latest existing post and modify the file name so the date and title are reflected appropriately.
* Write your post in markdown. You can base the metadata - title, author, tags, etc. - on one of the existing posts. It's at the very beginning of the post.
  * There are lots of tools for editing Markdown. In Atom, press `Ctrl+Shift+M` to preview your markdown while writing.
* Save your file.
* Run `jekyll serve` in the repository on your local machine. If you haven't installed Jekyll, follow the instructions on the website: http://jekyllrb.com/
* Preview your post. By default, the Jekyll server will run at `localhost:4000`
* If everything looks good, check out a new branch (e.g. `pairing-with-product-managers`) and submit a PR with a couple people from the team you'd like to proof your writing.
  * Proof readers should check for grammar, consistency, spelling, etc.
* When everything is set, merge to master and check out the glory you've added to `techblog.trunkclub.com`!
