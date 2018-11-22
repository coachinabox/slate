---
title: TwelveShifts v2 API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='http://coachinabox.github.io/coach_in_a_box/mother'>'MOTHER' API</a>
  - <a href='http://coachinabox.github.io/coach_in_a_box/doc#deck-of-cards-api'>Deck of Cards API</a>
  - <a href='http://coachinabox.github.io/coach_in_a_box/#api-standards-and-conventions'>API standards and conventions</a>
  - <a href='http://coachinabox.github.io/coach_in_a_box/#http-responses'>HTTP response codes</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - api_v2
  - sessions
  - doc_interface
  - errors

search: true
---

# TwelveShifts v2 API


## Introduction

This document refers to both actual API endpoints and controller endpoints used by the TwelveShifts v2 application to interact with the outside world.

Authorization for this API depends on the endpoint used, but it's essentially of two kinds:

- a static string token for endpoints or controller actions that do not have an associated session;
- a dynamic, expirable JWT token for anything that requires a session.

Birds-eye-view of the typical use-case:

![Card sort requests](images/api_card_sort.png "Card sort requests image")

Behind the hood, when a new Card Sort is requested, TSv2 makes an API call to Deck-Of-Cards, in order to
retrieve all Deck details needed to create the new Card Sort process locally.

The API endpoint used to retrieve the details of the Deck can be configured inside `config/secrets.yml`, together with the value of the _static token_ that has to be used to authorize certain _incoming_ API calls (such as the creation of a new card sort or the renewal of a JWT session), as well as certain _outgoing_ API calls (such as the retrieval of Deck details).

Please, refer to the deploy configuration in order to customize these static values for each deploy environment.
Typically, on `production` these can simply be set by using `ENV` variables.

---


## Summary:

The [`/api/v2`](#group-api-v2) groups all publicly available "external" endpoints, used for interaction with the rest of the system.

The [`/sessions`](#controller-sessions) controller is the specific entry-point used to create or update an authorized session given a JWT associated to an existing Card sort process.

The [`/card_sort`](#controller-card_sort) controller is the typical entrypoint for any ongoing CardSort-JWT session.

The [`/doc_interface`](#controller-doc_interface) controller is dedicated to the rendering of HTML partials specifically needed by the Deck-Of-Cards application.


### Endpoints for group `/api/v2/`

- **GET**  `/api/v2/status` (_no auth_): returns the API 'msg' status and the application 'version' fields.
- **POST** `/api/v2/card_sort/start` (_static token_): always creates a new card sort for a user, returning a JWT to connect to its endpoints.
- **POST** `/api/v2/session` (_JWT_): creates a new JWT in order to connect to its endpoints; handy in case of expired JWTs.
- **POST**  `/api/v2/card_sort/for_user` (_static token_): returns the list of all the existing card sorts for a specified user (`source_id`), with some status data about the ongoing process and a new dynamic JWT token that can be used to connect to this single card sort in the list.
- **POST**  `/api/v2/card_sort/for_cohort` (_static token_): same as above, but returns the list of all the existing card sorts for a cohort of Pathway IDs (`pathway_ids`).


### Endpoints for controller `/sessions`

- **POST** `/sessions/create` (_JWT_): allows to create a new JWT session in order to continue or connect to an existing card sort process, given its associated JWT. The JWT must be valid and not expired.


### Endpoints for controller `/card_sort`

- **GET** `/card_sort/continue` (_JWT_): continues or connects to an existing card sort process, given its associated JWT. The JWT must be valid and not expired.

- **GET** `/card_sort/show` (_JWT_): displays the end result report of a completed Card Sort.

Any GET request may be successful (with no parameters and no headers) if a previous HTTP session has been initiated (and stored/serialized) with a call to `/sessions/create`.


### Endpoints for controller `/doc_interface`

- **POST** `/doc_interface/card_layout` (_no auth_): returns the complete rendered HTML partial that can be inserted anywhere to display a single Card, by specifying the card contents as data payload of the request. (Restrictions may apply.)


---
