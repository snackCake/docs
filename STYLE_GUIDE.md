## Headers

1. The very first line in every markdown file should be the title of the page. In most cases, the title will be the same as the name of the page in the sidebar. This should be represented by an H1 tag, which is generated with the following syntax:

    ```md
    # My title
    ```

1. Subsequent headers should be formatted similarly, with each level incrementing the number of `#` symbols by one. Don't go from an H1 to an H3.

1. There should only ever be one H1 on any given page. Other H tags do not have a count restriction.

## Lists

1. Prefer using native markdown lists, as they provide the extra bonus of not having to worry about which number your step is. You should number every single step as 1. Markdown will automatically parse that as the next available number when rendering. Take a look at the spacing example below.

1. Prefer using dashes instead of asterisks for unordered lists. This clears up some confusing with using asterisks for bold.

## Spacing

1. Fenced code blocks (the three backticks) must **always** have a blank line above them. They do not necessarily need a new line after them, but it typically improves readability in the markdown file to have it.

1. Having code blocks within lists requires the following syntax (with 3 closing backticks, not 2 as shown below)

    ```text

    1. My first step

        ```java
        some java code
        ``

        > Here's a blockquote used for notes

    1. My second step

        ```xml
        some xml code
        ``
    ```

## Tables

1. Prefer using native markdown tables than writing your own HTML tags. This can be achieved with pipes and dashes, as described in [this help page](https://help.github.com/articles/github-flavored-markdown#tables)

## Links

1. When linking to an internal documentation piece, you should use the double square bracket syntax, [[Other Page Name]]. Note that Docula will automatically replace that with the URL of the other page, provided there exists another page with matching name. Name matching is done by lowercasing the filename, replacing dashes with spaces, and trimming out the extension.

1. When linking to external documentation, use the traditional Markdown syntax of `[friendly name](link-url)`

1. Docula supports a special markdown syntax for linking to associated Javadocs for the given module. This means that when you're in framework, you can link to framework Javadocs. When you're in a module, you can link to Javadocs in that module. Javadoc versions are associated with documentation versions in Docula. To link to the Javadoc for a given class, you can use:

    ```md
    ^[javadoc:org/broadleafcommerce/core/catalog/domain/Product]
    ```
If the current documentation version is set to point to the 3.1.0-GA, the generated link will be http://javadocs.broadleafcommerce.com/core/3.1.0-GA/index.html?org/broadleafcommerce/core/catalog/domain/Product.html
