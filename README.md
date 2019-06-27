# C++ Standard Committee Papers

The C++ Standard paper(s) that I have written.

Official C++ Standard Committee papers are available from [the C++ mailings][].

More information on the C++ Standard Committee is available on [the Committee site][].

To create a html version
* Install [pandoc][]
* Run the following command  
`pandoc input.md -f markdown_github --metadata pagetitle="title" -f gfm -s --include-in-header github.css -o output.html`

  [the C++ mailings]: http://open-std.org/jtc1/sc22/wg21/docs/papers/
  [the Committee site]: https://isocpp.org/std/the-committee
  [pandoc]: https://pandoc.org/
