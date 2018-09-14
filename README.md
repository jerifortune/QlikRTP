# QlikRTP
Relative Time Periods (RTP) is a Qlik script library which delivers time-related intelligence to your measures.  RTP will enable the following functionality:
- To-Date ranges such as YTD and QTD
- Relative time periods such as "Previous Year", "Previous Month", or “same period last year”
- Rolling time periods which are used for moving averages. (Ex. "Rolling 3 month average")
- Year-over-Year or Month-over-Month growth metrics

RTP adds all these time-related elements to your app and greatly simplifies your set expressions.  
RTP works with your existing time dimension.

# How it works
The RTP scripts will construct a number of tables that will enable RTP in any Qlik application.
- RTP_Type	- Contains the relative time period types (ex. CM, PM, CMPY, YTD, R3M, etc.)
- RTP_Map	- N:N mapping table that sits between a fact table and the Month Dim
	
Assumptions:
1. That the client has a date dimension that just contains months -- the "Month Dim".
2. The table should have a primary key (PK) that is unique for each month.
3. The PK does not have to be a sequential integer.  A six digit number (YYYYMM) or a string ('YYYY-MM') are acceptable.
4. The Month Dim has been exported to a QVD file.

Required Parameters:
- RTP.SCRIPTLIB 						          - lib path where RTP scripts and definitions file are located.
- RTP.SOURCELIB 						          - lib path where the Month Dim QVD resides.
- RTP.TARGETLIB 						          - lib path where RTP tables will be written.
- RTP.DEFS.FILENAME					          - Name of the RTP definitions file (csv).
- RTP.MONTH_DIM.TABLE_NAME 			      - the name of the table which contains the Month Dim.
- RTP.MONTH_DIM.PK_FIELD_NAME 	      - the name of the primary key field on that table.
- RTP.MONTH_DIM.MONTH_NUM_FIELD_NAME 	- The name of the field which contains the month number (1-12).
- RTP.DEBUG							              -1 (TRUE) or 0 (FALSE)
	
Note: LIB parameters must start with a 'lib://' and end with a slash.

# Installation
Installation assumptions:
1. That you have a data connection defined for a scripts library.  The app uses $(include).
2. That you have a data connection defined for reading and writing QVDs.

Steps:
1. Download the repository zip file
2. Unzip it in your Scripts folder (or a sub-folder of your Scripts folder).

# Usage Steps
1. Open the RTP_Generator app in Qlik Sense.  Open the data load script editor.
2. Set values for the above 8 variables and for the two parameters in the RTP.Replicate procedure call.
3. Run the Load.
4. Check your output data folder to see if the RTP QVDs have been created.

Subsequent Steps
4. Transform your fact table.  Create a YearMonth column that is derived from the event (date) you want to use.
5. Load your final app.  Use the transformed fact table and the 4 event-specific RTP tables.  The joins between the fact table and the RTP tables should happen automatically.

Note: RTP_Generator.qvf is not specifically required.  You can include these scripts in your scripts or apps and just call the RTP.Generate and RTP.Replicate procedures.

# Credit 
The original idea for this script library came from the following Qlik Community forum post:

[Simply create YTD, moving totals and comparisons versus Year Ago](https://community.qlik.com/docs/DOC-4821)
**Fabrice Aunez**
Sep 17, 2013

I was determined to make these concepts work with any month dimension.  I wanted a reusable, parameterized script that I could use at any of my clients.  After loading the client's month dim, I created a cross-product table -- joining the month dim to itself -- in order to generate the mapping table.  I eventually altered the scripts to generate relative time periods at the week and day level.  This repository only deals with months at this point in time.  I may apload the week and day scripts if I can genericize them sufficiently -- there are many nuances to working with weeks.
