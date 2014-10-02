# Notes API

## Notes [/notes]
+ Model

    + Body (array[Note])
 
### List all [GET]
+ Response 200 (application/json)
 
    [Notes][]
 
## Note [/notes/{id}]
+ Model

    + Body (object)
 
        + id: 42 (number) - id of the note
        + title: Buy Milk (string) - title of the note
  
### Retrieve a Note [GET]
+ Response 200 (application/json)
 
    [Note][]
