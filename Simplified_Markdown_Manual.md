# Markdown manual
This file is a simplified doc referred the website [markdown basic syntax](https://www.markdownguide.org/basic-syntax/#overview).

## Headings
To create a heading, add number signs(`#`) in front of a word or phrase. The number of number signs you use should correspond to the heading level.

For example, one number sign `#` in front of `heading` means first level heading.
> Alternatively, on the line below the text, add any number of `==` characters for heading level 1 or `--` characters for heading level 2.

## Emphasis
### Bold
To bold text, add two asterisks(`**`) or underscores(`__`) befor and after a word or phrase.

For example, `**bold text**` or `__bold text__` outputs **bold text**.

### Italic
To italicize text, add one asterisk or underscore before and after a word or phrase. To itelicize the middle of a word for emphasize, add one asterisk without space around the letters.

For example, `*italic*` or `_italic_` outputs *italic*. `it*ali*c` outputs it*ali*c.

### Bold and Italic
To emphasize text with bold and italics at the same time, add three asterisks or underscores before and after a word or phrase.

For example, `***bold and italic***` or `___bold and italic___` outputs ***bold and italic***.

## Blockquotes
To create a blockquote, add a `>` in front of a paragraph.
> Dell EMC is the most famous storage company in this world.

### Nested Blockquotes
Blockquotes can be nested. Add a `>>` in front of the paragraph you want to nest.
> Dell EMC is the most famous storage company in the world.
>> This company is created in 1984.

### Blockquotes with other elements
Blockquotes can contain other Markdown formatted elements. Not all elements can be used -- it depends on specific editors.
> ### Blockquote looks fantastic.
> ### Heading `OK`
> **Bold** `OK`
> 
> *Italic* `OK`

## List
### Order Lists
To create an ordered list, add line items with numbers followed by periods. The numbers don't have to be in numberical order, but the list should start with the number one.

For example:
1. First item
2. Second item
3. Third item

### Unordered Lists
To create an unordered list, add dash(`-`), asterisk(`*`), or plus sign(`+`) in front of line items. 

For example:
- `-` First item
* `*` Second item
+ `+` Third item

### Adding elements in Lists
To add another element in a list while preserving the continuity of the list, indent the element four spaces or one tab.

For example:
+ First item
  + 1.1 sub list
+ Second item
  
  2.2 paragraph
+ Third item
  > 3.3 Blockquote

## Code Blocks
Code blocks are normally indented four spaces or one tab. When they're in a list, indent them eight spaces or two tabs.

Alternatively, we can use ` ``` ` in front of and behind the code blocks.

For example:
```cpp
#incldue <iostream>
using namespace std;
int main() {
    cout << "test case!" << endl;

    return 0;
}
```
## Footnotes
Footnotes allow you to add notes and references without cluttering the body of the document. When you create a footnote, a superscript number with a link appears where you added the footnote reference. Readers can click the link to jump to the content of the footnote at the bottom of the page.

To create a footnote reference, add a caret(`^`) and an identifier inside brackets(`[^1]`). Identifier can be numbers or words, but they can't contain spaces or tabs. Identifiers only correlate the footnote reference with the footnote itself -- in the output, footnotes are numbered sequentially.

Add the footnote using another caret and number inside brackets with a colon and text(`[^1]: footnote.`). You don't have to put footnotes at the end of the doc. You can put them anywhere except inside other elements like lists, block quotes and tables.

For example:

Here is a simple footnote,[^1] and here's a longer one. [^bignote]

[^1]: This is the first footnote.

[^bignote]: Here's one with multiple paragraphs and code.

    Indent paragraphs to include them in the footnote.
    `{ my code }`
    Add as many paragraphs as you like.

## Strikethrough
To strikethrough words, use two tilde symbols(`~~`) before and after the words.

For example:

~~The world is flat.~~ We now know that the world is round.

## Task Lists
To create a task list, add dashes(`-`) and brackets with a space(`[]`) in front of task list items. To select a checkbox, add an `x` in between the brackets(`[x]`).

For example:
- [x] Markdown doc
- [ ] LaTex doc
- [ ] Makefile doc

## URL links
To create a link, enclose the link text in brackets and them follow it immediately with the URL in parentheses.

For example:

> `my favouriate search engine is [Duck Duck go](duckduckgo.com).`
> 
> my favouriate search engine is [Duck Duck go](duckduckgo.com).

If the link is worte in doc, markdown will treate it as a link.

For example:

www.baidu.com
### URL and Email address
To quickly turn a URL or email address into a link, enclose it in angle brackets.

<scrdyx@163.com>

<www.google.jp>

## Images
1. Open the file containing the Linux mascot.
2. Marvel at its beauty.
    `![Linux mascot](link)`

    ![Linux mascot](https://upload.wikimedia.org/wikipedia/commons/thumb/5/55/Tux_Enhanced.svg/154px-Tux_Enhanced.svg.png)

3. Close the file.

## Tables

For example:
|1|2|3|4|
|-|-|-|-|
|5|6|7|8|
|9|10|11|12|






