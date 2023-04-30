# PMT-Position-Tracker
Fresh Complete Rewrite of the old PMT Lotto Tracker to track ALL trading positions - long/short stock, spreads, box spreads, etc, using the transaction API to have a penny accurate figure. 

# Features
Tracks:     

* Long and Short Stock - with warnings for trailing losses or outright losses. (Default: trailing losses.)        
* Long and Short Option Singles         
* Long and Short Option Spreads       
* Long and Short Box Spreads - must specify ticker (defaults: SPX and iron condor/condor)       
* User configurable for all these values.       

Portfolio Warnings:    
   
* Warns on user-configurable % OTM breaches for options and % gains or losses for long/short stock (on a cost basis ("PL/Open") or a trailing-basis ("Watermark"), or on a daily-basis ("PL/Day")    
* Option Spreads warn both % OTM for the single legs and % gains or losses for the entire spread.    
* PNR warnings for entire position including delta hedging with any long or short stock.   
* Earnings Release Warnings and ER Database! 

PNL Reporting:    
    
* Reports Daily, Weekly, Monthly, and YTD with raw values, log-return values, and XIRR values, accounting for deposits or withdrawals.        
* Run PNL reports by an "effort" trade-date basis (when short positions are opened) and by a realized gains method (when short AND long options/short closed)
* By default, PNL "effort" basis ignores "investments" purchases, on a user configurable list. 
* Run detailed PNL reports that breaks down returns by "strategy" and its components. (short & long options together (excluding box spreads), short options, long options, option spread trades, stock trades, long stock trades, short stock trades, box spreads, fixed income.) 
* sumd, sumw, summ, sumy can be ran on an "effort" basis or "realized" basis. 
* Effort = trade opened max PNL for short options/short stock, max loss for long options/long stock(user configurable % values) + closing trades.     
* Realized basis = only accounting for gains/loss when the trade is closed/expired (should be equal to your tax reported gains.)    

Transaction API:

* Queries transaction API for historical data (1 day delay).
* Listens into account activity stream for most up to date data. 
* Only queries orders ONCE at startup - no more lag or API issues running other programs that use the API. 
* Track ALL fees to the penny. 

Saved State is now CSV and JSON - No More Database!

* The old database was problematic. It had conflicts sharing in dropbox if you had two different computers running it, or accidentally ran two instances. Deleting the database and recreating it did not have the same results, it had really weird results. 
* All saved state is now human readable CSV, TSV (text seperated) and JSON files - easily merged, easily created, easily verifiable. 

Waste Management Services:

* No more lag - no more lag waiting for earnings or having to parse your up to date orders! 


# Architecture

The old code was a huge mess, one giant file that lead to lots of merge conflicts, hard to read code, hard to understand code, etc. The new approach is going a vastly more object oriented approach where we have many libraries with small objects that do their job. 

Then it will be a requirement to use unittest - https://docs.python.org/3/library/unittest.html for every library to make sure it's working, etc. 

#Code Style

Code is going to be CamelCase - yes I know it breaks python convention but this is how I've written all my code - C++, C#, Java, etc. 
 
Private member variables: self.temporaryValue    
Private member functions: self.intermediateCalculation(self)   
Public member variables: self.ProfitOrLoss    
Public member functions: self.CalculatePNR(self)  (acronyms are upper cased as PNR = Point of No Return, unless it looks really bad)    
Private member variables that need to be guarded: self.__pleaseDontMessWithMeFromOutside (two underscores)    
Private functions that need to be guarded: self.__pleaseDontCallMeFromOutside(self) (two underscores)    

# Libraries
Feel free to use whatever is the best as long as its portable. 
We are using the async td-api library: https://tda-api.readthedocs.io/en/latest/
Prefer to use asyncio when it works, raw threads when it doesn't. 




