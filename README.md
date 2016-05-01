### Credit

I use [Jekyll Now](https://github.com/barryclark/jekyll-now) to generate my blog pages. It takes care of all of the HTML and CSS, and has made making this blog incredibly easy.

I went to the GitHub repo linked above, and followed the instructions there.

Big ass literal link to the repo: <https://github.com/barryclark/jekyll-now>

### Jekyll Edits

I've made some changes to the default Jekyll Now files. I'll document them here I guess:

#### Tables

I added the following code to the `style.scss` file to get table borders and styling to render. I got the code [here](https://gist.github.com/andyferra/2554919)

```css
table {
  padding: 0; }
  table tr {
    border-top: 1px solid #cccccc;
    background-color: white;
    margin: 0;
    padding: 0; }
    table tr:nth-child(2n) {
      background-color: #f8f8f8; }
    table tr th {
      font-weight: bold;
      border: 1px solid #cccccc;
      text-align: left;
      margin: 0;
      padding: 6px 13px; }
    table tr td {
      border: 1px solid #cccccc;
      text-align: left;
      margin: 0;
      padding: 6px 13px; }
    table tr th :first-child, table tr td :first-child {
      margin-top: 0; }
    table tr th :last-child, table tr td :last-child {
      margin-bottom: 0; }
```

#### Tags

I followed [this guide](http://blog.meinside.pe.kr/Adding-tag-cloud-and-archives-page-to-Jekyll/) to add tagging to my posts.

I ended up moving the location of the tags on my posts.
