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
