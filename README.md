# Live Project

## Overview
My last major project in The Tech Academy Software Development bootcamp involved building a content management service (CMS) website for a local theatre/acting company. The project is built using an ASP/.NET MVC and Entity Framework. The goal was to create a website that allowed easy management of the website display, login capabilities, and past performance and performer wikis. The website also allows for the theatre to rent out space or equipment. This includes rental requests, rental and equipment rooms, and post-rental surveys.

The 2 weeks of my Live Project (referred to as "Theatre Project" onward) involved working in the rental area portion of the website, with opportunities to be involved in both back-end and front-end stories. I first created a Rental History entity model and CRUD pages, My subsequent work featured client-specific styling and functionality for the Index and Create pages. 

## Story 1: Set-up
I first created an entity model for the Rental History class in order to include the information in the Theatre Project database. I subsequently used Entity Framework to get the CRUD pages started. 

## Story 2: Sytling Create and Edit pages
The second story involved styling the Create and Edit pages using a client-specific colour palette and layout. Based on the limited instructions, I was able to re-create this: ![createrentalgif](CreateRental.gif).

### Focus feature 1:
One of the main styling points was (1) making sure "Rental" and the "Damaged?" checkbox were on the same line, and (2) lining up the "Back to List" and "Create " buttons in the bottom center.  I took advantage of flex boxes to achieve this, but it took a bit to get the buttons to the desired position. 

### Focus feature 2:
The "Damaged?" checkbox determined if "Damages Incurred" or "Notes" would be displayed below. I utilized a JS function based on my earier bootcamp tutorials:

```javascript

$(document).ready(function () {
    // When checkbox is checked, show damages incurred section instead
    $('#damaged-checkbox').change(function () {
        if ($(this).is(':checked')) {
            $('#damages-incurred').show();
            $('#notes').hide();
        }
        else {
            $('#damages-incurred').hide();
            $('#notes').show();
        }                            
    });
});

// Function for Rental History Index Page sorting
function submitForm() {
    document.getElementById("rental-index--sorting").submit();
}

```
## Story 3, 4, 5: Index page
The rest of the stories were extra practice; I had already completed the requirements for the Live Project at this point. The stories focused on styling and adding extra features to the index page. 

### Story 3
This story focused on showing the Rental Histories in tabular form. It also included adding a green check mark or red x icon depending on the rental's state (damaged vs undamaged). The story further required the vertical ellipses that when hovered over, will show the Edit, Details, and Delete options.

### Story 4
Story 4 focused on the option to sort the rentals: No Sort, Damaged Rentals, Undamaged Rentals, A-Z, and Z-A. This stretched me, and required a lot of self-study and research. I was able to add the sorting options in the controller using the code below: 

``` cs
 public ViewResult Index(string sortOrder)
        {
            // Set up sorting links
            ViewBag.NameSortParm = String.IsNullOrEmpty(sortOrder) ? "name_desc" : "";
            ViewBag.DateSortParm = sortOrder == "RentalHistoryId" ? "rentalhistoryid_desc" : "RentalHistoryId";
            
            //Selecting all rental histories from the database
            var rentals = db.RentalHistories.AsQueryable();
                          
            // Apply sort options
            switch (sortOrder)
            {
                case "Damaged":
                    rentals = rentals.Where(r => r.RentalDamaged).OrderByDescending(r => r.RentalHistoryId);
                    break;
                case "Undamaged":
                    rentals = rentals.Where(r => !r.RentalDamaged).OrderByDescending(r => r.RentalHistoryId);
                    break;
                case "A-Z":
                    rentals = rentals.OrderBy(r => r.Rental).ThenByDescending(r => r.RentalHistoryId);
                    break;
                case "Z-A":
                    rentals = rentals.OrderByDescending(r => r.Rental).ThenByDescending(r => r.RentalHistoryId);
                    break;
                default:
                    rentals = rentals.OrderByDescending(r => r.RentalHistoryId);
                    break;
            }
            return View(rentals.ToList());
        }

```

### Story 5
Story 5's goal was to create the accordion effect to the index page, so that the Notes/Damages Incurred portion would remain hidden untl clicked on.

## Conclusion
As seen above, the Live Project required to to utilize different front-end and back-end skills to achieve the customer's desired results. The addition of daily stand-ups and regular meetings with the Project Director mimicked standard project management practices.
