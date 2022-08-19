# `xtdfet` - Dynamic Fixed Effects Regression with Time Interactions

### Currently Under Construction. Program will be released shortly (Updated 8/19/2022).

**Table of Contents**
1. [Description](#1-description)
2. [Syntax](#2-syntax)
3. [Options](#3-options)
4. [Estimating Short-Run and Long-Run Effects with Time Interactions](#4-estimating-short-run-and-long-run-effects-with-time-interactions)
5. [Examples](#5-examples)
    1. [Specifying the Lag Structure](#51-specifying-the-lag-structure)  
    2. [Output](#52-output)
    3. [Graphing Results](#53-graphing-results)
    4. [Testing Whether Effects Change Over Time](#54-testing-whether-effects-change-over-time)
    5. [Estimating Effects for a Subsample of Time](#55-estimating-effects-for-a-subsample-of-time)
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

**Note**: Robust standard errors are automatically used. 

# 4. Estimating Short-Run and Long-Run Effects with Time Interactions

The short-run effect for each year is calculated as:

     _b[var] + _b[var*time_#] 
     
As an example, if the user has annual data from 1990 to 2015, then the short-run effect for 1990 is: 
 
     _b[var] + _b[var*time_1990] 

For 1991 it is:

     _b[var] + _b[var*time_1991] 

and so on. 

The long-run effects are estimated by summing the coefficients of the moderating variable by year and dividing it 1-the sum of coefficients on the lag(s) of the dependent variable. For example, for an lagged dependent variable (LDV) model, the long-run effect is calculated as: 

     (_b[var] + _b[var*time_1990])/(1 - _b[L.DV])
     
An ARDL(1,1), which includes one lag of the dependent and independent variables in the model is calculated as: 

     (_b[var] + _b[var*time_1990] + _b[L.var] + _b[L.var*time_1990])/(1 - _b[L.DV])

This can be extended to higher order lags, e.g., an ARDL(2,2): 

     (_b[var] + _b[var*time_1990] + _b[L.var] + _b[L.var*time_1990] + _b[L2.var] + _b[L2.var*time_1990])/(1 - _b[L.DV] - _b[L2.DV])

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

Without any additional options specified, tables of the main effects, short-run, and long-run effects are displayed. The results in the main effects table appear as they would if the user used `xtreg, fe` and used stata's factor notation. The short-run effects are calculated and displayed immediately after the main results: 

<p align="center">
 <img src="https://user-images.githubusercontent.com/40503845/185687751-93ed5a42-b152-4c83-9c1d-77ed9bdf0e2a.jpg"/>
    <br>
    <em>Figure 1. Short-Run Effects Display</em>
</p>

The long-run effects are then displayed immediately after the short-run effects: 

<p align="center">
 <img src="https://user-images.githubusercontent.com/40503845/185688328-b744f207-4646-4e89-bc20-c95105bf5811.jpg"/>
    <br>
    <em>Figure 2. Long-Run Effects Display</em>
</p>

The main effects, short-run effects, and long-run effects can be exported to `EXcel` option:

     xtdfet lnffpc lny, lags(1 0/1) int(lnrenper) intlags(0/1) excel(results)

## 5.3 Graphing Results

Visual results of the short-run and long-run effects are obtained by specifying the `LIne` or `Bar` options. The following code will produce a line graph that will save the short-run effects graph as "example_sr" and the long-run effects graph as "example_lr": 

     xtdfet lnffpc lny, lags(1 0) int(lnrenper) intlags(0) li(example)

<p align="center">
 <img src="https://user-images.githubusercontent.com/40503845/185658638-aea649c5-fc05-47a0-8857-16c42ea33ad2.jpg"/>
    <br>
    <em>Figure 3. Short-Run Effects</em>
</p>

<p align="center">
 <img src="https://user-images.githubusercontent.com/40503845/185658796-1463aa86-0dda-45e9-98e9-5f44107cf654.jpg"/>
    <br>
    <em>Figure 4. Long-Run Effects</em>
</p>

Here, the two line graphs look similar except for the y-axis. That is because the long-run effects are a simple rescaling of the contemporaneous, short-run effect in a LDV model. We can also graph the short-run and long-run effects with a bar graph: 

     xtdfet lnffpc lny, lags(1 0) int(lnrenper) intlags(0/1) b(example)
     
<p align="center">
 <img src="https://user-images.githubusercontent.com/40503845/185670498-39715b45-5123-4f80-b37f-8c5f78e23a1d.jpg"/>
    <br>
    <em>Figure 5. Short-Run Effects</em>
</p>

<p align="center">
 <img src="https://user-images.githubusercontent.com/40503845/185670555-82fcd18a-bfb8-415d-9707-7d77af2c4d2d.jpg"/>
    <br>
    <em>Figure 6. Long-Run Effects</em>
</p>

# 5.4. Testing Whether Effects Change Over Time

In addition to determining whether the effects are statistically different than zero, users are often interested whether each period's effect is statistically different from the first period effect. Users can determine this by specifying the `timetest` option:

     xtdfet lnffpc lny, lags(1 0) int(lnrenper) intlags(0) timetest

# 5.5. Estimating Effects for a Subsample of Time

The time interactions are based on how the data are `xtset`. If the user's data sample extends from 1970 to 2010 and they want to examine only the 1970 to 1990 period then they can do the following: 

     preserve 
     drop if year>1991
     
     xtdfet ...
     
     restore 

# 6. Install 

Email me at [thombs@bc.edu](mailto:thombs@bc.edu) for the current version. A public version of the program will be made available shortly. 
    

# 7. Acknowledgements

Special thanks to Xiaorui Huang for inspiring me to write this program.  

# 8. Author

[**Ryan P. Thombs**](ryanthombs.com)  
**(Boston College)**  
**Contact Me: [thombs@bc.edu](mailto:thombs@bc.edu)**
