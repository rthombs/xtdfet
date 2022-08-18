# `xtdfet`
Dynamic Fixed Effects Regression with Time Interactions

# Description 


# Syntax

    syntax varlist(ts) [if] [in] [,LAgs(string) Int(string) INTLags(string) Timetest NOtable LIne(string) Bar(string) EXcel(string)]

## Options

`LAgs(string)` specifies the number of lags for each variable in varlist. If no lags are wanted, specify 0. At least one lag of the dependent variable is required. This is a required option. 

`Int(string)` specifies the variable to interact with time. This is a required option. 

`INTLags(string)` specifies the number of lags for the variable in `int`. If no lags are wanted, specify 0. This is a required option. 

`Timetest` tests whether the short-run and long-run effect for each time period is equal to the effect in time period 1 using Wald tests.

`NOtable` suppresses the table of main effects. 

`LIne(string)` generates line graphs of the short-run and long-run effects. A name must be specified to save each graph. The short-run effects graph is saved as *string*\_sr, and the long effects graph is saved as *string*\_lr. 

`Bar(string)` generates bar graphs with error bars of the short-run and long-run effects. A name must be specified to save each graph. The short-run effects graph is saved as *string*\_sr, and the long effects graph is saved as *string*\_lr. 

`EXcel(string)` saves the main effects, short-run effects, and long-run effects in an excel spreadsheet. Each set of results are given their own sheet. 

# Example 

A U.S. state-level dataset consisting of annual observations of fossil fuel energy consumption per capita, average personal income, and the % of energy consumption from renewable energyfrom 1995 to 2018 can be downloaded here. The examples below are based on this dataset, with the variable of interest being the % of energy consumption from renewable energy (lnrenper). 

A user can specify a lagged dependent variable (LDV) model as follows: 

     xtdfet lnffpc lny, lags(1 0) int(lnrenper) intlags(0)

Users can estimate a more general ARDL(*p*,*q*) model by using a forward slash and specifying the minimum and maximum number of lags wanted (e.g., 1/3). As an example, the commonly ARDL(1,1) is obtained by specifying:  

     xtdfet lnffpc lny, lags(1 0/1) int(lnrenper) intlags(0/1)

The lags for each variable can be different too: 

     xtdfet lnffpc lny, lags(1/2 0/1) int(lnrenper) intlags(0)
     
If a lag of the independent variable is wanted instead of the contemporaneous effect then the user can omit zero from the model: 

     xtdfet lnffpc lny, lags(1/2 1) int(lnrenper) intlags(0)

# Install 


    

# Acknowledgements

Special thanks to Xiaorui Huang for inspiring me to write this program.  

# Author

[**Ryan P. Thombs**](ryanthombs.com)  
**(Boston College)**  
**Contact Me: [thombs@bc.edu](mailto:thombs@bc.edu)**
