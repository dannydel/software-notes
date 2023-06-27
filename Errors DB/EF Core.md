# Errors and Possible Solutions


# 1. 
**Error**
>Cannot insert duplicate key row in object 'dbo.Prices' with unique index 'FK_price_loan_idx'. The duplicate key value is (3042).

**Solution(s)
1. There could be an object that is being "newed up" in the model that is attempting to be updated or saved. Remove the new initialization of that object.


