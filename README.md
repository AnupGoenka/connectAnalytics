
<!-- README.md is generated from README.Rmd. Please edit that file -->

# connectAnalytics

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
<!-- badges: end -->

The goal of connectAnalytics is to provide an out of the box shiny app
that will allow for RStudio Connect users to see analytics information
about their deployed applications and documents.

## Installation

And the development version from [GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("tbradley1013/connectAnalytics")
```

## Usage

### Prerequisites

Before you can get started using the `connectAnalytics` dashboard for
your organization, there are a few things that you need to do:

1.  Have access to an [RStudio
    Connect](https://rstudio.com/products/connect/) instance as either a
    [Publisher or
    Administrator](http://pwdrstudio.water.gov/rsconnect/__docs__/admin/user-management.html#user-roles)

2.  Configure the following environment variables:
    
      - `CONNECT_SERVER` - This variable should be the url of your
        connect server that you use to reach the base content page.
        **NOTE:** If your server’s port is masked then you will need to
        add the actual port to the server URL
      - `CONNECT_API_KEY` - An API key generated from the Connect
        system. You can see how to generate an API key
        [here](https://docs.rstudio.com/connect/user/api-keys.html)

3.  Install the `connectAnalytics` package as shown above

### Run the app

To run the application, you can simply run the `connectAnalytics`
wrapper function:

``` r
library(connectAnalytics)
connectAnalytics()
```

If you run it wittout specifying any arguments than it will
automatically look for the two environment variables discussed above to
connect to your RStudio Connect server. In addition, once the
application connects to your server and loads you will see a modal pop
up asking you to specify your connect username. By default, if a user is
not specified than it looks for the `session$user` variable, if
`session$user` is NULL (which it will be if running locally) than you
will be asked for your username via the modal. It is designed like this
because once deployed to your RStudio Connect system than whichever user
access the application will see their usage information. See the
deployment section below for more information.

There are several arguments that can be specified when running this app
that can help the user customize the app to themselves or their
organization:

  - `host` - If specified would override the `CONNECT_SERVER`
    environment variable
  - `api_key` - If specified would override the `CONNECT_API_KEY`
    environment variable
  - `user` - The RStudio Connect User name to run the application as
  - `switch_user` - `TRUE` or `FALSE`; Allows the user to specify
    different user names within the app to see usage information for
    other users. This feature could be userful for managers or
    supervisors who are not admins on the server but would like to be
    able to see how the content the people under them have deployed is
    being used.
  - `favicon` - The path to a favicon icon to be used by your app. If
    `NULL` then the `golem` favicon is used (this may be changed in the
    future)
  - `title` - The title for your dashboard header. Defaults to
    `connectAnalytics`
  - `window_title` - The title to be displayed in the browser tab for
    your app. Defaults to the same value as the `title` argument

## Deployment

If you want to deploy the application to your connect server so that
publishers and admins can easily track their application usage data, you
simply need to create an `app.R` file that looks like:

``` r
library(connectAnalytics)
library(shiny)

# Leave a commented out line of `shinyApp()` so that RStudio recognizes the script
# as a shiny application!
# shinyApp()

connectAnalytics()
```

This `app.R` file can then be published to your connect server in any
way you would normally publish an application. It is recommneded that
this application be deployed by an administrator with an API key for the
admin account. This will allow all publishers and admins to easily view
their usage data. If it is published by a user with `publiser`
permissions than it will only be able to be used by that user due to API
key restrictions. By default, API keys generated by an admin account are
able to view usage for all users, while an API key associated with a
publisher account can only see the data associated with their account.
The application will parse through the content and only show publishers
their data. If the user accessing the application is an admin, then an
additional “Admin” tab will be created showing data for all users on the
account.
