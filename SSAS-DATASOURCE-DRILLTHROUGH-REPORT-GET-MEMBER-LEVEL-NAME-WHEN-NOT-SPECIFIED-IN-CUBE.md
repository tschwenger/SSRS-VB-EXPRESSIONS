SSRS Expression:

## Get a dynamic cube caption name from the cube name using an SSRS expression

* This is for a drillthrough report if the unique name is not specified in the database

* this is to get a dynamic value from the unique value in a cube while stripping off the non-user friendly brackets

* use the MID function on the parameter value (this has to be specified in the parameters of the SSRS child report)

* use the INSTRREV function to find the last & in the cube value name (if & is used in the friendly name of the caption then you are screwed in this case)

example 

Let's say I want to create a title that says: "Deposits made for Contract 325412"

The Original expression in the child report would be something like this:

="Deposits made for Contract " & Parameters!PAR_CONTRACT.Value

* Parameters!PAR_CONTRACT.Value would be the contract parameter specified in the child report

When this is being passed through a drillthrough report, the unique name (see the below value) would get passed through like this:

"Deposits made for [Plan].[Contract ID].&[325412]" 

SSAS unique value name:

[Plan].[Contract ID].&[325412] 

Getting the unique name will confuse the user, so to avoid this, the following code will help handle those SSAS datasource names dynamically.

* the first INSTRREV gets the length of the first & and then add 2, because this is the structure for the final member 

* the second INSTRREV gets the length of the final "]" at the end of the string

* use the two in the final section of the MID function to subtract each other to get the correct length

MID(Parameters!PAR_CONTRACT.Value,
	INSTRREV(Parameters!PAR_CONTRACT.Value,"&")+2,
	((INSTRREV(Parameters!PAR_CONTRACT.Value,"]")) - INSTRREV(Parameters!PAR_CONTRACT.Value,"&")-2))

This will yield the Contract number: 325412 instead of [Plan].[Contract ID].&[325412]

So the final VB code would be:

="Deposits made for Contract " & MID(Parameters!PAR_CONTRACT.Value,
	INSTRREV(Parameters!PAR_CONTRACT.Value,"&")+2,
	((INSTRREV(Parameters!PAR_CONTRACT.Value,"]")) - INSTRREV(Parameters!PAR_CONTRACT.Value,"&")-2))
	
Yielding: Deposits made for Contract 325412 
