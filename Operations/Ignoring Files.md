There are classes of files that do not need to be added, since they're either automatically generated, dependent on the user using the repository or log files that may contain sensitive data. Therefore, a `.gitignore` file has either the name of the files or patterns that exclude such files.

The rules for the patterns you can use are as follows:

- Blank lines or lines starting with `#` are ignored;
- Standard glob patterns work, and will be applied recursively throughout the entire working tree;
- You can start patterns with a forward slash (`/`) to avoid recursivity;
- You can end patterns with a forward slash (`/`) to specify a directory;
- You can negate a pattern by starting it with an exclamation point (`!`);
- Glob patterns are like simplified regular expressions that shells use. An asterisk (`*`) matches zero or more characters; `[abc]` matches any character inside the brackets (in this case a, b, or c); a question mark (`?`) matches a single character; and brackets enclosing characters separated by a hyphen (`[0-9]`) matches any character between them (in this case 0 through 9). You can also use two asterisks to match nested directories; `a/**/z` would match `a/z`, `a/b/z`, `a/b/c/z`, and so on.


