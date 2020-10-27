# API documentation

## Resources

### Search

Find item information in Zotero using search. Use search to find items by title, tags, collection, and document type. Search can discover items in the Trash as well.

Endpoints
Base URL: https://api.zotero.org

Requests in specific libraries begin with: /users/{userID} or {groupID}


### Search Endpoints

| Method | Endpoint                      | Description                                      |
|--------|-------------------------------|--------------------------------------------------|
| Get    | {userID}/searches             | Get all saved searches in the library. |
| Get    | {userID}/searches/{searchkey} | Get a specific saved search in the library       |
| Get    | /users/{userID}/tags          | Get a list of all tags in the library            |
| Delete | /users/{userID}//searches?searchKey=<searchKey>,         | Delete a search in the library            |

<!-- To be recognizable at a glance, I would use the customary ALL CAPS on the endpoint method. Instead of using "get" as a verb also, I might say "returns" or "retrieves" which is probably closer to what the api script does -->

Example Request:
GET https://api.zotero.org/groups/2199751/searches

Components:

* Method : GET
* Users Parameter: /groups/ 
* groupID: 2199751
* Search Parameter: /searches 

### General Parameters


You must include the following parameters in your request for data from specific libraries. If you do not include these parameters, you may encounter an error in your request.

Note: You must use the /users/{userID} OR /groups/{groupID}

| Parameters        | Value   | Data Type | Description                                                                                           | Optional/Required |
|-------------------|---------|-----------|-------------------------------------------------------------------------------------------------------|-------------------|
| /users/{userID}   | integer | id        | Specify a specific user's library in which you wish to access items, collections, searches and more   | required          |
| /groups/{groupID} | integer | id        | Specify a specific group's library in which you wish to access items, collections, searches and more. | required          |
| v=3               | string  | null      | Specify a specific API version for code request documentation                                         | optional          |


### Search parameters

| Parameters | Value                                                           | Data Type        | Description                                                                                                                                 | Optional/Required |
|------------|-----------------------------------------------------------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------|-------------------|
| Since      | integer                                                         | 0                | Request search results for items modified after a specific library version update.                                                          | optional          |
| q          | string                                                          | null             | Conduct a quick search for title and individual creator fields.                                                                             | optional          |
| qmode      | titleCreatorYear, everything                                    | titleCreatorYear | Enable a quick search mode for title and individual creator fields by year. Specify "everything" to include the full context of the search. | optional          |
| tag        | search syntax varying from item to item. Example: itemType=book | null             | Conduct a quick search from tags placed on items.                                                                                           | optional          |                                  | yes             |

<!-- "Value" and "Data Type" columns are reversed. Base information for the parameters looks good. Descriptions are also tighter - they are verb phrases, but that seems okay given that they are query parameters. Probably want to label the parameters as such. "q" and "qmode" could use some differentiation. Do we need to describe the search on "q" and "tag" as a "quick" search? I'm not sure what the adjective designates, if something specific. -->

### Items

Items are documents contained within the Zotero library. Items can be materials such as journal articles, books, news articles, blogs, online publications and more. You may search for items in the library even if you have moved the items to the trash.


### Item Endpoints

| Method | Endpoint                                                                 | Description                                                                                           |
|--------|--------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Get    | /users/{userID}/                                                         | Specify items within a specific user's library                                                        |
| Get    | /groups/{groupID}/                                                       | Specify items within a specific group's library                                                       |
| Get    | /users OR groups/{userID}OR{groupID}/top                                 | Search for top level items in the library. Items in this request do not include items from the trash. |
| Get    | /users OR groups/{userID}OR{groupID}//items/trash                        | Search for items contained in the library's trash.                                                    |
| Get    | /users OR groups/{userID}OR{groupID}/items/itemkey                       | Search for a specific item in the library.                                                            |
| Get    | /users OR groups/{userID}OR{groupID}/items/itemkey/children              | Search for a specific child item under an item in the library.                                        |
| Get    | /users OR groups/{userID}OR{groupID}/publications/items                  | Search for items in publications.                                                                     |
| Get    | /users OR groups/{userID}OR{groupID}/collections/collectionkey/items     | Search for specific items within a collection in the library.                                         |
| Get    | /users OR groups/{userID}OR{groupID}/collections/collectionkey/items/top | Search for top level items in a collection in the library.                                            |


### Item Parameters

| Parameter      | Value                                                         | Data Type            | Description                                                                                   | Optional/Required |
|----------------|---------------------------------------------------------------|----------------------|-----------------------------------------------------------------------------------------------|-------------------|
| itemKey        | string                                                        | null                 | Search for individual or groups of items. Search for up to 50 item keys in a single request.  | optional          |
| itemType       | search syntax varying form item to item. Example: tag=foo bar | null                 | Search for a specific item type.                                                              | optional          |
| itemQ          | string                                                        | null                 | Conduct a quick item search request.                                                          | optional          |
| itemQmode      | contains, startswith                                          | contains             | Conduct a quick search mode item request.                                                     | optional          |
| itemTag        | search syntax varying from item to item. Example: tag=foo     | null                 | Search by tags included on items.                                                             | optional          |
| includeTrashed | 0, 1                                                          | 0 (except in /trash) | Search for items included in the trash in group or individual libraries.                      | optional          |

<!-- Similar comments as with the previous parameters table: switched "values" and "data type" columns, need to check on some typos, question about the adjective "quick," differentiation between "q" and "q mode", also might want to indicate that these are query parameters, as opposed to the <userOrGroupPrefix> which is a path parameter -->

## Potential errors

If you forget to include the full required path parameters /users/{userID} OR /groups/{groupID}, you will receive the error "404 not found."

Error "404 not found" will appear if you omit /users/ OR /groups/.

Error "403 Forbidden" will appear if you omit specific {userID} OR {groupID} numbers.

Error "403 Forbidden" will appear if you try to access a private library

An important note: <!-- Note: --> user or group libraries that have not been listed as public will not return search or item results. You must search within libraries that allow anyone to view the library's items. You can access a list of libraries categorized by group and by people via this link https://www.zotero.org/search/type/group


### Response example for a single item within a group library using an itemkey.

GET https://api.zotero.org/groups/2561333/items/4MUH7D5W?v=3

Image shows what a response for an item for a specific group library will look like on the Zotero web app.

![API Documentation FINAL Deijah Scales-b5d3e4cd](https://media.github.ncsu.edu/user/18564/files/34243380-fcfb-11ea-8c42-8b6f0982afba)

```
{
    "key": "4MUH7D5W",
    "version": 25,
    "library": {
        "type": "group",
        "id": 2561333,
        "name": "Health Fair - Sleep Deprivation",
        "links": {
            "alternate": {
                "href": "https://www.zotero.org/groups/health_fair_-_sleep_deprivation",
                "type": "text/html"
            }
        }
    },
    "links": {
        "self": {
            "href": "https://api.zotero.org/groups/2561333/items/4MUH7D5W",
            "type": "application/json"
        },
        "alternate": {
            "href": "https://www.zotero.org/groups/health_fair_-_sleep_deprivation/items/4MUH7D5W",
            "type": "text/html"
        }
    },
    "meta": {
        "createdByUser": {
            "id": 6823912,
            "username": "cheystory",
            "name": "",
            "links": {
                "alternate": {
                    "href": "https://www.zotero.org/cheystory",
                    "type": "text/html"
                }
            }
        },
        "creatorSummary": "Joo et al.",
        "parsedDate": "2012-06",
        "numChildren": 1
    },
    "data": {
        "key": "4MUH7D5W",
        "version": 25,
        "itemType": "journalArticle",
        "title": "Adverse Effects of 24 Hours of Sleep Deprivation on Cognition and Stress Hormones",
        "creators": [
            {
                "creatorType": "author",
                "firstName": "Eun Yeon",
                "lastName": "Joo"
            },
            {
                "creatorType": "author",
                "firstName": "Cindy W",
                "lastName": "Yoon"
            },
            {
                "creatorType": "author",
                "firstName": "Dae Lim",
                "lastName": "Koo"
            },
            {
                "creatorType": "author",
                "firstName": "Daeyoung",
                "lastName": "Kim"
            },
            {
                "creatorType": "author",
                "firstName": "Seung Bong",
                "lastName": "Hong"
            }
        ],
        "abstractNote": "Background and Purpose\nThe present study was designed to investigate whether 24 h of SD negatively affects the attention and working memory and increases the serum concentrations of stress hormones, glucose, and inflammatory markers.\n\nMethods\nThe acute effects of sleep deprivation (SD) on cognition and the stress hormones were evaluated in six healthy volunteers (all men, age 23-27 years). All were good sleepers, had no history of medical or neuropsychiatric diseases, and were not taking any kind of medication. All of the volunteers were subjected to the Continuous Performance Test (CPT) for attention and working memory of cognition and blood tests both before and after 24 h of SD. Electroencephalographic monitoring was performed during the study to confirm the wakefulness of the subjects.\n\nResults\nSD significantly elevated the serum concentrations of stress hormones (cortisol, epinephrine, and norepinephrine), but serum levels of glucose and inflammatory markers were not changed compared to baseline. For easier steps of the CPT the subjects performed well in giving correct responses after SD; the correct response scores decreased only at the most difficult step of the CPT. However, the subjects performed consistently poor for the error responses at all steps after SD. There was no correlation between the CPT scores and stress hormone levels.\n\nConclusions\nThe 24 h of SD significantly heightened the levels of stress hormones and lowered attention and working memory. The acute SD condition seems to render the subject more susceptible to making errors.",
        "publicationTitle": "Journal of Clinical Neurology (Seoul, Korea)",
        "volume": "8",
        "issue": "2",
        "pages": "146-150",
        "date": "2012-6",
        "series": "",
        "seriesTitle": "",
        "seriesText": "",
        "journalAbbreviation": "J Clin Neurol",
        "language": "",
        "DOI": "10.3988/jcn.2012.8.2.146",
        "ISSN": "1738-6586",
        "shortTitle": "",
        "url": "https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3391620/",
        "accessDate": "2020-09-18T14:36:39Z",
        "archive": "",
        "archiveLocation": "",
        "libraryCatalog": "PubMed Central",
        "callNumber": "",
        "rights": "",
        "extra": "PMID: 22787499\nPMCID: PMC3391620",
        "tags": [],
        "collections": [
            "IWK7QWGD"
        ],
        "relations": {},
        "dateAdded": "2020-09-18T14:36:39Z",
        "dateModified": "2020-09-18T14:36:39Z"
    }
}


```

### Response example for a search within a group library using an tags.

GET https://api.zotero.org/groups/2392847/tags?v=3

Image shows a response example for a search within a group library using the specific "animals" tag on the Zotero web app.

![response example animals](https://media.github.ncsu.edu/user/18564/files/fd025200-fcfb-11ea-87f9-3fcf8a3d9e47)

```
[
    {
        "tag": "population viability analysis",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/population+viability+analysis",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/population+viability+analysis",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "butterflies",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/butterflies",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/butterflies",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "source-sink dynamics",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/source-sink+dynamics",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/source-sink+dynamics",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "demographic models",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/demographic+models",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/demographic+models",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "dispersal behaviour",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/dispersal+behaviour",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/dispersal+behaviour",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "Fender's blue",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/Fender%27s+blue",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/Fender%27s+blue",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "disturbance",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/disturbance",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/disturbance",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "succession",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/succession",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/succession",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "fire",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/fire",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/fire",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "mega-matrix",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/mega-matrix",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/mega-matrix",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "Patch",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/Patch",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/Patch",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "Alberta",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/Alberta",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/Alberta",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
        "tag": "Rocky Mountains",
        "links": {
            "self": {
                "href": "https://api.zotero.org/groups/2392847/tags/Rocky+Mountains",
                "type": "application/json"
            },
            "alternate": {
                "href": "http://zotero.org/groups/2392847/tags/Rocky+Mountains",
                "type": "text/html"
            }
        },
        "meta": {
            "type": 1,
            "numItems": 1
        }
    },
    {
```
### Response example for a search within a group library using a collection and a collection key.

$ curl https://api.zotero.org/groups/2392847/collections/2FICMUJM?v=3

Image shows what a response for collections for a specific group library will look like on the Zotero web app.

![collections](https://media.github.ncsu.edu/user/18564/files/8ade3d00-fcfc-11ea-94d9-10009be99961)

      C:\Users\das12>curl https://api.zotero.org/groups/2392847/collections/2FICMUJM?v=3
      {
          "key": "2FICMUJM",
          "version": 12,
          "library": {
              "type": "group",
              "id": 2392847,
              "name": "Introduction to Conservation Biology",
              "links": {
                  "alternate": {
                      "href": "https://www.zotero.org/groups/introduction_to_conservation_biology",
                      "type": "text/html"
                  }
              }
          },
          "links": {
              "self": {
                  "href": "https://api.zotero.org/groups/2392847/collections/2FICMUJM",
                  "type": "application/json"
              },
              "alternate": {
                  "href": "https://www.zotero.org/groups/introduction_to_conservation_biology/collections/2FICMUJM",
                  "type": "text/html"
              }
          },
          "meta": {
              "numCollections": 0,
              "numItems": 24
          },
          "data": {
              "key": "2FICMUJM",
              "version": 12,
              "name": "Butterflies",
              "parentCollection": false,
              "relations": {}
          }
      }
