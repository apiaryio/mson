# Notes API

## Notes [/notes]
+ Model (application/json)
 
    + Body (array: Note)

    + Body
 
            [ { "id": 42, "title": "Buy Milk" } ]
 
### List all [GET]
+ Response 200
 
    [Notes][]
 
## Note [/notes/{id}]
+ Model (application/json)
 
    + Body (object)
 
        + id: 42 (number) - id of the note
        + title: Buy Milk (string) - title of the note
 
    + Body
 
            { "id": 42, "title": "Buy Milk" }
 
### Retrieve a Note [GET]
+ Response 200
 
    [Note][]
