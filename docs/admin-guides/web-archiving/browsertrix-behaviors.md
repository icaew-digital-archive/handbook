# Browsertrix-crawler custom behaviors

## Overview

> **Purpose:** This document provides information about creating and implementing custom behaviors for the Browsertrix web crawler. These behaviors are used to enhance the crawling process by automatically handling dynamic content, auto-playing videos, and implementing site-specific interactions.

## Prerequisites

### Template Documentation
- Before writing custom behaviors, request an updated list of templates from the web team
- Record these templates in the SharePoint document: [Sitecore-templates-for-testing.docx](https://icaew.sharepoint.com/:w:/r/sites/digitalarchive/_layouts/15/Doc.aspx?sourcedoc=%7B6944968F-7BA4-4675-9B26-244DD1030D83%7D&file=Sitecore-templates-for-testing.docx&action=default&mobileredirect=true)
- The URLs listed in this document form the basis for testing capture and playback functionality

### Testing Requirements
- Currently, there are approximately 6 JavaScript elements requiring custom behaviors
- These elements are documented in the Sitecore-templates-for-testing.docx
- A test crawl of the listed pages should be performed to verify capture and playback

## Development Process

### 1. Learning Resources
- Review the tutorial at [browsertrix-behaviors](https://github.com/webrecorder/browsertrix-behaviors) GitHub page
- Understand the behavior development framework and requirements

### 2. Testing Environment Setup

**Command for testing custom behaviors:**

```bash
docker run -p 9037:9037 \
    -v $PWD/crawls:/crawls \
    -v $PWD/custom-behaviors/:/custom-behaviors/ \
    webrecorder/browsertrix-crawler crawl \
    --url [URLS] \
    --customBehaviors /custom-behaviors/ \
    --screencastPort 9037 \
    --scopeType page \
    --behaviors siteSpecific \
    --generateWACZ
```

**Key parameters:**
- `--url`: Accepts multiple URLs for testing
- `--scopeType`: Set to "page" to crawl only specified pages
- `--behaviors`: Set to "siteSpecific" to avoid confusion with default behaviors
- `--generateWACZ`: Creates a WACZ file for easy testing

### 3. Monitoring and Verification
- Monitor the crawl at **localhost:9037**
- Verify custom behaviors are functioning as expected
- Test the resulting WACZ file using ReplayWeb.page

## Implementation Details

### File Structure
- Custom behaviors are JavaScript functions in a .js file
- File must be located in a "custom-behaviors" folder
- Folder should be in the same directory as the docker command execution

### Behavior Script

> **Note:** The behavior script is available on the [icaew-digital-archive](https://github.com/icaew-digital-archive) GitHub page. The ICAEW behaviors file can be found at [icaew-com-behaviors.js](https://github.com/icaew-digital-archive/digital-archiving-scripts/blob/main/browsertrix-crawler%20files%20and%20scripts/icaew-com-behaviors.js).


## Appendix

### Complete Behavior Script
```javascript
class ICAEWBehaviors {
    static init() {
        return {
            state: {},
        };
    }

    static get id() {
        return "ICAEWBehaviors";
    }

    static isMatch() {
        const pathRegex = /^(https?:\/\/)?([\w-]+\.)*icaew\.com(\/.*)?$/;
        return window.location.href.match(pathRegex);
    }

    async *run(ctx) {
        ctx.log("In ICAEW Behavior!");

        // Change navbar to pink - FOR TESTING
        // Select all elements matching the specified selector
        //    var navLinks = document.querySelectorAll('#u-nav .u-nav--links > li > a');

        // Iterate over each element and set its color to pink
        //    navLinks.forEach(function(link) {
        //        link.style.color = 'pink';
        //    });

        // Function to click a cookie consent button
        function clickCookieConsentButton() {
            const buttonId = "CybotCookiebotDialogBodyLevelButtonLevelOptinAllowAll";
            const element = document.getElementById(buttonId);

            if (element) {
                element.click();
            } else {
                console.log(`Element with ID '${buttonId}' not found`);
            }
        }

        // Trigger the function
        clickCookieConsentButton();

        // Function to click buttons within a dynamic filter with a delay
        function clickButtonsInFilter(selector, delay = 500) {
            const parentElement = document.querySelector(selector);

            if (parentElement) {
                const buttons = parentElement.querySelectorAll("button");

                function clickButtonAtIndex(index) {
                    if (index < buttons.length) {
                        buttons[index].click();
                        setTimeout(() => clickButtonAtIndex(index + 1), delay);
                    }
                }

                clickButtonAtIndex(0);
            } else {
                console.log(`Parent element with selector '${selector}' not found`);
            }
        }

        // Trigger the function for "dynamic filter" buttons
        clickButtonsInFilter(
            "div.c-filter.c-filter--dynamic > div.c-filter__filters"
        );

        // Function to click an element with a delay
        function clickElementWithDelay(selector, delay = 500) {
            const element = document.querySelector(selector);

            if (element) {
                element.click();
                setTimeout(() => clickElementWithDelay(selector, delay), delay);
            } else {
                console.log(`Element with selector '${selector}' not found`);
            }
        }

        // Start clicking the "pagination control" with a delay
        clickElementWithDelay("div.c-navigation-pagination > nav > a.page.next");

        // Function to click an element repeatedly until it is hidden
        function clickUntilHidden(selector, delay = 500) {
            const interval = setInterval(() => {
                const element = document.querySelector(selector);

                if (element && getComputedStyle(element).display !== "none") {
                    element.click();
                } else {
                    console.log(
                        `Element with selector '${selector}' not found or hidden`
                    );
                    clearInterval(interval);
                }
            }, delay);
        }

        // Call the function for the "more-link"
        clickUntilHidden("div.more-link > a");

        yield ctx.Lib.getState(ctx, "icaew-stat");
    }
}
```
