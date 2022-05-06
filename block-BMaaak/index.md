writeCode

Q1. Create an userSchema which has fields

- name
- username
- email
- address {
  - city
  - state
  - country
  - pin
    }



1. Define unique indexes on username and email.

 - name:String
- username:{type:String,unique:true}
- email :{type:String,unique:true}
- address {
  - city
  - state
  - country
  - pin
    }

    



2. define compound indexes on state and country field inside address document. Each country must have states which are unique.

Ans- userSchema.index({"address.country":1,"address.state":1})

Q2. Create an article Schema with fields

- title
- description
- tags[String]

1. Add multikey indexes on tags which is an array of strings

Ans - articleSchema.index({tags:1})
2. Add text indexes on title as users will search for texts present in an article's title.
Ans - articleSchema.index({title:"text"});
3. Update text indexes to include descriptions as well. Implement text indexes on both title and description.
Ans - articleSchema.index({title:"text",description:"text"});