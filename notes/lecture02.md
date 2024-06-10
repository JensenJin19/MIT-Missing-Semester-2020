# Shell Tools and Scripting

This lecture present some of the basics of using `bash` as a scripting language along with a number of shell tools.

## Shell Scripting

`Shell scripting` is optimized for performing shell-related tasks. For this section, bash scripting is spotlight since it's the most common script

Assign variables in bash:

```Bash
foo=bar
echo "$foo"
# prints bar
echo '$foo'
# prints $foo
```

Strings in bash with `'` or `"` delimiters:
- `'`: Literal strings that will not substitute variable values
- `"`: Will substitute

> NOTE
> `foo = bar` will not work since it's interpreted as calling the `foo` program with arguments `=` and `bar`

As with most programming languages, bash supports control flow techniques including `if`, `case`, `while` and `for`

```Bash
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```