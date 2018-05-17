# Helpful Tips and Tricks for Reports

### Placeholder expression

#### Problem: get an error message in report layer, because of multiple values in parameters cannot be shown Note: this will work if there is only one value for the report. 

basic expression (causes error):

        =Parameters!CenterList.Label

Fixed expression:

        =Join(Parameters!CenterList.Label,", ")

#### Problem: All the selected values in a parameter show in the report and it gets cluttered

this takes the above example one step further (if QA is giving you hell) note the field expression is coming from the dataset in a table within the report layer which the parameter value must equal note, the parameter is the parameter for the referenced field value:

Fixed expression:

        
        =Iif(Parameters!ClassroomList.Count = Count(Fields!ExternalClassroomName.Value,"ClassroomList"),"All", Join(Parameters!ClassroomList.Label,", "))


### Use a Like search find in mdx parameter

Situation: A parameter/filter in the report has too much results and we want to search and filter down the filter
        Think something like Products or Cities etc. 
        
Add the following dataset type to the report:

        //--Like Operator Query
        //WITH
        //SET Product
        //AS
        //HEAD(FILTER(
        //      [Product].[Product].CHILDREN,
        //      vbamdx!INSTR([Product].[Product].CURRENTMEMBER.Name,@ProductLike,1 )
        //),10)
        //
        //MEMBER [Measures].[ParameterCaption] AS [Product].[Product].CURRENTMEMBER.MEMBER_CAPTION
        //
        //MEMBER [Measures].[ParameterValue] AS [Product].[Product].CURRENTMEMBER.UNIQUENAME
        //
        //MEMBER [Measures].[ParameterLevel] AS [Product].[Product].CURRENTMEMBER.LEVEL.ORDINAL
        //
        //SELECT
        //{[Measures].[ParameterCaption]
        //,[Measures].[ParameterValue]
        //,[Measures].[ParameterLevel]
        //} ON COLUMNS
        //, Product ON ROWS
        //FROM [Adventure Works]

This enables the user to enter in a search value and filter down the original parameter
