# 高效搜索 github 开源项目

When I was a beginner to open-source contributions, one of my greatest challenge was finding the correct projects/issues to work on.

For the longest time I relied on resources curated by different writers on the internet (which were good, by the way). But I always wanted to find a way around this problem – a way I could search for and track projects that were right for my skill set.

Let's agree on one thing: unlike Google, searching GitHub is not easy. But as a developer, chances are that you'll interacting with GitHub or Gitlab on a daily basis.

Now the question isn't what you use these version control systems for, but how you are using them. Just like mastering Google search skills is essential for any regular internet user, I believe it's also essential for developers to learn how to effectively search GitHub.

In this article we are going to take a look at different techniques you can use to correctly search GitHub. You'll learn how to search through:

-   Issues and Pull Requests
-   Repositories
-   Users
-   Topics
-   And more. Let's get started.

## GitHub Search Queries

In order to find detailed information about something on the internet, you need to have the correct searching skills. It's not any different with GitHub – to find detailed info you can utilize common filter, sort, and searching techniques to easily find specific issues and pull requests of a given project.

Even though you have multiple resources listed on the internet for different projects, the main problem comes in when you want to do a search by yourself. How do you get started? Which keywords should you use to find the correct results?

Most maintainers tend to label their projects with issues, which makes it easier for contributors to find suitable projects. Listed below are some of the tricks that might help you out when you are using GitHub.

### How to Search Issues and Pull Requests on GitHub

One of the most common ways of finding projects to contribute to is by searching through issues and related PRs. Here are some tricks you can use to easily find reliable answers:

-   `is:issue is:open label:beginner` - This particular query will list all projects with issues that are open and labeled beginner.
-   `is:issue is:open label:easy` - This will list all open issues that are labeled easy.
-   `is:issue is:open label:first-timers-only` - This lists all open issues that welcome first-timer contributions.
-   `is:issue is:open label:good-first-bug` - This lists projects with open issues labeled good-first-bug, to attract contributors to work on them.
-   `is:issue is:open label:"good first issue"` - This will list all open issues with the label good first issue, meaning it is good for place for beginners to get started.
-   `is:issue is:open label:starter` - This lists all open issues from across GitHub that are labeled starter.
-   `is:issue is:open label:up-for-grabs` - This lists open issues that are ready to be worked on if you have the necessary skills.
-   `no:project type:issue is:open` - This will list all open issues that are not assigned to a specific project.
-   `no:milestone type:issue is:open` - Many times, projects are tracked with milestones. But if you want to find issues that are not tracked, this search query will list those projects for you.
-   `no:label type:issue is:open` - This lists all open issues that are not labeled.
-   `is:issue is:open no:assignee` - This shows all open issues that have not yet been assigned to a person.

### How to Search Repositories

By default, to make a search you will type the repository name in the search bar and voilà! You get some search results.

But the chances of you landing on the exact repo you intended are very low.

Let's look at some ways you can narrow down your search:

### How to Find by Name, Description/README

A thing to note when you search by Name and description of the README file is that your search phrase should begin with the `in` qualifier. This makes it possible to search "inside" what you are looking for.

#### Example

Using `in:name`. Let's say you are looking for resources to learn more about Data Science. In this case, you can use the command `Data Science in:name` which will list repositories with Data Science in the repository name.

Using `in:description`. If you want to find repositories with a ceratin description, for example repositories where the term "freeCodeCamp" is included in the descriptionm, our search will be: `freecodecamp in:description`

Using `in:readme`. You use this to search through a README of a file for a certain phrase. If we want to find repositories where the term freecodecamp is included in the README, our search will be: `freecodecamp in:readme`.

Using `in:topic`. You use this to find if a certain phrase or word is labeled in the topics. For example to find all repositories where freecodecamp is listed in the topic, our search will be: `freecodecamp in:topic`

You can also combine multiple search queries to further narrow down the search.

### How to Find by Stars, Forks

You can also search for a repository based on how many stars and forks the project has. This makes it easier for you to know how popular the project is.

#### Examples

Using `stars:n`. If you search for a repository with 1000 stars, then your search query will be `stars:1000`. This will list repositories with exactly 1000 stars.

Using `forks:n`. This specifies the number of forks a repository should have. If you want find repositories that have less than 100 forks, your search will be: `forks:<100`.

The good thing is that you can always use relational operators like `<`, `>`, `<=`, `>=` & `..` to help you further narrow your search.

### How to Find by Language

Another cool way to search through GitHub is by language. This helps you filter out repositories to a specific language.

#### Example:

Using `language:LANGUAGE`. For example if you want to find repositories written in PHP, your search will be: `language:PHP`
How to Find by Organization Name
You can also search repositories/projects that are maintained or created by a specific organization. For this you need to begin your search with the keyword org:... followed by the organization name.

For example if you search `org:freecodecamp` it will list repositories that match freeCodeCamp.

### How to Find by Date

If you want your results based on a specific date, you can search using one of these keywords: created, updated, merged and closed. These keywords should be accompanied by date in the format YYYY-MM-DD.

#### Example:

Using `keyword:YYYY-MM-DD`. Take an instance where we want to make a search of all repositories with the word freeCodeCamp that were created after 2022-10-01. Then our search will be: `freecodecamp created:>2022-10-01`
You can also use `<`, `>`, `>=` and `<=` to search for dates after, before and on the specified date. To search within a range you can use ....

### How to Find by License

Licenses are very important when you are are looking for a project to contribute to. Different licenses give different rights as to what a contributor can do or can not do.

To make it easier for you to find projects with right licenses you need to have a good understanding of licenses. You can read more about them here.

#### Example:

Using `license:LICENSE_KEYWORD`. This is a good way to search for projects with specific licenses. To search projects with the MIT license, for instance, you would do `license:MIT`.

### How to Find by Visibility

You can also conduct your search in terms of the visibility of the repository. In this case you can either use public or private. This will match issues and PRs that are either in a public or private repository, respectively.

#### Examples:

Using `is:public`. This will show a list of public repositories. Let's take an isntance where we want to search all public repositories owned by freeCodCamp. Then our search will be: `is:public` `org:freecodecamp`.
Using `is:private`. This query is meant to lists all private repositories under the given search query.
Conclusion
Even though we have covered many search queries here, you can always be creative to further narrow your search by combining multiple parameters together.

For additional resources and more search parameters, you can always refer to the GitHub Docs or make use of Advanced GitHub Search. These methods will always come in handy as they offer more filering options.

There are a ton of search parameters you can use to make your daily activity around GitHub easier. Hopefully this will help you use the platform more easily and effectively.

来源：https://www.freecodecamp.org/news/github-search-tips/
