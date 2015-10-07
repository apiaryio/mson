# Notes API

## Notes [/notes]
 
### List all [GET]
+ Response 200 (application/json)
    + Attributes (array[Note])
 
## Note [/notes/{id}]
+ Attributes
    + id: 42 (number) - id of the note
    + title: Buy Milk (string) - title of the note
  
### Retrieve a Note [GET]
+ Response 200 (application/json)
    + Attributes (Note)
