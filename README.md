# LazyTest: tests for developers too lazy to write them
I don't mind writing documentation that includes examples, but I don't really
like to write tests. LazyTest finds `bash` fenced code regions in Markdown
files and interprets them as examples that should produce the demonstrated
output. For example:

```bash
$ seq 4                 # command with output below
1
2
3
4
```

This works by compiling Markdown files into bash scripts that contain the
assertions.

## Using LazyTest
```sh
$ ./lazytest *.md > tests.sh
$ bash tests.sh && echo 'woohoo, everything works'
```

You can write shell script snippets that aren't tests by using `sh` instead of
`bash`; for example:

```sh
# this code region won't turn into a test
$ false
woohoo, you ran /bin/false!
```

## How to write examples
LazyTest is pretty good at figuring out what's a command and what's
expected-output, but there are a few things that can throw it off. First,
things that work:

### Multiline strings
These work most of the time because lazytest reads until it has an even number
of them.

```bash
$ echo 'multiline single-
quoted strings' | perl -lne 'print "$_ FTW"'
multiline single- FTW
quoted strings FTW
```

```bash
$ echo "that's confusing"
that's confusing
```

```bash
$ echo "it's also possible
to have multiline double quoted strings"
it's also possible
to have multiline double quoted strings
```

### Line continuations
```bash
$ seq 1 \
      5
1
2
3
4
5
```

### Heredocs
You can have a heredoc and lazytest will figure out where it ends:

```bash
$ cat <<EOF
this
is
a
test
EOF
this
is
a
test
```

## Conditionals
Anything you put into a `lazytest` code block will appear verbatim in the
compiled script. This makes it possible to conditionalize execution for things;
for example:

```lazytest
# Skip over the following stuff because it's lame
if false; then
```

```bash
$ echo hi               # a noncompliant echo
nope
```

```lazytest
fi
```

Similarly, you can use for-loops:

```lazytest
for i in `seq 100`; do
```

```bash
$ echo the same test
the same test
```

```lazytest
done
```
