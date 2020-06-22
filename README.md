[![Platform](https://img.shields.io/badge/Episerver-%2011.0.+-orange.svg?style=flat)](http://world.episerver.com/cms/)

# SEOBOOST for EPiServer

## Description
This package facilitates developers and editors to improve the SEO ranking of the website by utilizing helper methods & features provided.

## Features
The package provides the following helper methods & features:
* Canonical Link 
* Alternate Links (hreflang attributes)
* Breadcrumbs items
* robots.txt

## How to install
Install NuGet package from Episerver NuGet Feed:

    Install-Package SeoBoost

## How to use

Include the follow **@ using SeoBoost.Helper** at top of the Mater page.
     
### Canonical link
Use the following extension **@Html.GetCanonicalLink()** within **<head></head>** section.
     
### Alternate links (hreflang attributes)
Use the following extension **@Html.GetAlternateLinks()** within **<head></head>** section.

### Breadcrumbs items
Use the following extension **@Html.GetBreadcrumbItemList()** where required.

Example:
                    
    <ul>
        @{ var breadCrumbList = Html.GetBreadcrumbItemList(ContentReference.StartPage); }

        @foreach (var breadCrumbItem in breadCrumbList)
        {
            if (breadCrumbItem.Position == 1)
            {
                <li><a href="@Url.ContentUrl(breadCrumbItem.PageData.ContentLink)">Home</a></li>
            }
            else if (breadCrumbItem.PageData.HasTemplate() && !breadCrumbItem.PageData.ContentLink.CompareToIgnoreWorkID(CURRENTPAGE.ContentLink))
            {
                <li><a href="@Url.ContentUrl(breadCrumbItem.PageData.ContentLink)">@breadCrumbItem.PageData.PageName</a></li>
            }
            else if (breadCrumbItem.Selected)
            {
                <li>@breadCrumbItem.PageData.PageName</li>
            }
            else
            {
                <li>@breadCrumbItem.PageData.PageName</li>
            }

            if (breadCrumbItem.Position != breadCrumbList.Count)
            {
                <li class="spacer">/</li>
            }
        }
    </ul>

### robots.txt

The idea behind this feature is simple, provide editors with the flexibility to change robots.txt file on the go. 

The Robots.txt page (backed by SBRobotsTxt PageType) will automatically be created for the site under Start Page. 

![robots.txt PageType](assets/docsimages/image001.png)

The physical rebots.txt file content (if any) will be replaced with the CMS robobts.txt page content. The fallback behaviour of /robots.txt URL is the content of physical rebots.txt file (if any) otherwise the default 404 error page will be shown.

**IMPORTANT**: If there is a physical robot.txt exist in the site root, always purged the CDN cache after the deploy or site restart. It is a recommendation to delete physical robots.txt file from the site root to ensure editable robot.txt content loads without a problem.


There are restrictions in place to move (change parent) or delete the Rebots.txt page. 

![robots.txt restrictions](assets/docsimages/image003.png)

#### How to enable robots.txt feature
The editable robot.txt feature is disabled by default and can be enabled by setting "Disable Robots.txt feature" property to "true" in the Robotx.Txt page. 

![Robots.txt Page properties](assets/docsimages/image002.png)

### Additional helper methods

There are some helper methods in the package to get external URLs of the page. The developer can use these methods for their implementations 

usage 

      var urlHelper = ServiceLocator.Current.GetInstance<SeoBoost.Business.Url.IUrlService>();


There are three methods available to get external URL for the content 

       string GetExternalUrl(ContentReference contentReference);
       string GetExternalFriendlyUrl(ContentReference contentReference, string culture);
       string GetExternalFriendlyUrl(ContentReference contentReference);

## Changelog
### Changes in version 1.5.0
1. Added functionality to support editable robots.txt in CMS 
