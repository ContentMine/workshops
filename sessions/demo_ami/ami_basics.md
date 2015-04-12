
![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

# AMI basics

## Commands

AMI is run from the command line, so you'll need to open a terminal window. If you're using the ContentMine VM, it should look like this:

![ContentMine VM terminal screenshot](http://i.cubeupload.com/JsSiu1.png)

To run AMI, you use a command that starts with `ami2-` and ends with the name of the plugin you want to use. For example:

- `ami2-words` will run AMI with the words plugin

Other options are:

- `ami2-species`
- `ami2-identifier`
- `ami2-sequence`
- `ami2-regex`

If you run any of the commands above with no arguments, they will print out help messages. For example if you run `ami2-words`, you will get output that looks like the following (only the first few lines are printed here):

```
-i or --input  file(s)_and/or_url(s)

INPUT:
Input stream (Files, directories, URLs), Norma tries to guess reasonable actions.
also expands some simple wildcards. The argument can either be a single object, or a list. Within objects
the content of curly brackets {...} is expanded as wildcards (cannot recurse). There can be multiple {...}
within an object and all are expanded (but be sensible - this could generate the known universe and crash the
system. (If this is misused it will be withdrawn). Objects (URLs, files) can be mixed but it's probably a
poor idea.

The logic is:
(a) if an object starts with 'www' or 'http:' or 'https;' it's assumed to be a URL
(b) if it is a directory, then the contents (filtered by extension) are added to the list as files
(c) if it's a file it's added to the list
the wildcards in files and URLs are then expanded and the results added to the list

Current wildcards:
{n1:n2} n1,n2 integers: generate n1 ... n2 inclusive
{foo,bar,plugh} list of strings
```

This text describes all the arguments that can be passed to the particular AMI plugin that you are running. For example, the text above describes the `--input` argument, which tells you that when running the command, you can tell it what file to use as input, like this:

```
ami2-words --input scholarly.html
```

## Specifying which files to analyse

AMI needs to be told which files you want to analyse. Typically, a user will have collected some scholarly documents from the internet. These might have been scraped, for example with [quickscrape](https://github.com/ContentMine/quickscrape), or downloaded in bulk some other way. AMI expects the documents to be collected into a directory structure so that each unique document has its own directory, and all files associated with that document are contained within that directory.

For example, if we want to analyse an article from the PeerJ: https://peerj.com/articles/708/. If we used [quickscrape](https://github.com/ContentMine/quickscrape) to download the document and associated resources, it will create a directory structure like this:

```

```
