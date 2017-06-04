Interactive fiction
=======================

Version `0.12.0` added experimental support for writing interactive fiction. 

## Basics ##

If you want to have a non-linear story, you can simply use Markdown
links just as you would for any other link:

```markdown
* [Open the treasure chest](open_chest.md)
* [It might be trapped, stay away from it](stay_away.md)
```

All Crowbook renderers should render this correctly, allowing the
reader to "choose her adventure". Note, however, that you still need
to include all these Markdown files in you book configuration files. 

## The interactive fiction renderer ##

While the above allows you to generate correct EPUB and PDF files, it
will still display all the content if the reader chooses to read your
book linearly. While this may not be a problem, you might want to only
display the part of the book that the reader is actually exploring. 

In order to do so, you can use the interactive fiction html renderer:

```yaml
output.html.if: my_book.html
```

This output is similar to the standalone HTML output, except the
option to display only a chapter at a time is always true, and there
is no way to display the table of contents. 

## Using Javascript in your interactive fiction

While the above allows the reader to choose his own path, its
interactivity is quite limited. With the interactive fiction renderer,
it is possible to include Javascript code in your Markdown files,
using a code block element: 

```markdown
You open the chest, and you find a shiny sword. Yay!

    user_has_sword = true;
```

This Javascript code can return a string value, which will be displayed
inside the document according to the reader's previous choices:

```markdown
You encounter a goblin, armed with a knife!

    if (user_has_sword) {
	    return "You kill him with your sword, congratulation!";
	} else {
	    return "You don't have any weapon, you die :(";
	}
```

If you want to include Markdown formatting in the text you return, you
can use the `@"..."@` syntax:

```markdown
You face a troll!

    if (user_has_sword) {
	    return @"* [Attack him with your sword](fight_troll.md)"@;
	} else {
	    return @"* [Better run away](run_away.md)"@;
	}
```

> Note that *only* the interactive fiction renderer supports this way
> of embedding Javascript code. If you try to render a document
> containing such code blocks to EPUB, PDF, or the "normal" HTML
> renderer, they will be displayed as regular code blocks. 