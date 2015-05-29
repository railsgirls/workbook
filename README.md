# RailsGirls Workbook ![Travis CI](https://travis-ci.org/railsgirls/workbook.svg?branch=master)

These are the sources for the RailsGirls Workbook. It is build using HTML and CSS with the intention to make it easier to collaboratively edit its contents. 

## Downloads

* [Download finished PDF releases of the Workbook](https://github.com/railsgirls/workbook/releases)
* [Download the development PDF version of the Workbook](https://s3.eu-central-1.amazonaws.com/railsgirls-workbook/workbook.pdf)

## How to contribute

### Small contributions

The simplest way to contribute small changes to the content of the workbook is to use [GitHub's file editing feature](https://help.github.com/articles/editing-files-in-another-user-s-repository/). Just open the [`index.html.erb`](https://github.com/railsgirls/workbook/blob/master/source/index.html.erb) file and start editing by clicking the pen icon. After you're done editing, commit your changes and create a new pull request. 


### Advanced contributions

For larger changes, such as changes to the styling, it's recommended to fork and check out the repository. You must have [Ruby](http://ruby-lang.org) and [Git](http://git-scm.com) installed. Start by [forking the workbook repository](https://help.github.com/articles/fork-a-repo/) and [cloning your fork](https://help.github.com/articles/cloning-a-repository/) using this command in your Terminal:

```shell
git clone https://github.com/<your-github-username>/workbook
```

Afterwards, you can switch into the new directory and run the setup command, to install the necessary dependencies.

```shell
cd workbook
rake setup
```

Open the directory in your favorite editor and start changing the contents of the `source` directory. To preview your changes run `rake preview` and point your browser to [http://localhost:4567/](http://localhost:4567/). After you've made some changes, simply reload the page to see the updated preview. For your convenience you may also install the [LiveReload browser extension](http://livereload.com/extensions/), which will automatically reload the page after you save a file.

*Please bear in mind that this is only a simulation of what the finished PDF will look like. There might be slight differences between the preview and the generated PDF*.

When you're done editing, commit your changes, push them to GitHub and [create a new pull request](https://help.github.com/articles/creating-a-pull-request/).


## How this works under the hood

The project is using [Middleman](http://middlemanapp.com) to generate the workbook's HTML source. The generated HTML is sent to [DocRaptor](http://docraptor.com) and transformed into a PDF. The process is automated using [Travis CI](http://travis-ci.org/railsgirls/workbook) which will push the generated PDF to [Amazon S3](http://aws.amazon.com/s3/). 

## Todos

* Create basic CSS framework to style pages
