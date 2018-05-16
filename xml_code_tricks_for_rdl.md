# Updates to xml code behind the scenes that can help correct default ssrs behavior

## Ensure SSRS doesn't automatically update parameter data set

Situation: update the originally report query will automatically update parameter query. we want to suppress this behavior, because we lose our code

Find the following line of code in the xml:

      <rd:DesignerState>
     
Add this line of code:

      <rd:SuppressAutoUpdate>True</rd:SuppressAutoUpdate>
  
