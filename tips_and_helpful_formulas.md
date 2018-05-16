# Helpful Tips and Tricks for Reports

### Placeholder expression

Problem: get an error message in report layer, because of multiple values in parameters cannot be shown

basic expression (causes error):

        Parameters!CenterList.Label

Fixed expression:

        =Join(Parameters!CenterList.Label,", ")
