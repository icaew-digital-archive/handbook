# Browsertrix-crawler custom behaviors

This document gives some brief information around the creation of custom browsertrix web crawler behaviors.

The browsertrix web crawler supports automatically running customized in-browser behaviors. The behaviors auto-play videos (when possible), and auto-fetch content that is not loaded by default, and also run custom behaviors on certain sites.

- Before writing any custom behaviors, as part of the pre-crawl work an updated list of templates currently in use by ICAEW.com should be requested from the web team.
- These are to be recorded in the SharePoint document [Sitecore-templates-for-testing.docx](https://icaew.sharepoint.com/:w:/r/sites/digitalarchive/_layouts/15/Doc.aspx?sourcedoc=%7B6944968F-7BA4-4675-9B26-244DD1030D83%7D&file=Sitecore-templates-for-testing.docx&action=default&mobileredirect=true). The URLs listed form the basis for the testing of the capture and the playback of the web archiving software. Currently there are about 6 JavaScript elements that need custom behaviors written to ensure the highest fidelity capture. These are also recorded in the above document.
- A crawl of the pages listed should be carried out to test the crawl and the playback of the capture.
- Once these problematic elements are identified, custom behaviours need to be written. There is a tutorial found at the [browsertrix-behaviors](https://github.com/webrecorder/browsertrix-behaviors) GitHub page.

    For testing, I've found the following command to be useful:

    `docker run -p 9037:9037 -v $PWD/crawls:/crawls -v $PWD/custom-behaviors/:/custom-behaviors/ webrecorder/browsertrix-crawler crawl --url [URLS] --customBehaviors /custom-behaviors/ --screencastPort 9037 --scopeType page --behaviors siteSpecific --generateWACZ`

    The ongoing crawl can be viewed in a web browser at **localhost:9037** so the custom behaviors can be confirmed to be working. The custom behaviors are JavaScript functions in a .js file located in a "custom-behaviors" folder in the present working directory, i.e. from where the docker command is run. `--url` can accept multiple URLs, which will be the URLs to be tested. The `--scopeType` is set to "page" so only the pages given are crawled. `--behaviors` is set to "siteSpecific" so there is no confusion with the default behavours when testing. `--generateWACZ` is specified so the resulting crawl can easily be tested.

- Once the crawl has completed the resulting WACZ file should be tested via ReplayWeb.page

## Appendix

The behavior script is available on the [icaew-digital-archive](https://github.com/icaew-digital-archive) GitHub page, but it is also here for reference:

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
