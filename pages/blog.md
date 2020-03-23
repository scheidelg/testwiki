# Blog

<hr class="tight">
## MDwiki and File Timestamps

I thought it would be nice to include timestamps on each of the pages displayed on this MDwiki.  Initially I just put a static timestamp similar to what I put at the bottom of each one of these 'blog' entries, something like:

```
2020.03.23 13:34 ET
```

Then I decided that it would be nice if the timestamp were formatted in italics, slightly smaller text, and under a `<hr>` tag.  So I updated the CSS file to include some formatting specifically for this:

```CSS
/*
 * Create a horizontal ruler style that doesn't have top or bottom margins,
 * for a cleaner look - particularly when used at the bottom of a page.
 */
hr.tight {
   margin-top: 0;
   margin-bottom: 0;
   border-color: #ffffff;
}

/*
 * Create a paragraph style that can be used for timestamp 'footers' at the
 * bottom of each page.
 *
 * Note: Instead of creating a top border, I'm using an <hr> above each
 * timestamp for improved portability if a different them is used.
 */
p.timestamp {
    margin-bottom:0;
    margin-top:0;
    padding-top: 5px;
    font-size: 75%;
    font-style: italic;
}
```

And then updated the timestamp text at the bottom of each \*.md file:

```HTML
<hr class="tight"><p class="timestamp">2020.03.23 13:34 ET</p>
```

That looked pretty good to me.

However, it was annoying that I'd have to manually update that timestamp every time I edited an `*.md` file. After a bit of research and testing, I replaced the timestamp footer with a bit of JavaScript:

```JavaScript
<hr class="tight"><p class="timestamp" id="timestamp"></p>
<script type='text/javascript'>document.getElementById("timestamp").innerHTML = Date(document.lastModified);</script>
```

This JavaScript a simple hack that retrieves the timestamp from the Last-Modified HTTP response header, converts it to a Date object, and then sets an 'id' tag to the text representation of that Date object. The HTML code `<p id="timestamp"></p>` creates a paragraph that references the value set for the 'id' tag.

My intent was that this would automatically render the modification timestamp for that page without me having to manually edit the text at the bottom of the file.

Unfortunately a bit of testing showed that when I modified one file (e.g., the `blog.md` file), the timestamp rendered on another page (e.g., the 'home' page that loades the `index.md` file) was updated as well.

*\*le sigh\**

I theorized that when I updated a single file in my GitHub repository, GitHub pushed (or at least 'touched') every file in the repository.  So from the GitHub web server's perspective, every single file is updated every time a single change is committed.

I tested this out by firing up WireShark, modifying the `blog.md` file, and retrieving the `index.md` and `config.json` files directly (e.g., `www.scheidel.net/index.md`, `www.scheidel.net/config.json`).  Sure enough, the packet capture showed that the Last-Modified HTTP response header for those files matched the commit time for the `blog.md` file.

So even if I ran some JavaScript that bypassed MDwiki and expressly retrieved the backend `*.md` file (e.g., <a href="https://stackoverflow.com/questions/2313620/is-it-possible-to-retrieve-the-last-modified-date-of-a-file-using-javascript" target="_blank">https://<span></span>stackoverflow.<span></span>com/questions/2313620/is-it-possible-to-retrieve-the-last-modified-date-of-a-file-using-javascript</a>), the Last-Modified value would still always be the timestamp of the last commit for the entire repository.

I could potentially write some JavaScript code to access the GitHub API and identify the most commit date for each individual file (e.g., <a href="https://stackoverflow.com/questions/50194241/get-when-the-file-was-last-updated-from-a-github-repository" target="_blank">https://<span></span>stackoverflow.<span></span>com/questions/50194241/get-when-the-file-was-last-updated-from-a-github-repository</a>).  But that's more work then I want to spend on this particular problem right now.
  
So for now I'm going with a footer that has:

 * an automatically updated timestamp from the Last-Modified HTTP response header as the 'site last modified' timestamp, and
 
 * a manually edited timestamp for the 'page last modified' timestamp.

```HTML
<hr class="tight"><p class="timestamp">Page updated: 2020.03.23 14:10 ET -- Site updated: <span id="timestamp"></span></p>
<script type='text/javascript'>var timestamp=new Date(document.lastModified); document.getElementById("timestamp").innerHTML = timestamp;</script>
```

I'll log this as an 'issue' in my GitHub repository for this site and come back to this later.

_**#MDwiki #blog #JavaScript** | 2020.03.23 14:10 ET_

<hr class="tight">
## MDwiki and Blogging

Initially I was hoping to integrate a basic blog-like capability into this website, using MDwiki's 'iframe' gimmick.  The <a href="http://dynalon.github.io/mdwiki/#!blog.md" target="_blank">http://<span></span>dynalon.<span></span>github.<span></span>io/mdwiki/#!blog.md</a> webpage demonstrates this with a `blog.md` file that consists solely of:

```markdown
MDwiki Blog
====

[gimmick:iframe](http://dynalon.tumblr.com/tagged/MDwiki)
```

Unfortunately this utterly failed when I tested this out by using exactly that same code and then with 'example.com' as the test domain. Since the point of this web site is to have an easy way to collect, manage, and share content and work - and **not** to spend an inordinate amount of time building the web site itself - I'm just letting that go for the moment. I'll do just fine writing notes here with date+time stamps and a bit of extra attention when I post something there.

_**#MDwiki #blog** | 2020.03.22 01:03 ET_

<hr class="tight"><p class="timestamp">Page updated: 2020.03.23 14:10 ET -- Site updated: <span id="timestamp"></span></p>
<script type='text/javascript'>var timestamp=new Date(document.lastModified); document.getElementById("timestamp").innerHTML = timestamp;</script>
