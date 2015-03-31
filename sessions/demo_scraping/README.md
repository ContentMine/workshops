![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

# Scraping

This directory contains materials for tutorials and demonstrations of the scraping pipeline, specifically using [quickscrape](https://github.com/ContentMine/quickscrape) with the [existing scraper set](https://github.com/ContentMine/journal-scrapers/tree/master/scrapers) and creating [new scrapers](https://github.com/ContentMine/journal-scrapers) using the [scraperJSON standard](https://github.com/ContentMine/scraperJSON).

## Getting quickscrape

If we have released a VM with this workshop, the software will already be downloaded there, and able to use immediately. In this case, you can skip ahead to the next section (using quickscrape).

`quickscrape` is very easy to install. Simply:

```bash
sudo npm install --global quickscrape
```

However, `quickscrape` depends on [Node.js](http://nodejs.org), a platform which enables standalone JavaScript apps.

You'll need to install Node if you don't already have it before you can install quickscrape. Follow the instructions below. Currently we only support OSX and Debian/Ubuntu Linux. If you need instructions for another operating system please [create an issue](https://github.com/ContentMine/quickscrape/issues).

#### OSX

The simplest way to install Node.js on OSX is to go to  http://nodejs.org/download/, download and run the Mac OS X Installer.

Alternatively, if you use the excellent [Homebrew](http://brew.sh/) package manager, simply run:

```bash
brew update
brew install node
```

Then you can install quickscrape:

```
sudo npm install --global quickscrape
```

#### Debian

```bash
sudo apt-get update
sudo apt-get install -y nodejs nodejs-legacy
curl --insecure https://www.npmjs.org/install.sh | bash
```

Then you can install quickscrape

```bash
sudo -H npm install --global quickscrape
```

#### Ubuntu

```bash
sudo apt-get install -y software-properties-common build-essential python-software-properties libfontconfig1
sudo add-apt-repository -y ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install -y nodejs
```

Then you can install quickscrape:

```bash
sudo -H npm install --global quickscrape
```

## Using quickscrape

[quickscrape](http://github.com/ContentMine/quickscrape) is a command-line tool that uses the ContentMine web scraping library, [thresher](http://github.com/ContentMine/thresher).

`quickscrape` is not like other scraping tools. It is designed to enable large-scale content mining. Here's what makes it different:

Websites can be rendered in a GUI-less browser ([PhantomJS](http://phantomjs.org) via [CasperJS](http://casperjs.org)). This has some important benefits:

- Many modern websites are only barely specified in their HTML, but are rendered with Javascript after the page is loaded. Headless browsing ensures the version of the HTML you scrape is the same one human visitors would see on their screen.
- User interactions can be simulated. This is useful whenever content is only loaded after interaction, for example when article content is gradually loaded by AJAX during scrolling.
- The full DOM specification is supported (because the backend is WebKit). This means pages with complex Javascripts that use rare parts of the DOM (for example, Facebook) can be rendered, which they cannot in most existing tools.

Scrapers are defined in separate JSON files that follow a defined structure. This too has important benefits:

- No programming required! Non-programmers can make scrapers using a text editor and a web browser with an element inspector (e.g. Chrome, Firefox).
- Large collections of scrapers can be maintained to retrieve similar sets of information from different pages. For example: newspapers or academic journals.
- Any other software supporting the same format could use the same scraper definitions.

Combined with our community-driven [collection of scrapers for journals](http://github.com/ContentMine/journal-scrapers), `quickscrape` allows you to scrape the majority of scientific journal articles on the internet.

In this exercise, we will learn to run `quickscrape`.

### Scraping a single URL

The journal-scrapers collection is available [here](https://github.com/ContentMine/journal-scrapers/tree/master/scrapers), or if there is a VM available for this workshop, the collection is already downloaded to your VM, at `~/journal-scrapers`.

We will scrape an eLife paper: "[Cryo-EM structure of the Plasmodium falciparum 80S ribosome bound to the anti-protozoan drug emetine](http://elifesciences.org/content/elife/3/e03080.full)".

To run quickscrape, we simply need to tell it where to find the scraper collection, and what URL we want to scrape. Open up a terminal and run the following:

```bash
$ quickscrape --url http://elifesciences.org/content/elife/3/e03080.full --scraperdir ~/journal-scrapers/scrapers
```

quickscrape will automatically select the correct scraper from its collection, download the page, and extract the specified elements, downloading any linked files specified in the scraper definition.

You can examine the results in the `output` directory:

```
output
├── http_elifesciences.org_content_3_e03080
    ├── fulltext.pdf
    ├── fulltext.xml
    └── results.json
```

The file `results.json` contains all the scraped data, while the files `fulltext.xml` and `fulltext.pdf` are linked files that were downloaded and renamed.

### Scraping a list of URLs

quickscrape can run in batch-mode, where it iterates over a list of URLs, scraping each of them.

Here are 5 recently published articles from the journal Molecules published by MDPI ([here's a script to get them yourself](https://gist.github.com/Blahah/e669f1d3899dcb392564)).

Create a file `urls.txt`:

```
http://www.mdpi.com/1420-3049/19/2/2042/htm
http://www.mdpi.com/1420-3049/19/2/2049/htm
http://www.mdpi.com/1420-3049/19/2/2061/htm
http://www.mdpi.com/1420-3049/19/2/2077/htm
http://www.mdpi.com/1420-3049/19/2/2089/htm
```

Now run `quickscrape` with the `--urllist` option:

```bash
quickscrape \
  --urllist urls.txt \
  --scraperdir ~/journal-scrapers/scrapers
```

Notice that `quickscrape` rate-limits itself to 3 scrapes per minute. This is a conservative limit, but note that imposing some kind of limit is important. It is a basic courtesy to the sites you are scraping - you wouldn't block the door of a library (I hope!), so don't take up more than a reasonable share of a site's bandwidth.

Your results are organised into subdirectories, one per URL:

```bash
$ tree output
output/
├── http_www.mdpi.com_1420-3049_19_2_2042_htm
│   ├── fulltext.html
│   ├── fulltext.pdf
│   ├── molecules-19-02042-g001-1024.png
│   ├── molecules-19-02042-g002-1024.png
│   └── results.json
├── http_www.mdpi.com_1420-3049_19_2_2049_htm
│   ├── fulltext.html
│   ├── fulltext.pdf
│   ├── molecules-19-02049-g001-1024.png
│   ├── molecules-19-02049-g002-1024.png
│   ├── molecules-19-02049-g003-1024.png
│   ├── molecules-19-02049-g004-1024.png
│   ├── molecules-19-02049-g005-1024.png
│   └── results.json
├── http_www.mdpi.com_1420-3049_19_2_2061_htm
│   ├── fulltext.html
│   ├── fulltext.pdf
│   ├── molecules-19-02061-g001-1024.png
│   ├── molecules-19-02061-g002-1024.png
│   ├── molecules-19-02061-g003-1024.png
│   ├── molecules-19-02061-g004-1024.png
│   └── results.json
├── http_www.mdpi.com_1420-3049_19_2_2077_htm
│   ├── fulltext.html
│   ├── fulltext.pdf
│   ├── molecules-19-02077-g001-1024.png
│   ├── molecules-19-02077-g002-1024.png
│   ├── molecules-19-02077-g003-1024.png
│   ├── molecules-19-02077-g004-1024.png
│   ├── molecules-19-02077-g005-1024.png
│   ├── molecules-19-02077-g006-1024.png
│   ├── molecules-19-02077-g007-1024.png
│   └── results.json
└── http_www.mdpi.com_1420-3049_19_2_2089_htm
    ├── fulltext.html
    ├── fulltext.pdf
    ├── molecules-19-02089-g001-1024.png
    ├── molecules-19-02089-g002-1024.png
    └── results.json

5 directories, 40 files
```

Note that the extracted figures *may* be suitable for processing by AMI.
