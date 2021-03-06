# Getting Started / Authentication
To get started using the API, you'll need at least a developer API key. For some routes you will also need an access token for your account. Both of these are available [here](https://crabber.net/developer)

When making requests to routes that need your key or token, they need to be added as **query parameters**, as opposed to headers or part of the request body. An example URL would look like this:  
`https://crabber.net/api/v1/crabs/1/?api_key=abcdefghijklm&access_token=nopqrstuvwxyz`

# Models

## Crab
Represents a user on the site. Currently can't be patched outside of the bio
| Property      | Type    | Notes |
|---------------|---------|-------|
| avatar        | string  | Relative link from `crabber.net` |
| bio?          | object  | Only present when using `/crabs/:id/bio` |
| display_name  | string  |
| followers     | int     | Follower count |
| following     | int     | Following count |
| id            | int     | |
| register_time | int     | Unix timestamp |
| username      | string  | |
| verified      | boolean | |

### Crab bio
| Property  | Type    | Notes |
|-----------|---------|-------|
| age       | string  |       |
| emoji     | string  |       |
| jam       | string  |       |
| location  | string  |       |
| obsession | string  |       |
| pronouns  | string  |       |
| quote     | string  |       |
| remember  | string  |       |

### Crab list
| Property  | Type    | Notes |
|-----------|---------|-------|
| count     | int     | |
| crabs     | Crab[]  | |
| limit     | int     | |
| offset    | int     | |
| total     | int     | |

## Molt
Represents a post on the site
| Property      | Type     | Notes |
|---------------|----------|-------|
| author        | Crab     | Does not include bio |
| content       | string   | |
| crabtags      | string[] | Strings do not contain `%` |
| edited        | boolean  | |
| id            | int      | |
| image?        | string   | Relative to `crabber.net` |
| likes         | int      | Number of likes |
| mentions      | string[] | Contains usernames of mentioned users |
| quoted_molt?  | int      | Contains ID of quoted molt, or null |
| quotes        | int      | Number of quote molts |
| remolts       | int      | Number of remolts |
| replies       | int      | Number of replies |
| replying_to?  | int      | ID of molt replied to |
| timestamp     | int      | Unix timestamp |

### Molt list
| Property  | Type    | Notes |
|-----------|---------|-------|
| count     | int     | |
| limit     | int     | |
| molts     | Molt[]  | |
| offset    | int     | |
| total     | int     | |

# Endpoints
## Authentication
> ### GET /authenticate
Fetches the current authenticated crab using the provided access token

**Returns:** Crab

## Crabs
### Crab info
> ### GET /crabs/:id
Fetches a crab with the given ID

**Returns:** Crab

> ### GET /crabs/username/:name
Fetches a crab with the given username

**Returns:** Crab

> ### GET /crabs/:id/bio
Fetches a crab with their bio included

**Returns:** Crab with bio

> ### POST /crabs/:id/bio
Updates the bio of a crab, given the access token matches the account

**Body:** Should be form data containing any of the keys found in the bio model

**Returns:** Crab with bio

### Following and follower info
> ### POST /crabs/:id/follow
Follows a crab from the account linked to the given access token

**Returns:** plain text confirmation message

> ### POST /crabs/:id/unfollow
Unfollows a crab from the account linked to the given token

**Returns:** plain text confirmation message

> ### GET /crabs/:id/followers
Gets a list of crabs following the given crab

**Params:**
| name   | type   | description |
|--------|--------|-------------|
| limit  | int    | The number of crabs to fetch |
| offset | int    | Used for paginating the list |

**Returns:** Crab list

> ### GET /crabs/:id/following
Gets a list of crabs the given crab is following

**Params:**
| name   | type   | description |
|--------|--------|-------------|
| limit  | int    | The number of crabs to fetch |
| offset | int    | Used for paginating the list |

**Returns:** Crab list

### Molt-related
> ### GET /crabs/:id/molts
Fetches molts created by this crab

**Params:**
| name     | type   | description |
|----------|--------|-------------|
| limit    | int    | The number of crabs to fetch |
| offset   | int    | Used for paginating the list |
| since_id | int    | Molt ID; used to check for molts made after that one |

> **NOTE:** Remolts currently appear as molts with no content, using the ID of the original molt

**Returns:** Molt list

> ### GET /crabs/:id/bookmarks
Fetches the bookmarks of the given crab

**Params:**
| name     | type   | description |
|----------|--------|-------------|
| limit    | int    | The number of crabs to fetch |
| offset   | int    | Used for paginating the list |

**Returns:** Molt list

## Molts
### Managing molts
> ### POST /molts
Creates a new molt

**Body:** Should be form data containing the following:
| name     | type   | description |
|----------|--------|-------------|
| content  | string | Must be less than 280 characters |
| image    | file   | A file attachment of a valid image |
| source   | string | Defaults to "Crabber API" |

**Returns:** Molt

> ### GET /molts/:id
Fetches a specific molt

**Returns:** Molt

> ### DELETE /molts/:id
Deletes a specific molt

**Returns:** plain text confirmation message

> ### POST /molts/:id/edit
Edits the given molt.

> **NOTE:** This endpoint is only valid within the first 5 minutes after sending a molt

**Body:** Should be form data containing the following:
| name     | type   | description |
|----------|--------|-------------|
| content  | string | Must be less than 280 characters |
| image    | file   | A file attachment of a valid image |
| source   | string | Defaults to "Crabber API" |

**Returns:** Molt

### Interaction

> ### POST /molts/:id/quote
Quote-molts the given molt to the authorized account

**Body:** Should be form data containing the following:
| name     | type   | description |
|----------|--------|-------------|
| content  | string | Must be less than 280 characters |
| image    | file   | A file attachment of a valid image |
| source   | string | Defaults to "Crabber API" |

**Returns:** Molt

> ### POST /molts/:id/reply
Replies to the given molt from the authorized account

**Body:** Should be form data containing the following:
| name     | type   | description |
|----------|--------|-------------|
| content  | string | Must be less than 280 characters |
| image    | file   | A file attachment of a valid image |
| source   | string | Defaults to "Crabber API" |

**Returns:** Molt

> ### POST /molts/:id/remolt
Remolts the given molt to the authorized account

**Returns:** plain text confirmation message

> ### DELETE /molts/:id/remolt
Deletes a remolt made from the authorized account

**Returns:** plain text confirmation message

> ### POST /molts/:id/bookmark
Bookmarks the given molt

**Returns:** plain text confirmation message

> ### POST /molts/:id/unbookmark
Un-bookmarks the given molt

**Returns:** plain text confirmation message

> ### POST /molts/:id/like
Likes the given molt from the authorized account

**Returns:** plain text confirmation message

> ### POST /molts/:id/unlike
Unlikes the given molt from the authorized account

**Returns:** plain text confirmation message

> ### GET /molts/:id/replies
Fetches replies on a molt

**Params:**
| name     | type   | description |
|----------|--------|-------------|
| limit    | int    | The number of crabs to fetch |
| offset   | int    | Used for paginating the list |
| since_id | int    | Molt ID; used to check for molts made after that one |

**Returns:** Molt list

> ### GET /molts/:id/quotes
Fetches quote-molts on a molt

**Params:**
| name     | type   | description |
|----------|--------|-------------|
| limit    | int    | The number of crabs to fetch |
| offset   | int    | Used for paginating the list |
| since_id | int    | Molt ID; used to check for molts made after that one |

**Returns:** Molt list

### Querying
> ### GET /molts/mentioning/:name
Fetches molts mentioning a user with the given username

**Params:**
| name     | type   | description |
|----------|--------|-------------|
| limit    | int    | The number of crabs to fetch |
| offset   | int    | Used for paginating the list |
| since_id | int    | Molt ID; used to check for molts made after that one |

**Returns:** Molt list

> ### GET /molts/replying/:name
Fetches molts replying to a user with the given username

**Params:**
| name     | type   | description |
|----------|--------|-------------|
| limit    | int    | The number of crabs to fetch |
| offset   | int    | Used for paginating the list |
| since_id | int    | Molt ID; used to check for molts made after that one |

**Returns:** Molt list

> ### GET /crabtag/:tag
Fetches molts using the given crabtag

**Params:**
| name     | type   | description |
|----------|--------|-------------|
| limit    | int    | The number of crabs to fetch |
| offset   | int    | Used for paginating the list |
| since_id | int    | Molt ID; used to check for molts made after that one |

**Returns:** Molt list

> ### GET /timeline/:name
Fetches the timeline for a user with the given username

**Params:**
| name     | type   | description |
|----------|--------|-------------|
| limit    | int    | The number of crabs to fetch |
| offset   | int    | Used for paginating the list |
| since_id | int    | Molt ID; used to check for molts made after that one |

**Returns:** Molt list
