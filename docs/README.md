# Routes  
  
::: tip Base URL
`https://api.discord.bio/v1`
:::  
<PointOfInterest anchor="basicNotes"/>
::: tip  
All paths return JSON with a `success` key (a boolean) to determine if the operation failed or succeeded.  
If it succeeded, the `payload` key contains the requested config.  
All successful responses in this documentation are assumed to be inside `payload`.
:::

<PointOfInterest anchor="userObjects"/>

## User Details
<Route path="/userDetails/:slugOrId" method="get"/>

Fetch user details (but not connections).

::: details Path Parameters
|Parameter|Explanation|
|-|-|
|`slugOrId`|discord.bio slug or Discord ID|
:::

::: details Response
|Key|Explanation|
|-|-|
|`settings.name`|Slug|
|`settings.user_id`|User ID, currently the same as on Discord|
|`settings.premium_status`|Premium status. C-style boolean (either `0` or `1`)|
|`settings.verified`|Whether the user is verified on discord.bio. C-style boolean (either `0` or `1`)|
|`settings.upvotes`|Number of upvotes.|
|`settings.created_at`|Date and time of account creation.<br>Format: `yyyy-mm-ddThh:mm:SS.sssZ`, with T and Z always being as-is. Milliseconds currently appear to always be 000.|
|`settings.description`|Account description.|
|`settings.location`|Account location.|
|`settings.gender`|Account gender.<br>`null` is unspecified, `1` is male, `2` is female, and `3` is other.|
|`settings.birthday`|Account birthday. Same format as `settings.created_at`.|
|`settings.email`|Account email.|
|`settings.occupation`|Account occupation.|
|`settings.banner`|Account banner for premium users.|
|`discord.id`|Discord [ID](https://discordapp.com/developers/docs/reference#snowflakes), currently same as `settings.user_id`.|
|`discord.username`|Discord username.|
|`discord.avatar`|Discord [avatar hash](https://discordapp.com/developers/docs/reference#image-formatting-cdn-endpoints).|
|`discord.discriminator`|Discord [discriminator](https://discordapp.com/developers/docs/resources/user#user-object-user-structure).|
:::
::: details HTTP 401 Unauthorized
This means that not enough path parameters were passed.  
The `message` key of the JSON root will contain the following:  
```
You can only have zero parameters on this endpoint if you are logged in.
```
:::
::: details HTTP 404 Not Found
This means that the user passed via path parameters does not exist.  
The `message` key of the JSON root will contain the following:  
```
No data was found for this user.
```
:::

## Non-Discord connections
<Route path="/userConnections/:slugOrId" method="get"/>

Fetch user details (but not connections).
::: details Path Parameters
|Parameter|Explanation|
|-|-|
|`slugOrId`|discord.bio slug or Discord ID|
:::
::: details Response
|Key|Explanation|
|-|-|
|`github.name`|GitHub username|
|`website.name`|Website URL|
|`instagram.name`|Instagram handle|
|`snapchat.name`|Snapchat username|
|`linkedin.name`|LinkedIn username|
:::
::: details HTTP 202 Accepted
This means that the user does not exist. Note that `success` is still true and all keys above are still returned, but set to `null`.
:::

## Discord connections
<Route path="/discordConnections/:slugOrId" method="get"/>

::: tip 
Users can avoid displaying their connections here by hiding them in their user settings. 
:::
Fetch user details (but not connections).
::: details Path Parameters
|Parameter|Explanation|
|-|-|
|`slugOrId`|discord.bio slug or Discord ID|
:::
::: details Response
[As usual](#basicNotes), but `payload` is an array of connection objects, which are described in the following:  
|Key|Explanation|
|-|-|
|`id`|ID of the connected account, in the service's native ID format.|
|`connection-type`|Service that the connection comes from.|
|`name`|Username on the service.|
|`url`|Account page URL.|
|`icon`|CSS class name for the services icon via [Font Awesome Brands](https://fontawesome.com/icons?d=gallery&s=brands).|
:::
::: details HTTP 202 Accepted
`success` is still true, but the `payload` key is an empty array.  
This means that the user has no public connections, or the user does not exist.
:::  
  
## Total registered users
<Route path="/totalUsers" method="get"/>

::: details Response
`payload` is a JSON `number` type with the amount of registered discord.bio users.
:::

## Top users
<Route path="/topUpvoted" method="get"/>

::: warning
Despite the name, the route takes not only upvotes into account, but also the premium status of the user.
:::
::: details Response
`payload` is an array of shortened [user objects](#userObjects).  
  
Shortened user objects have `settings` renamed to `user`, an `id` key with the ranking in the root, and no `gender`, `email`, `occupation`, `banner`, `created_at`, `birthday`, `location`, `status` or `description` keys. 
:::