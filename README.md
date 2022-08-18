# `xtdfet` - Dynamic Fixed Effects Regression with Time Interactions

**Table of Contents**
1. [Description](#1-description)
2. [Syntax](#2-syntax)
3. [Options](#3-options)
4. [Estimating Short-Run and Long-Run Effects with Time Interactions](#4-estimating-short-run-and-long-run-effects-with-time-interactions)
5. [Examples](#5-examples)
    1. [Specifying the Lag Structure](#51-specifying-the-lag-structure)  
    2. [Output](#52-output)
    3. [Graphing Results](#53-graphing-results)
6. [Install](#6-install) 
7. [Acknowledgements](#7-acknowledgements)
8. [Author](#8-author)

# 1. Description 

# 2. Syntax

    syntax varlist(ts) [if] [in] [,LAgs(string) Int(string) INTLags(string) Timetest NOtable LIne(string) Bar(string) EXcel(string)]

# 3. Options

`LAgs(string)` specifies the number of lags for each variable in varlist. If no lags are wanted, specify 0. At least one lag of the dependent variable is required. This is a required option. 

`Int(string)` specifies the variable to interact with time. This is a required option. 

`INTLags(string)` specifies the number of lags for the variable in `int`. If no lags are wanted, specify 0. This is a required option. 

`Timetest` tests whether the short-run and long-run effect for each time period is equal to the effect in time period 1 using Wald tests.

`NOtable` suppresses the table of main effects. 

`LIne(string)` generates line graphs of the short-run and long-run effects. A name must be specified to save each graph. The short-run effects graph is saved as *string*\_sr, and the long effects graph is saved as *string*\_lr. 

`Bar(string)` generates bar graphs with error bars of the short-run and long-run effects. A name must be specified to save each graph. The short-run effects graph is saved as *string*\_sr, and the long effects graph is saved as *string*\_lr. 

`EXcel(string)` saves the main effects, short-run effects, and long-run effects in an excel spreadsheet. Each set of results are given their own sheet. 

# 4. Estimating Short-Run and Long-Run Effects with Time Interactions

# 5. Examples 

A U.S. state-level dataset consisting of annual observations of fossil fuel energy consumption per capita, average personal income, and the % of energy consumption from renewable energyfrom 1995 to 2018 can be downloaded here. The examples below are based on this dataset, with the variable of interest being the % of energy consumption from renewable energy (lnrenper). 

## 5.1 Specifying the Lag Structure

A user can specify a lagged dependent variable (LDV) model as follows: 

     xtdfet lnffpc lny, lags(1 0) int(lnrenper) intlags(0)

Users can estimate a more general ARDL(*p*,*q*) model by using a forward slash and specifying the minimum and maximum number of lags wanted (e.g., 1/3). As an example, the commonly used ARDL(1,1) is obtained by specifying:  

     xtdfet lnffpc lny, lags(1 0/1) int(lnrenper) intlags(0/1)

The lags for each variable can be different too: 

     xtdfet lnffpc lny, lags(1/2 0/1) int(lnrenper) intlags(0)
     
If a lag of the independent variable is wanted instead of the contemporaneous effect then the user can omit zero from the model: 

     xtdfet lnffpc lny, lags(1/2 1) int(lnrenper) intlags(0)
   
## 5.2 Output

Without any additional options specified, tables of the main effects, short-run, and long-run effects are displayed. The results in the main effects table appear as they would if the user used `xtreg, fe` and used stata's factor notation. 

## 5.3 Graphing Results

# 6. Install 


    

# 7. Acknowledgements

Special thanks to Xiaorui Huang for inspiring me to write this program.  

# 8. Author

[**Ryan P. Thombs**](ryanthombs.com)  
**(Boston College)**  
**Contact Me: [thombs@bc.edu](mailto:thombs@bc.edu)**
