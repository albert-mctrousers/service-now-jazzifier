# Service Now Userscript
## Make your Service Now look a bit more jazzy

## Introduction

This is a userscript which adds a number of user interface improvements and utilities to Service Now.

1. [Features](#features)
1. [Installation](#installation)
1. [Screenshots](#screenshots)
1. [Disclaimer](#disclaimer)

## Features

1. Task Lists Page ([screenshot](#tasks-list-page))
   1. Colour code `Task Type`
   1. Replace text used for `Priority` with coloured marker emojis (e.g P1 to P5 = 🔴, 🟠, 🟡, 🟢, 🔵)
   1. Replace text used for `Status` with coloured markers
      1. `On Hold` -> ✅
      1. `Work in Progress` -> WIP
      1. `Root Cause Analysis` -> RCA
      1. `Development` -> DEV
      1. `In Progress` -> 🔴
      1. `Specification` -> Spec
   1. Can be used to shorten customer names, if you support multiple customers.
   1. Checks to see if your username appears on the Tasks List page, and if it does, wraps it in a styled `span` so that you can easily see e.g. if you last updated a record.
   1. Colours table rows in yellow when you hover over them - useful to see which record your cursor is hovering over.
   1. Counts records on the page and adds the record count to the page title, including the current time, so you can see at a glance how many records are on the page and when the page last updated - see next point below for auto-refresh.
   1. Auto refreshes the page every 5 minutes.
      - I find this useful on the page I have bookmarked for unassigned calls.
      - If a new call comes through, I don't have to refresh the page to check - the updated page title shows the time the page last refreshed.
1. Individual Record Pages (e.g. Incidents, Changes, Tasks etc).
   1. Increases text side to 14px Arial in most cases (I find the small text hard to read).
   1. Updates page title to include record number, description and customer name.
      - I find this useful as this allows me to search for e.g. description or customer name in the browser's history manager.
   1. Assigns a different background colour to activity cards based on what type they are. I like this as I can quickly tell the difference between the following:
      - Customer notes / Additional comments: (background: `#cce6ff`).
      - Work notes: (background: `#ffe6cc`).
      - Image uploaded: (background: `#ffffcc`).
      - Attachment uploaded.: (background: `#ccffe6`).
      - [Screenshot](#colour-coded-activities)
   1. Adds the following buttons to the top of each record page:
      1. Email Customer (📧) ([screenshot](#email))
         - Creates pre-populated email.
         - The email's subject is in format of `RE: Record Number: Record Description`.
         - Therefore if the record's number is `INC123456` and its description is `My boomerang won't come back` then the email's subject line will be `RE: INC123456 - My boomerang won't come back`.
         - If your Service Now URL is e.g. `https://acme.service-now.com` then as long as you also CC your email update to `acme@service-now.com` and format the subject line as detailed above, then the update you send in your email, along with any attachments, will be automatically added to the record as a new customer note.
      1. Copy Record Reference (🆎)
         - If the record's number is `INC123456` and its description is `My boomerang won't come back` then `RE: INC123456 - My boomerang won't come back` will be copied to the clipboard.
      1. Copy Title (🅰)
         - If the record's description is `My boomerang won't come back` then `My boomerang won't come back` will be copied to the clipboard.
      1. Copy Full Record Number (💯)
         - If the record's number is `INC123456`, then `INC123456` will be copied to the clipboard.
      1. Copy Numeric Record Number (1️⃣)
         - If the record's number is `INC123456`, then `123456` will be copied to the clipboard.
      1. Copy URL (🔗)
         - Copies URL for current record to clipboard.
      1. Copy Update Note Text to Clipboard (📑) ([screenshot](#update-note))
         - Copies pre-populated update note to clipboard with today's date in the "Latest Action" section
         - This can be used to paste into e.g. the Customer notes field.
      1. Copy Resolution Note Text to Clipboard (✅) ([screenshot](#resolution-note))
         - Copies pre-populated resolution note to clipboard.
         - This can be used to paste into e.g. the Resolution notes field.
      1. Internal Link - Top Of Page (🔼)
      1. Internal Link - First Set of Tabs (🔵)
      1. Internal Link - Bottom Set of Tabs (🔽)
1. Knowledgebase Home Page
   - Hides entries which have no articles against them.
   - Where I work, we started to use Knowledgebase articles.
   - On the Knowledgebase homepage, hundreds of categories / cards were listed, one for each customer.
   - Only a handful of categories had articles published against them, so I set this up to hide categories with nothing published against them, making it simpler to see which categories do contain articles.

## Installation

1. Download the zip file containing the userscript
1. Import it into whichever flavour of userscript monkey manager you prefer, such as:
   1. [Tampermonkey Chrome]
   1. [Tampermonkey Firefox]
   1. [Tampermonkey Edge]
   1. [Violentmonkey Chrome]
   1. [Violentmonkey Firefox]
   1. [Violentmonkey Edge]
   1. [Greasemonkey Firefox]
1. Update the values of these variables in the script as required:

		// Details for logged in Service Now user

		var Person = "Albert McTrousers";
		var TelNo = "Tel: 01234 123456";
		var JobTitle = "Helpdesk Monkey";
		var Company = "ACME Corporation";
		var MyUserName = "albert.mctrousers";
		var cc_email = "acme@service-now.com";

1. For the Task List updates to work (such as auto-refresh and colour-coding task type), you need to do the following:
   1. Make sure to include the Task Type column as shown in the [screenshots](#setup) screenshots below.
   1. Use the following URLs for:
      1. Unassigned records (Incidents / Changes / Tasks etc)
         1. https://acme.service-now.com/nav_to.do?uri=%2Ftask_list.do%3Fsysparm_query%3Dactive%253Dtrue%255Eassignment_groupDYNAMICd6435e965f510100a9ad2572f2b47744%255EstateNOT%2520IN0%252C-4%252C6%255Eassigned_toISEMPTY%26sysparm_first_row%3D1%26sysparm_view%3D
      1. Records assigned to yourself:
         1. https://acme.service-now.com/nav_to.do?uri=%2Ftask_list.do%3Fsysparm_query%3Dactive%253Dtrue%255Eassigned_toDYNAMIC90d1921e5f510100a9ad2572f2b477fe%255EstateNOT%2520IN0%252C-4%252C6%255Eu_state_label!%253DFulfilled%255Eu_state_label!%253DClosed%26sysparm_first_row%3D1%26sysparm_view%3D
   1. These URLs are useful because you can see all records on one page, instead of having to go to one place to view Incidents, another to see Changes, another to see Request Items etc.
   1. The Auto Refresh code looks for specific values in the above URLs to ensure that only those 2 pages auto refresh.


## Screenshots

### Setup

Include Task type on Task List page

![Gear Icon](https://jimpix.co.uk/images/service-now/2022-04-23-task-list-gear-icon.png)
![Task Type selected](https://jimpix.co.uk/images/service-now/2022-04-23-task-type-selected.png)

### Tasks List Page

Tasks List page showing colour coded Task Types, Priority markers, State markers and my username styled in green in the Last Updated column

![Tasks List Page](https://jimpix.co.uk/images/service-now/2022-04-23-task-list.png)

### Individual Record Pages

#### Extra Buttons

![Extra Buttons](https://jimpix.co.uk/images/service-now/2022-04-23-extra-buttons.png)

#### Auto Text

##### Update Note

![Update Note](https://jimpix.co.uk/images/service-now/2022-04-23-auto-text-note.png)

##### Resolution Note

![Resolution Note](https://jimpix.co.uk/images/service-now/2022-04-23-auto-text-resolution.png)

#### Email

![Email](https://jimpix.co.uk/images/service-now/2022-04-23-email.png)

#### Colour Coded Activities

![Colour Coded Activities](https://jimpix.co.uk/images/service-now/2022-04-23-colour-coded-activities.png)

## Disclaimer

I am not a "real" programmer, and so the javascript in this userscript probably contains lots of glaring mistakes, examples of bad practice, poor design etc. etc.

Over the years I've learnt a bit about how to use javascript to change the various web-based incident management tools I've used at work, the latest being Service Now.

As of April 23rd 2022, I decided to tidy up the code and put it on Github in case other Service Now users like it and in case it revolutionises their lives, and leads to the ultimate thrill of me receiving praise from random strangers (which I think is the scourge of the internet and is why I think social media is a grim cess pit and a nest of vipers).

## License

MIT

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [Tampermonkey Chrome]: <https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=en>
   [Tampermonkey Firefox]: <https://addons.mozilla.org/en-GB/firefox/addon/tampermonkey/>
   [Tampermonkey Edge]: <https://microsoftedge.microsoft.com/addons/detail/tampermonkey/iikmkjmpaadaobahmlepeloendndfphd>
   [Violentmonkey Chrome]: <https://chrome.google.com/webstore/detail/violentmonkey/jinjaccalgkegednnccohejagnlnfdag?hl=en>
   [Violentmonkey Firefox]: <https://addons.mozilla.org/en-GB/firefox/addon/violentmonkey/>
   [Violentmonkey Edge]: <https://microsoftedge.microsoft.com/addons/detail/violentmonkey/>
   [Greasemonkey Firefox]: <https://addons.mozilla.org/en-GB/firefox/addon/greasemonkey/>
