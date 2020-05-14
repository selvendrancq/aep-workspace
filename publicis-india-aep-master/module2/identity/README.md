## Exercise 2.2: Understanding Identity
In this exercise, the goal is to fully understand what Identity means, what ID-syncs are and why this is crucial to understand when talking about Adobe Experience Platform.

### Learning Objectives

- Understand Identity, ID-syncs and Device Coop
- Be able to reproduce the "Trees in the Forest"-whiteboard.
- Be able to articulate the concept of ID-syncs

### Lab Resources

- [Video & Whiteboard: Identity, ID-syncs and Device Coop](http://bit.ly/2DWorZe)

### Lab Tasks

- Query Adobe I/O to retrieve your own Identity and understand which ID's are part of your profile in the "La Boutique"-website context.

Let's first understand the basics by having a look at this video, which explains the concepts of Identity, ID-syncs and Device Coop.

[![Video & Whiteboard: Identity, ID-syncs and Device Coop](./images/video.png)](http://bit.ly/2DWorZe) 

Now that you have a basic understanding of Identity, Namespaces and ID's, let's review the configuration you did in Module 1.

#### Exercise 2.2.1 - Introduction to your Identifiers

First of all, you built a rule "All Authenticated Pages". This rule is activated when a customer is logged in.

The first action in this rule is to send a Beacon to Adobe Experience Platform. In the configuration of this beacon, you've told Launch to capture data elements like customerFirstName, customerEmail and customerMobileNr and send the values of those data elements to Platform. Below is a sample of such a call, which contains a number of values, but also a number of identities.

You can find this call yourself on your website by going into Chrome and opening the Chrome Developer Tools, just like you did in Module 1, Exercise 1.2.8. If you need help, go back to [Exercise 1.2.8](../../module1/launch/ex8.md).

```
{
  "header": {
    "datasetId": "5d635b98098d0a16354f497a",
    "imsOrgId": "1F6324765762BE0E7F000101@AdobeOrg",
    "source": {
      "name": "vangeluw Launch"
    },
    "schemaRef": {
      "id": "https://ns.adobe.com/publicisemeanorthpartnersand/schemas/c691f72dfa4b68409788214831161f94",
      "contentType": "application/vnd.adobe.xed-full+json;version=1"
    }
  },
  "body": {
    "xdmMeta": {
      "schemaRef": {
        "id": "https://ns.adobe.com/publicisemeanorthpartnersand/schemas/c691f72dfa4b68409788214831161f94",
        "contentType": "application/vnd.adobe.xed-full+json;version=1"
      }
    },
    "xdmEntity": {
      "_repo": {
        "createDate": "2019-09-03T04:33:48Z"
      },
      "person": {
        "name": {
          "lastName": "Van Geluwe",
          "firstName": "Wouter"
        },
        "gender": "male",
        "birthDate": "1982-01-01"
      },
      "homeAddress": {
        "city": "Waregem",
        "country": "Belgium",
        "street1": "Bosstraat 32",
        "postalCode": "8790"
      },
      "identityMap": {
        "ECID": [
          {
            "id": "12985050058071343400593524106372419809",
            "authenticatedState": "authenticated"
          }
        ],
        "Email": [
          {
            "id": "woutervangeluwe+03092019-1@gmail.com",
            "authenticatedState": "authenticated"
          }
        ],
        "Phone": [
          {
            "id": "+32473622044+03092019-1",
            "authenticatedState": "authenticated"
          }
        ]
      },
      "mobilePhone": {
        "number": "+32473622044+03092019-1"
      },
      "personalEmail": {
        "address": "woutervangeluwe+03092019-1@gmail.com"
      },
      "profilePictureLink": "http://s7e4a.scene7.com/is/image/OmniPS/adobelogo?$fmt=alpha-png",
      "_publicisemeanorthpartnersand": {
        "retailSizes": {
          "shoeSize": "43",
          "shirtSize": "L",
          "preferredColor": "black"
        },
        "identification": {
          "emailId": "woutervangeluwe+03092019-1@gmail.com"
        }
      }
    }
  }
}
```

Let's zoom in on the identities-part of this call.

```
 "identityMap": {
        "ECID": [
          {
            "id": "12985050058071343400593524106372419809",
            "authenticatedState": "authenticated"
          }
        ],
        "Email": [
          {
            "id": "woutervangeluwe+03092019-1@gmail.com",
            "authenticatedState": "authenticated"
          }
        ],
        "Phone": [
          {
            "id": "+32473622044+03092019-1",
            "authenticatedState": "authenticated"
          }
        ]
      }
```
and
```
"_publicisemeanorthpartnersand": {
        "identification": {
          "emailId": "woutervangeluwe+03092019-1@gmail.com"
        }
```

In the identities-property, we can see 3 Identity Types and ID's are being used:

| Identity Type             | ID |
|:---------------------:|:--|
| **ecid**              |12985050058071343400593524106372419809 |
| **email**             |woutervangeluwe+03092019-1@gmail.com|
| **phone**          |+32473622044+03092019-1|

These are the branches of the tree. Since these 3 ID's have been part of the same call to Platform, they are synced to eachother.

With the above Namespace and ID's in mind, let's have a look at your own identity in Platform, based on the interactions you've had with the "La Boutique"-website.

You need to know your values for these namespaces. To find these values, the easiest thing to do is to go onto the X-ray panel, which will show you all available ID's.

![Adobe IO New Integration](./images/xrayids.png)

With these ID's in mind, go to Postman.

![Adobe IO New Integration](../prerequisites/images/postmanui.png)

Before sending a request to Platform, you need to be properly authenticated. To be authenticated, you need to request an access token.

#### Exercise 2.2.2 - Postman authentication to Adobe I/O

If you're already authenticated and have a valid access_token/bearer token, you can continue directly to "Exercise 2.2.3 - Unified Profile API".

Make sure that you've got the right Environment selected before executing any call. You can check the currently selected Environment by verifying the Environment-dropdown list in the top right corner. 

The selected Environment should be "_Publicis Platform Enablement".

![Adobe IO New Integration](../prerequisites/images/envselemea.png)

You need to load an external library that will take care of the encryption and decryption of communication. To load this library, you have to execute the call with the name ```INIT: Load Crypto Library for RS256```. Select this call in the \_Adobe I/O - Token collection and you'll see it displayed in the middle of your screen.

![Adobe IO New Integration](../prerequisites/images/iocoll.png)

![Adobe IO New Integration](../prerequisites/images/cryptolib.png)

Click the blue "Send"-button. After a couple of seconds, you should see a response displayed in the "Body" section of Postman:

![Adobe IO New Integration](../prerequisites/images/cryptoresponse.png)

With the crypto libray now loaded, we can authenticate to Adobe I/O.
In the \_Adobe I/O - Token collection, select the call with the name ```IMS: JWT Generate + Auth```. Again, you'll see the call details displayed in the middle of the screen.

![Adobe IO New Integration](../prerequisites/images/ioauth.png)

Click the blue "Send"-button. After a couple of seconds, you should see a response displayed in the "Body" section of Postman:

![Adobe IO New Integration](../prerequisites/images/ioauthresp.png)

If your configuration was successfull, you should see a similar response that contains the following information:

| Key     | Value     | 
|:-------------:| :---------------:| 
| token_type          | **bearer** |
| access_token    | **eyJ4NXUiOiJ...o6anVZORc0ZQ** | 
| expires_in          | **86399973** |

Adobe I/O has given us a 'bearer'-token, with a specific value (this very long access_token) and an expiration window.

The token that we've received is now valid for 24 hours. This means that in 24 hours, if you want to use Postman to authenticate to Adobe I/O, you will have to generate a new token by running this call again.

#### Exercise 2.2.3 - Unified Profile API, Schema: Profile

Now you can go ahead and send your first call to Platform's Unified Profile Service API's.

In Postman, locate the collection \_Publicis - Platform Enablement Start - JWT

![Adobe IO New Integration](./images/coll_enablement.png)

In "1. Unified Profile Service", select the first call with the name "UPS - GET Profile by Entity ID & NS".

![Adobe IO New Integration](./images/callecid.png)

For this call, there are 3 required variables:

| Key     | Value     | 
|:-------------:| :---------------:| 
| entityId          | **id** |
| entityIdNS    | **namespace** | 
| schema.name          | **\_xdm.context.profile** |

* entityId = the specific customer ID
* entityIdNS = the specific namespace that is applicable to the ID
* schema.name = the specific schema for which you want to receive information

So, if you want to ask Platform's API's to give you back all Profile information for your own ecid, you will need to configure the call as follows:

| Key     | Value     | 
|:-------------:| :---------------:| 
| entityId          | **yourECID** |
| entityIdNS    | **ecid** | 
| schema.name          | **\_xdm.context.profile** |

![Adobe IO New Integration](./images/callecid.png)

Click "Send" to send your request to Platform.

You should get an immediate response from Platform, showing you something like this:

![Adobe IO New Integration](./images/callecidresponse.png)

This is the full response from Platform:

```
{
    "A2-EZjNKbDcFEuGQ9bUHnzwI": {
        "entityId": "A2-EZjNKbDcFEuGQ9bUHnzwI",
        "mergePolicy": {
            "id": "89bb59d0-52bb-4e6e-b5c8-c4acfdd29b8d"
        },
        "sources": [
            "",
            "5d635b98098d0a16354f497a"
        ],
        "tags": [
            "",
            "1567485228943:2562:0"
        ],
        "identityGraph": [
            "A2-EZjNKbDcFEuGQ9bUHnzwI",
            "BUF9zMKLrXq72p4HpbsHv1SSs8THLTFAZ21haWwuY29t",
            "BkFtK4QcJpSPByuSs8THLTE"
        ],
        "entity": {
            "personalEmail": {
                "address": "woutervangeluwe+03092019-1@gmail.com"
            },
            "_repo": {
                "createDate": "2019-09-03T04:33:48Z"
            },
            "homeAddress": {
                "country": "Belgium",
                "city": "Waregem",
                "postalCode": "8790",
                "street1": "Bosstraat 32"
            },
            "mobilePhone": {
                "number": "+32473622044+03092019-1"
            },
            "_publicisemeanorthpartnersand": {
                "identification": {
                    "emailId": "woutervangeluwe+03092019-1@gmail.com"
                },
                "retailSizes": {
                    "shoeSize": "43",
                    "shirtSize": "L",
                    "preferredColor": "black"
                }
            },
            "person": {
                "gender": "male",
                "name": {
                    "lastName": "Van Geluwe",
                    "firstName": "Wouter"
                },
                "birthDate": "1982-01-01"
            },
            "profilePictureLink": "http://s7e4a.scene7.com/is/image/OmniPS/adobelogo?$fmt=alpha-png",
            "identityMap": {
                "ecid": [
                    {
                        "id": "12985050058071343400593524106372419809"
                    }
                ],
                "email": [
                    {
                        "id": "woutervangeluwe+03092019-1@gmail.com"
                    }
                ],
                "phone": [
                    {
                        "id": "+32473622044+03092019-1"
                    }
                ]
            }
        },
        "lastModifiedAt": "2019-09-03T04:33:49Z"
    }
}
```
This is currently all of the available Profile data in Platform for this ECID.
You're not required to use the ECID to request Profile data from Platform's Unified Profile, you can use any ID in any namespace to request this data. 

Let's go back to Postman and pretend we're the call center, and send a call to Platform specifying the namespace of **phone** and your mobile number.

So, if you want to ask Platform's API's to give you back all Profile information for a specific mobilenr, you will need to configure the call as follows:

| Key     | Value     | 
|:-------------:| :---------------:| 
| entityId          | **yourmobilenr** |
| entityIdNS    | **phone** \(replace ecid with phone) | 
| schema.name          | **\_xdm.context.profile** |

If your phone number contains a ``+`` - symbol, don't forget to select your phone number completely, then right-click and select ``EncodeURIComponent``.

![Adobe IO New Integration](./images/encode.png)

![Adobe IO New Integration](./images/callmobilenr.png)

Click the blue "Send"-button and verify the response.

![Adobe IO New Integration](./images/callmobilenrresponse.png)

Let's do the same thing for your email ID by specifying the namespace of **email** and your email ID.

So, if you want to ask Platform's API's to give you back all Profile information for a specific email ID, you will need to configure the call as follows:

| Key     | Value     | 
|:-------------:| :---------------:| 
| entityId          | **youremail** |
| entityIdNS    | **email** \(replace phone with email) | 
| schema.name          | **\_xdm.context.profile** |

If your email address contains a ``+`` - symbol, don't forget to select your email address completely, then right-click and select ``EncodeURIComponent``.

![Adobe IO New Integration](./images/encodeemail.png)

![Adobe IO New Integration](./images/callemail.png)

Click the blue "Send"-button and verify the response.

![Adobe IO New Integration](./images/callemailresponse.png)

This is a very important kind of flexibility that is offered to brands. This means that any environment can send a request to Platform, using their own ID and namespace, without having to understand the complexity of multiple namespaces and ID's.

As an example:

  * the Call Center will request data from Platform using the namespace "phone"
  * the Loyalty System will request data from Platform using the namespace "email"
  * online applications might use the namespace "ecid"

The Call Center doesn't necessarily know what kind of identifier is used in the Loyalty System and the Loyalty System doesn't necessarily know what kind of identifier is used by online applications. Each individual system can use the information that they have and understand to get the information they need, when they need it.

#### Exercise 2.2.4 - Unified Profile API, Schema: Profile and ExperienceEvent

After having queried Platform's API's successfully for Profile data, let's now do the same with ExperienceEvent data.

In Postman, locate the collection \_Publicis - Platform Enablement Start - JWT

![Adobe IO New Integration](./images/coll_enablement.png)

In "1. Unified Profile Service", select the second call with the name "UPS - GET Profile & EE by Entity ID & NS".

![Adobe IO New Integration](./images/upseecall.png)

For this call, there are 4 required variables:

| Key     | Value     | 
|:-------------:| :---------------:| 
| schema.name          | **\_xdm.context.experienceevent** |
| relatedSchema.name          | **\_xdm.context.profile** |
| relatedEntityId          | **id** |
| relatedEntityIdNS    | **namespace** | 

* schema.name = the specific schema for which you want to receive information. In this case, we're looking for data that is mapped against the ExperienceEvent schema. 
* relatedSchema.name = While we're looking for data that is mapped against the ExperienceEvent schema, we need to specify an identity for which we want to receive that data. The schema that has access to identity is the Profile-schema, so the relatedSchema here is the Profile-schema.
* relatedEntityId = the specific customer ID
* relatedEntityIdNS = the specific namespace that is applicable to the ID


So, if you want to ask Platform's API's to give you back all Profile information for your own ecid, you will need to configure the call as follows:

| Key     | Value     | 
|:-------------:| :---------------:| 
| schema.name          | **\_xdm.context.experienceevent** |
| relatedSchema.name          | **\_xdm.context.profile** |
| relatedEntityId          | **yourECID** |
| relatedEntityIdNS    | **ecid** | 

![Adobe IO New Integration](./images/eecallecid.png)

Click "Send" to send your request to Platform.

You should get an immediate response from Platform, showing you something like this:

![Adobe IO New Integration](./images/eecallecidresponse.png)

Below is the full response from Platform. In this example, there are 8 ExperienceEvents linked to this customer's ECID. Have a look at the below to see the different variables on the call, as what you see below is the direct consequence of your configuration in Launch in Modules 1 and 2.

Also, when the X-ray panel shows ExperienceEvent information, it is using the below payload to parse and retrieve the information like Product Name (search for productName in the below payload) and Product Image URL (search for productUrl in the below payload).

```
{
    "_page": {
        "orderby": "timestamp",
        "start": "6004574439930.821",
        "count": 7,
        "next": ""
    },
    "children": [
        {
            "relatedEntityId": "A2-EZjNKbDcFEuGQ9bUHnzwI",
            "entityId": "6004574439930.821",
            "timestamp": 1567485069000,
            "entity": {
                "environment": {
                    "browserDetails": {
                        "acceptLanguage": "en",
                        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
                    }
                },
                "web": {
                    "webPageDetails": {
                        "name": "Admin"
                    }
                },
                "identityMap": {
                    "ECID": [
                        {
                            "id": "12985050058071343400593524106372419809"
                        }
                    ]
                },
                "_publicisemeanorthpartnersand": {
                    "identification": {
                        "ecid": "12985050058071343400593524106372419809"
                    }
                },
                "_id": "6004574439930.821",
                "timestamp": "2019-09-03T04:31:09Z"
            },
            "lastModifiedAt": "2019-09-03T04:31:09Z"
        },
        {
            "relatedEntityId": "A2-EZjNKbDcFEuGQ9bUHnzwI",
            "entityId": "1231448128529.3574",
            "timestamp": 1567485082000,
            "entity": {
                "environment": {
                    "browserDetails": {
                        "acceptLanguage": "en",
                        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
                    }
                },
                "web": {
                    "webPageDetails": {
                        "name": "La Boutique Home"
                    }
                },
                "identityMap": {
                    "ECID": [
                        {
                            "id": "12985050058071343400593524106372419809"
                        }
                    ]
                },
                "_publicisemeanorthpartnersand": {
                    "identification": {
                        "ecid": "12985050058071343400593524106372419809"
                    }
                },
                "_id": "1231448128529.3574",
                "timestamp": "2019-09-03T04:31:22Z"
            },
            "lastModifiedAt": "2019-09-03T04:31:23Z"
        },
        {
            "relatedEntityId": "A2-EZjNKbDcFEuGQ9bUHnzwI",
            "entityId": "8250800865648.47",
            "timestamp": 1567485097000,
            "entity": {
                "environment": {
                    "browserDetails": {
                        "acceptLanguage": "en",
                        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
                    }
                },
                "web": {
                    "webPageDetails": {
                        "name": "La Boutique Home"
                    }
                },
                "identityMap": {
                    "ECID": [
                        {
                            "id": "12985050058071343400593524106372419809"
                        }
                    ]
                },
                "_publicisemeanorthpartnersand": {
                    "identification": {
                        "ecid": "12985050058071343400593524106372419809"
                    }
                },
                "_id": "8250800865648.47",
                "timestamp": "2019-09-03T04:31:37Z"
            },
            "lastModifiedAt": "2019-09-03T04:31:40Z"
        },
        {
            "relatedEntityId": "A2-EZjNKbDcFEuGQ9bUHnzwI",
            "entityId": "4480828225306.617",
            "timestamp": 1567485185000,
            "entity": {
                "environment": {
                    "browserDetails": {
                        "acceptLanguage": "en",
                        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
                    }
                },
                "web": {
                    "webPageDetails": {
                        "name": "Register"
                    }
                },
                "identityMap": {
                    "ECID": [
                        {
                            "id": "12985050058071343400593524106372419809"
                        }
                    ]
                },
                "_publicisemeanorthpartnersand": {
                    "identification": {
                        "ecid": "12985050058071343400593524106372419809"
                    }
                },
                "_id": "4480828225306.617",
                "timestamp": "2019-09-03T04:33:05Z"
            },
            "lastModifiedAt": "2019-09-03T04:33:06Z"
        },
        {
            "relatedEntityId": "A2-EZjNKbDcFEuGQ9bUHnzwI",
            "entityId": "3341812235559.787",
            "timestamp": 1567485218000,
            "entity": {
                "environment": {
                    "browserDetails": {
                        "acceptLanguage": "en",
                        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
                    }
                },
                "web": {
                    "webPageDetails": {
                        "name": "La Boutique Home"
                    }
                },
                "identityMap": {
                    "ECID": [
                        {
                            "id": "12985050058071343400593524106372419809"
                        }
                    ]
                },
                "_publicisemeanorthpartnersand": {
                    "identification": {
                        "ecid": "12985050058071343400593524106372419809"
                    }
                },
                "_id": "3341812235559.787",
                "timestamp": "2019-09-03T04:33:38Z"
            },
            "lastModifiedAt": "2019-09-03T04:33:39Z"
        },
        {
            "relatedEntityId": "A2-EZjNKbDcFEuGQ9bUHnzwI",
            "entityId": "4943869542065.549",
            "timestamp": 1567485221000,
            "entity": {
                "environment": {
                    "browserDetails": {
                        "acceptLanguage": "en",
                        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
                    }
                },
                "web": {
                    "webPageDetails": {
                        "name": "La Boutique Home"
                    }
                },
                "identityMap": {
                    "ECID": [
                        {
                            "id": "12985050058071343400593524106372419809"
                        }
                    ]
                },
                "_publicisemeanorthpartnersand": {
                    "identification": {
                        "ecid": "12985050058071343400593524106372419809"
                    }
                },
                "_id": "4943869542065.549",
                "timestamp": "2019-09-03T04:33:41Z"
            },
            "lastModifiedAt": "2019-09-03T04:33:41Z"
        },
        {
            "relatedEntityId": "A2-EZjNKbDcFEuGQ9bUHnzwI",
            "entityId": "7500838426711.108",
            "timestamp": 1567485228000,
            "entity": {
                "environment": {
                    "browserDetails": {
                        "acceptLanguage": "en",
                        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
                    }
                },
                "web": {
                    "webPageDetails": {
                        "name": "La Boutique Home"
                    }
                },
                "identityMap": {
                    "ECID": [
                        {
                            "id": "12985050058071343400593524106372419809"
                        }
                    ]
                },
                "_publicisemeanorthpartnersand": {
                    "identification": {
                        "ecid": "12985050058071343400593524106372419809"
                    }
                },
                "_id": "7500838426711.108",
                "timestamp": "2019-09-03T04:33:48Z"
            },
            "lastModifiedAt": "2019-09-03T04:33:49Z"
        }
    ],
    "_links": {
        "next": {
            "href": ""
        }
    }
}
```

This is currently all of the available ExperienceEvent data in Platform for this ECID.
You're not required to use the ECID to request ExperienceEvent data from Platform's Unified Profile, you can use any ID in any namespace to request this data. 

#### Exercise 2.2.5 - Unified Profile View of the Customer's Golden Record


In Platform there's a new feature that visualizes the entore customer profile. This one feature is what all of our customer's have been trying to get for years: a single view of the customer.

Go to the Platform IU: [https://platform.adobe.com/home](https://platform.adobe.com/home).

In the Platform UI, go to "Profiles".

![Adobe IO New Integration](./images/profiles.png)

By going to the new UI of Platform (not available in Production yet - link will be placed at a later stage), you'll be able to go to "Profiles" and search for a Profile.

![Adobe IO New Integration](./images/findaprofile.png)

By clicking on "Find a Profile", a popup appears in which a namespace and an ID can be entered. 

![Adobe IO New Integration](./images/popup.png)

In this case, I'm taking my ECID (58869277403188560570697962446749491646), but any other namespace and ID can also be used to retrieve a profile here.

![Adobe IO New Integration](./images/popupecid.png)

By clicking OK, I'll be seeing my full profile.

![Adobe IO New Integration](./images/profile.png)

And by going to the menu option "Experience Events", all of my Experience Events are being shown.

![Adobe IO New Integration](./images/ee.png)

In this single view of the customer, all profile data is shown alongside behavioral and transactional data and the view will also be enriched with existing segment memberships. The data that is shown here come from anywhere, from any Adobe Solution to any external solution. This is the most powerful view of Adobe Experience Platform: the true Experience System of Record.

Congrats for making it all the way here. It's now time to experience Platform's new, unified segmentation environment!

[Next Step: Using the new, Unified Segmentation Experience](../segmentation/README.md)

[Go Back to Module 2](../README.md)

[Go Back to All Modules](/../../)



