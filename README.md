# Machine Translation from AWS Translate

**aws.translate** is a package for [AWS Translate](https://aws.amazon.com/translate/), an Amazon Web Services machine translation web service.

To use the package, you will need an AWS account and to enter your credentials into R. Your keypair can be generated on the [IAM Management Console](https://aws.amazon.com/) under the heading *Access Keys*. Note that you only have access to your secret key once. After it is generated, you need to save it in a secure location. New keypairs can be generated at any time if yours has been lost, stolen, or forgotten. The [**aws.iam** package](https://github.com/cloudyr/aws.iam) profiles tools for working with IAM, including creating roles, users, groups, and credentials programmatically; it is not needed to *use* IAM credentials.

By default, all **cloudyr** packages for AWS services allow the use of credentials specified in a number of ways, beginning with:

 1. User-supplied values passed directly to functions.
 2. Environment variables, which can alternatively be set on the command line prior to starting R or via an `Renviron.site` or `.Renviron` file, which are used to set environment variables in R during startup (see `? Startup`). Or they can be set within R:
 
    ```R
    Sys.setenv("AWS_ACCESS_KEY_ID" = "mykey",
               "AWS_SECRET_ACCESS_KEY" = "mysecretkey",
               "AWS_DEFAULT_REGION" = "us-east-1",
               "AWS_SESSION_TOKEN" = "mytoken")
    ```
 3. If R is running an EC2 instance, the role profile credentials provided by [**aws.ec2metadata**](https://cran.r-project.org/package=aws.ec2metadata).
 4. Profiles saved in a `/.aws/credentials` "dot file" in the current working directory. The `"default" profile is assumed if none is specified.
 5. [A centralized `~/.aws/credentials` file](https://blogs.aws.amazon.com/security/post/Tx3D6U6WSFGOK2H/A-New-and-Standardized-Way-to-Manage-Credentials-in-the-AWS-SDKs), containing credentials for multiple accounts. The `"default" profile is assumed if none is specified.

Profiles stored locally or in a centralized location (e.g., `~/.aws/credentials`) can also be invoked via:

```R
# use your 'default' account credentials
aws.signature::use_credentials()

# use an alternative credentials profile
aws.signature::use_credentials(profile = "bob")
```

Temporary session tokens are stored in environment variable `AWS_SESSION_TOKEN` (and will be stored there by the `use_credentials()` function). The [aws.iam package](https://github.com/cloudyr/aws.iam/) provides an R interface to IAM roles and the generation of temporary session tokens via the security token service (STS).


## Code Examples

The basic use of the package is super simple and revolves around the `translate()` function, which takes a character string as input:


```r
library("aws.translate")

# translate some text from English
translate("Bonjour le monde!", from = "fr", to = "en")
```

```
## [1] "Hello, world!"
## attr(,"SourceLanguageCode")
## [1] "fr"
## attr(,"TargetLanguageCode")
## [1] "en"
```

```r
# translate some text to English
translate("Hola mundo!", from = "auto", to = "en")
```

```
## [1] "Hello world!"
## attr(,"SourceLanguageCode")
## [1] "es"
## attr(,"TargetLanguageCode")
## [1] "en"
```

## Installation

[![CRAN](https://www.r-pkg.org/badges/version/aws.translate)](https://cran.r-project.org/package=aws.translate)
![Downloads](https://cranlogs.r-pkg.org/badges/aws.translate)
[![Build Status](https://travis-ci.org/cloudyr/aws.translate.png?branch=master)](https://travis-ci.org/cloudyr/aws.translate)
[![codecov.io](https://codecov.io/github/cloudyr/aws.translate/coverage.svg?branch=master)](https://codecov.io/github/cloudyr/aws.translate?branch=master)

This package is not yet on CRAN. To install the latest development version you can install from the cloudyr drat repository:

```R
# latest stable version
install.packages("aws.translate", repos = c(getOption("repos"), "http://cloudyr.github.io/drat"))
```

Or, to pull a potentially unstable version directly from GitHub:

```R
if(!require("remotes")){
    install.packages("remotes")
}
remotes::install_github("cloudyr/aws.translate")
```

---
[![cloudyr project logo](http://i.imgur.com/JHS98Y7.png)](https://github.com/cloudyr)
