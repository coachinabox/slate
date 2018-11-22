# Controller **`/doc_interface`**

## **`POST`** render card layout partial (`/doc_interface/card_layout`)

> Sample call:

```shell
curl --include --request POST --data="JSON_DECK_DATA_STRUCTURE" \
     "<TSv2_API_HOST>/sessions/create"
```

```javascript
// Call Rails endpoint via AJAX to apply current card layout to JSON data:
function ajaxTestDoCInterfaceCardLayout( jsonDeckData ) {
  $.ajax({
    url: "<TSv2_API_HOST>/doc_interface/card_layout",
    method: "post",
    data: jsonDeckData,
    success: function(result) {
      console.log("/doc_interface/card_layout call ok.");
      // Display resulting partial:
      $('#wherever-you-wanna-put-the-result-into').html( result );
    },
    error: function(result) {
      console.log("/doc_interface/card_layout response error!");
      // Clear the partial in case of error:
      $('#wherever-you-wanna-put-the-result-into').html('');
    },
    dataType: "html"
  });
}
//--------------------------------------------------------------------
```

> Response (200, 'text/html'):

```html
<p>
  This is a placeholder for what will be an <code>HTML snippet</code>
  obtained from rendering the current <b>card layout</b>, filled-in with
  the details of the JSON deck data payload.<br/>
  This HTML partial can be easily <i>included directly on a DIV inside a page</i>,
  or wrapped in another container in order to be displayed.<br/>
  (The actual HTML resulting code is not included here since it's too long and complex.)
</p>
```

Retrieves the rendered HTML partial that can be used to display a single Card with its contents.

The resulting Card "widget" is somehow simplified (and possibly optimized in its usage of the CSS styles).
It won't have the highlighting feature incorporated, but it'll include the Javascript code for flipping on its sides.

For this reason, the resulting text should always be _evalutated_ for Javascript code by the callback on a successful retrieval, in order for the flip button to work.

Currently this resource works only via XHR (AJAX) POST request, with no authentication checks.

Future versions may introduce a static token or a session authorization.


### HTTP Request

`POST <TSv2_API_HOST>/doc_interface/card_layout`


### Body Parameters

> Example dataset valid for both the _data payload_ of the Body parameter, or as object value of `jsonDeckData` in the Javascript example above:

```json
{
  "id": 1,
  "brand": "BTS",
  "name": "Twelve Shifts",
  "supportedLanguages": [
    "en",
    "fr"
  ],
  "frontHeader": "some/upload/url/to_an_asset1.png",
  "backHeader": "some/upload/url/to_an_asset2.png",
  "clientLogoOne": "some/upload/url/to_an_asset3.png",
  "clientLogoTwo": "some/upload/url/to_an_asset4.png",
  "vertical": false,
  "customCss": ".custom_card_title {\n  color: #28324b;\n}\n.custom_card_text {\n  color: #28324b;\n}\n.custom_card_body {\n  background-color: #ffffff;\n}\n.custom_card_header {\n  color: #fb054b;\n  background-color: #afafaf;\n}\n.custom_sort_button {\n  color: #ffffff;\n  background-color: #fb054b;\n}\n",
  "frontTitleOne": {
    "en": "Development",
    "fr": "Development FR"
  },
  "frontTitleTwo": {
    "en": "Example Challenge",
    "fr": "Example Challenge FR"
  },
  "backTitle": {
    "en": "TBC",
    "fr": "TBC FR"
  },
  "welcomeText": {
    "en": "<h1>Welcome</h1>\r\n<p>This card sort is a process which will provide you and your Coach with some insight into your strengths and development areas.</p>\r\n<p>This card sort process will take you 20 to 30 minutes to complete.</p>",
    "fr": "<h1>Welcome FR</h1>\r\n<p>This card sort is a process which will provide you and your Coach with some insight into your strengths and development areas.</p>\r\n<p>This card sort process will take you 20 to 30 minutes to complete.</p>"
  },
  "strengthPreText": {
    "en": "<h1>Strength Sort</h1>\r\n<p>The first stage of this process is to identify your strengths.</p>\r\n<p>You will now be presented with a series of cards. Please read each card and, without too much thought, sort the cards into one of three piles: Strengths, Development Areas and those which are neither a clear strength nor development area.</p>\r\n<p>Once you have sorted all cards, you should have three roughly equal piles of cards.</p>",
    "fr": "<h1>Strength Sort FR</h1>\r\n<p>The first stage of this process is to identify your strengths.</p>\r\n<p>You will now be presented with a series of cards. Please read each card and, without too much thought, sort the cards into one of three piles: Strengths, Development Areas and those which are neither a clear strength nor development area.</p>\r\n<p>Once you have sorted all cards, you should have three roughly equal piles of cards.</p>"
  },
  "strengthPostText": {
    "en": "<h1>Strengths Sort Complete?</h1>\r\n<p>You have now identified your strengths.</p>\r\n<p>You can review each pile of cards by clicking on the box on the left hand side and scrolling through the cards.</p>\r\n<p>If you change your mind and want to move a card to a different pile, just click on the relevant sort button underneath.</p>\r\n<p>Once you are happy with the sort you have completed, please click the button to go to Stage 2.</p>",
    "fr": "<h1>Strengths Sort Complete? FR</h1>\r\n<p>You have now identified your strengths.</p>\r\n<p>You can review each pile of cards by clicking on the box on the left hand side and scrolling through the cards.</p>\r\n<p>If you change your mind and want to move a card to a different pile, just click on the relevant sort button underneath.</p>\r\n<p>Once you are happy with the sort you have completed, please click the button to go to Stage 2.</p>"
  },
  "contextPreText": {
    "en": "<h1>Context Sort</h1>\r\n<p>Now that you have identified your strengths, we have put these cards to one side (virtually) and we would now like you to carry out a second sort on the remaining cards.</p>\r\n<p>This time, you are sorting the cards into three piles according to the importance of that card to you at the moment.</p>",
    "fr": "<h1>Context Sort FR</h1>\r\n<p>Now that you have identified your strengths, we have put these cards to one side (virtually) and we would now like you to carry out a second sort on the remaining cards.</p>\r\n<p>This time, you are sorting the cards into three piles according to the importance of that card to you at the moment.</p>"
  },
  "contextPostText": {
    "en": "<h1>Context Sort Complete?</h1>\r\n<p>You have now sorted the remaining cards according to their importance to you.</p>\r\n<p>You can review each pile of cards by clicking on the box on the left-hand side.</p>\r\n<p>Your coach will discuss these results with you during your next coaching session.</p>",
    "fr": "<h1>Context Sort Complete? FR</h1>\r\n<p>You have now sorted the remaining cards according to their importance to you.</p>\r\n<p>You can review each pile of cards by clicking on the box on the left-hand side.</p>\r\n<p>Your coach will discuss these results with you during your next coaching session.</p>"
  },
  "resultText": {
    "en": "<h1>Results</h1>\r\n<p>TBD</p>",
    "fr": "<h1>Results FR</h1>\r\n<p>TBD</p>"
  },
  "cards": [
    {
      "id": 1,
      "name": "Influencing others",
      "ordinal": 1,
      "domain": "Relate",
      "level": 1,
      "tags": {},
      "frontTextOne": {
        "en": "Flexing my approach to find the right way to influence and motivate different people",
        "fr": null
      },
      "frontTextTwo": {
        "en": "There is a particular individual or group of individuals that I find it hard to get through to",
        "fr": null
      },
      "backText": {
        "en": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. ",
        "fr": null
      }
    }
  ]
}
```

The JSON deck data (`jsonDeckData` in the Javascript example) that has to be used to populate the resulting card layout, has to be sent with the request itself as the full body of POST payload data.

Its format is the one defined by the DeckOfCards API.
(Current schema at: [CiaB API call for GET deck_details](http://coachinabox.github.io/coach_in_a_box/doc#get-details-about-a-deck))
