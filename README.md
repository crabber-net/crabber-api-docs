# Crabber API Docs
## Getting Started / Authentication
To get started using the API, you'll need at least a developer API key. For some routes you will also need an access token for your account. Both of these are available [here](https://crabber.net/developer)

When making requests to routes that need your key or token, they need to be added as **query parameters**, as opposed to headers or part of the request body. An example URL would look like this:  
`https://crabber.net/api/v1/crabs/1/?api_key=abcdefghijklm&access_token=nopqrstuvwxyz`

## Models

### Crab
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

### Molt
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

