[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/suhcjUE-)
# Exam #1: "CMSmall"
## Student: s308494 ESPOSITO GABRIELE 

## React Client Application Routes

- Route `/`: pagina del front office, mostra la lista completa delle pagine pubblicate.
- Route `/BackOffcie`: pagina del back office, mostra la lista completa delle pagine, si tiene conto della data corrente e in base a quest'ultima le pagine possono essere programmate o pubblicate, un 'INVALID DATE' => in DRAFT.
- Route `/BackOffice/add`: pagina con i vari form utili alla creazione della pagina, la data di crezione corrisponde alla data corrente, quindi viene generata in automatico.
- Route `/BacOffice/edit/:id`: pagina con i vari form da modificare in base alla pagina da modificare, :id => id della pagina da modificare.
- Route `/login`: pagina del login.
- Route `/*`: Default route per le pagine che non esistono.

## API Server

### List Pages(Front)

URL: `/api/pagesFrontOffice`

Method: GET

Description: Get all published pages, for Front Office.

Request body: _None_

Response: `200 OK` (success) or `500 Internal Server Error` (generic error).

Response body: An array of objects, each describing a page.
```
[{
  "head": {
    "id_page": 1,
    "title": "Contea",
    "author": "Frodo",
    "authorId": 1,
    "data_creation": "2023-06-20"
  },
  "blocchi": [{
    "id_block": 1,
    "type": "h",
    "description": "Per Frodo",
    "pageId": 1
  }
  ...
  ]  
}
...
]
```

### List Pages(Back) AUTENTICATA

URL: `/api/pagesBackOffice`

Method: GET

Description: Get all pages, for Back Office.

Request body: _None_

Response: `200 OK` (success) or `500 Internal Server Error` (generic error).

Response body: An array of objects, each describing a page.
```
[{
  "head": {
    "id_page": 1,
    "title": "Contea",
    "author": "Frodo",
    "authorId": 1,
    "data_creation": "2023-06-20"
  },
  "blocchi": [{
    "id_block": 1,
    "type": "h",
    "description": "Per Frodo",
    "pageId": 1
  }
  ...
  ]  
}
...
]
```

### ADD Page AUTENTICATA

URL: `/api/pages`

Method: POST

Description: Create a new page

Request body: An object representing a page (Content-Type: `application/json`).
```
{
  "head": {
    "title": "Contea",
    "data_creation": "2023-06-20"
    "data_published": "2023-06-20"
  },
  "blocchi": [{
    "type": "h",
    "description": "Per Frodo",
  }
  ...
  ]  
}
```
Response: `201 OK` (success), `404 Not Found`. If the request does not come on an authenticated session, `422` error check the content of the page, `503` `Database error during the creation of Page

Response body: The id of the newly created page, as a JSON value (Content-Type: `application/json`).

### EDIT Page AUTENTICATA
URL: `/api/pages/:id`

Method: POST

Parameters: id of the page you want to edit.

Description: Edit a page, this API does a delete of the page and create a new page,
with another field "author" in the object if the user that is editing the page is an ADMIN.

Request body: An object representing a page (Content-Type: `application/json`).
```
{
  "head": {
    "title": "Contea",
    "data_creation": "2023-06-20"
    "data_published": "2023-06-20"
  },
  "blocchi": [{
    "type": "h",
    "description": "Per Frodo",
  }
  ...
  ]  
  "author": "Frodo"
}
```
Response: `201 OK` (success), `422 Not Found` the content of the page sent is wrong , `503` `Database error during the editing of Page`, or another user(ADMIN) has already changed the content of that page, so to be sure that a user wants to edit the page with the correct data, If the request does not come on an authenticated session, `401 Unauthorized`.

Response body: The id of the newly created page, as a JSON value (Content-Type: `application/json`).

### Delete a Page AUTENTICATA
URL: `/api/pages/<id>`

Method: DELETE

Parameter: id of the page you want to delete

Description: Delete an existing Page, identified by its id. A cookie with a VALID SESSION ID must be provided. The user requesting the deletion of the page must be the same that owns the answer.

Request body: _None_

Response: `200 OK` (success) or `503 Service Unavailable` (generic error). If the request does not come on an authenticated session, `401 Unauthorized`.

Response body: The number of the Row deleted, as a JSON value (Content-Type: `application/json`),
=> {numRowCHangesPages, numRowChangesBlocks}.

### GET the site Name NON AUTENTICATA
URL: `/api/siteName`

Method: GET

Parameter: _None_

Description: Get the website's name from the server

Request body: _None_

Response: `200 No Content` (success), `404` (nameSite not found) `500 Service Unavailable` (generic error)

Response body: The name of the site.

### PUT update the website's name AUTENTICATA
URL: `/api/siteName/:id`

Method: PUT

Parameter: the object's id from the website object

Description: update the websits's name.

Request body: An object representing a page (Content-Type: `application/json`).
{
  "id": 1,
  "name": "Il Signore degli anelli"
}

Response: `200 OK` (success), `422` (id not Found) `503 Database error during the update of the siteName` (Internal database error)

### __Create a new session (login)__

URL: `/api/sessions`

HTTP Method: POST

Description: Create a new session starting from given credentials.

Request body:
```
{
  "username": "frodo@ring.com",
  "password": "pwd"
}
```

Response: `200 OK` (success), `401` wrong login credentials `500 Internal Server Error` (generic error).

Response body: _None_

### __Get the current session if existing__

URL: `/api/sessions/current`

HTTP Method: GET

Description: Verify if the given session is still valid and return the info about the logged-in user. A cookie with a VALID SESSION ID must be provided to get the info of the user authenticated in the current session.

Request body: _None_ 

Response: `200 Created` (success) or `401 Unauthorized user!` (error).

Response body:
```
{
  "username": "frodo@ring.com",
  "id": 1,
  "name": "Frodo"
}
```

### __Destroy the current session (logout)__

URL: `/api/sessions/current`

HTTP Method: DELETE

Description: Delete the current session. A cookie with a VALID SESSION ID must be provided.

Request body: _None_

Response: `200 OK` (success) or `500 Internal Server Error` (generic error).

Response body: _None_



## Database Tables

- Table `users` - contains xx yy zz
- Table `something` - contains ww qq ss
- ...

## Main React Components

- `ListOfSomething` (in `List.js`): component purpose and main functionality
- `GreatButton` (in `GreatButton.js`): component purpose and main functionality
- ...

(only _main_ components, minor ones may be skipped)

## Screenshot

![Screenshot](./img/screenshot.jpg)

## Users Credentials

- username, password (plus any other requested info)
- username, password (plus any other requested info)
