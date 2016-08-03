# UMP-Events-System
A custom system for making events and display in the UMT Ponderosa Template in the Cascade CMS specifically for the UM Productions Site in 2014. The following was originally documented with Bitbucket markdown intended for internal use and is provided here for reference.

Updated 8/2/2016 @g30ff4y 

[TOC]

##Overview
Similar to the [Roster System](https://bitbucket.org/sfdadmin/sfd-documentation/wiki/SFD%20Roster%20System), the Events System is a system designed specifically for UM Productions to display various featured events happening on and off campus. The system uses two separate indexes to display four featured events (folder order) on the home page in place of the standard Ponderosa Tiles/Stories, in addition to an Events page that displays all events. Individual events are kept in a separate folder for easier maintenance and site navigation. 

****Please note that the Event System requires quite a bit of setup, and while it would be possible to implement and adapt on another site, it is extremely ill-advised as maintenance could become problematic.****

Due to high level of customization and complexity, this documentation will describe the components of the system and how to manage events, as opposed to initial implementation and setup (which is similar to the [Roster System](https://bitbucket.org/sfdadmin/sfd-documentation/wiki/SFD%20Roster%20System)).

##Managing Events

###Adding Events

1. Under the New menu in the blue bar, select **Event**.
2. Change the System Name to the name of the event you are entering data for. This should be all lowercase, separate words if necessary with '-'.
3. **Display Name and Title:** Event Title.
4. Each event will have these fields to enter:

     1.  **Photo:** Add square image here, should be the same dimensions as other images like 400x400 pixels.
     2.  **Event Name:** Name of event. Be careful with superfluous text; keep the event name limited ideally to 20-25 characters including spaces.  
     3.  **Venue:**  Pick a pre-set venue from the selector. You may also enter a custom venue name.
     4.  **Show Date and Time:** Enter the show date and time from the date and time picker.
     5.  **Door Time:** Enter the door time. You will still need to pick the correct date here.
     6.  **Tickets**

          *  **Ticket Price(s):** Enter ticket prices here, may include letters, number and the dollar sign special character.
          *  **Advanced:**
               *  **Custom Ticket Information:** Some events may have special details about how to purchase tickets. The custom ticket information will need to be entered in this WYSIWYG Editor. The basic UM Productions ticket information will automatically be displayed (see the Event list format above for the structure). 

     7.  **Event Content:** Enter any additional information about the show, performer, etc. May also embed videos. Avoid using additional images to keep formatting clean.

###Deleting Events

After each event occurs the event must be manually deleted from Cascade. 

1.  Go to the event that needs to be deleted in the event-details folder and select delete from the flyout menu to the right of the file name or select Delete from the More tab. 
2.  Make sure Un-publish content and both Production and Staging destinations are checked.
3.  Click submit. 

###Events Folder Contents

####Events Default Page

The default page in the Events folder is the place where all events are indexed. Events are displayed in their sort order in the folder, this allows for events to be added at any time and be moved to the top of the list for time sensitivity.

Event list format:


```
#!html

<div class="row-fluid event-row">
     <div class="row event-heading">
          <h2>$name</h2> 
     </div>   
     <div class="row event-info">  
          <div class="col-sm-4">
               <img class="event-image" src="[system-asset]${photo}[/system-asset]" alt="$name"/>
          </div>  
          <div class="col-sm-8">
               <p class="lead">
                    <span class="venue-name"><strong>Venue: </strong>venue</span>
                    <br />
                    <span class="show-time">
                         <strong>Date: </strong>Show Date
                         <br />
                         <strong>Show: </strong> Show Time <strong>Doors: </strong> Door Time 
                    </span>
                    <br />
                    <span class="ticket-info">
                         <strong>Ticket Price: </strong> ticketPrice 
                         <br />
                         <span class="lead"> custTicketInfo</span>
                       <!-- or -->
                         <p class="lead">Tickets are available at all GrizTix locations including Worden&#39;s Market, Southgate Mall, The Source in the University Center, MSO Hub, and the Adams Center Box Office. Also available by calling 243-4051, toll-free at 1-888-MONTANA or online at <a href="http://www.umt.edu/griztix/">Griztix.com</a>.</p>   
                    </span>
                </p>
          </div>        
     </div>
     <div class="row-fluid">
          Event content
     </div>
     <hr />
</div>

```


####Event Details

Event details are the individual events which are a Page type asset in Cascade. There is an asset factory that allows users to create a new Event from the New menu in the Blue Cascade Navigation Bar. Creating a new event in this manner will automatically place the event in the correct location in the Event Details folder.

####Event Images 

To make sure images for the events are contained and easier to manage, a folder exists in the events area to keep these images separate from the rest of the images for the site. Images should be edited to be the same dimensions 300x300 or 400x400 works well.

##Components

###Events Control

Located in the _cms folder, the Events Control folder contains the elements that control various parts of the system. These elements include scripts and index blocks.

####Scripts

#####Event Data

This script sets up each individual event. Data is formatted here slightly generically so it can be used throughout the site. The event-data script works with the event-details Data Definition and is attached to the DEFAULT region of the Event page via the Event basepage in the _cms > basepages folder.

#####Event Index

This script determines how the events will be rendered on the Events default page, which is ultimately controlled by the Event Index Block.

#####Event Tile Index

This script determines how events will render on the home page and is ultimately controlled by the Event Tile Index

####Index Blocks

There are two index blocks that are configured to be nearly identical; however, they need to be separated to limit the number events (to four) to be displayed on the home page for proper formatting.

#####Event Index

This index block selects the event details folder and sorts the events in their sort order in the folder. The block is connected to the DEFAULT_AFTER region on the event default page with the event-index script format.

#####Event Tile Index

This index block is limited to four events.  It selects the first four events (sort order of folder) and displays them on the homepage. The block is attached to the SITE_TILES region on the homepage with the event-tile-index script format.

###Administration

####Configuration Set

The Event Page configuration set is a copy of the Page config for the UM Productions site. The event-data script is attached to the DEFAULT region as the format. No need for an index block.

####Data Definition

This is the XML document and GUI for the event page for users to enter data. The event-details Data Definition essentially contains the variables that are used in the script formats to render user entered data about the given events.

####Content Type

The Event Page content type connects the Event Page configuration set and the event-details Data Definition, which is then connected to the event basepage.
