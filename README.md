# Web Coursework 3 - Hobbies Web Application Using Django, Python, Vue.js and Bootstrap

# TODO - Fill in README

## API

Django python server
Artists and Songs as many-to-many relationship

- conda activate djangoServer
- Run server using `{py or python} manage.py runserver`
- Make migrations when changing a model using `{py or python} manage.py makemigrations`
- Run migrations after making migrations using `{py or python} manage.py migrate`

| Url              | Function                            |
| ---------------- | ----------------------------------- |
| /api/artists     | GET all artists and POST new artist |
| /api/artists/:id | PUT updates or DELETE Artist        |
| /api/songs       | GET all songs and POST new song     |
| /api/songs/:id   | PUT updates or DELETE Song          |

## Home View

Vue.js reactive front-end with Bootstrap for styling.

As this is a one-page application, all of the components are rendered on the same page at '/'.
The page is split into two parts, artists and songs.

In each part there is a table, that can be shown/hidden with the click of a button, which displays all objects for the relative model as well as the information associated with each object.
In the 'Songs' and 'Artists' columns in the tables respectively, the number of related objects is displayed, and when the cell is clicked, the row expands to show the names/titles of the related objects.

For each row in the table, you can edit the object and save the changes in the row, which will then update all related objects on the page assuming the request is successful.
The object can also be deleted from the row, where it will hit the related api for the object with the DELETE request method.

In addition to the tables, there are also create object forms for each model that can be triggered by clicking the create button to the top right of the related table.
Then, for each model all of the available inputs are listed to the user, then the inputs are assembled in the createModel() method and sent to the API through a POST request.
If the response is ok, then it will update the table with the new object that is returned from the API.

## Models

For each model, there is a to_dict() function which converts the model attributes to a Python Dictionary so that it can be handled much more easily in the API, and is organised to be returned as a JSON format.

For each model, there are is an extra api value, which utilise the reverse() url function from django to return the url strings.
The api value points to the api function that can PUT changes and DELETE the object.

In addition to to_dict(), there is an additional function for each model that extends the output of to_dict() which adds the related objects of the other model to the output which will be used in the front-end.
to_dict() is only used in the API as it prevents an infinite loop when getting the dictionary of the other model.
The to_dict_with_model() functions have two attributes, list (list of all associated model objects) and number (number of objects associated).

### Artist

| Attribute   | Type          |
| ----------- | ------------- |
| name        | Char / String |
| nationality | Char / String |
| label       | Char / String |

### Song

| Attribute | Type               |
| --------- | ------------------ |
| title     | Char / String      |
| genre     | Char / String      |
| pub_date  | DateTime           |
| length    | Integer            |
| artists   | ManyToMany(Artist) |

#### to_dict() modifications for Songs:

| Field    | Value                                                                                                              |
| -------- | ------------------------------------------------------------------------------------------------------------------ |
| pub_date | value: raw value, display: display string (DD/MM/YYYY), date_value: value used for date field in form (YYYY-MM-DD) |
| length   | value: raw value in seconds, display: display string (mm:ss)                                                       |
