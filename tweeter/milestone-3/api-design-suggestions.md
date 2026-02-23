# M3 API Endpoints Design Suggestions

## Endpoint Design

1. **Pluralization/Singular Resources** - `/status/:id` or `/statuses/:id`? Pick one and stick with it. Don't mix and match plural/singular forms. Resource: https://stackoverflow.com/q/6845772
1. **GET Requests generally don't have request bodies** - It used to be "they should be ignored" in the RFC specifications, but now it's just uncommon. It is possible that GET requests with bodies may not be able to take advantage of cache/proxy layers. Resource: https://stackoverflow.com/q/978061
1. **URL parameters inadvertently create reserved words** - If you have two URLS: /users/feed and /users/:alias then suddenly you have created a reserved word. A user account that has the alias "feed" might be unable to access parts of your app, since it could fetch the wrong resource. The severity of this depends on which route has priority.
1. **Order of URL parameters** - /follows/:user1/:user2 might be ambiguous about who follows who. You can make this a little more clear by just rearranging the order of your parameters to /:user1/follows/:user2 which could be read out loud "user1 follows user2". This generally occurs when you have 2 URL parameters of the same type or 2 URL parameters next to each other.

## User Authentication

1. **Hashing should happen on the server** - Hashing on the client is NOT secure. At all. Resource: https://security.stackexchange.com/a/8600
1. **Salts stay on the server** - Salts should never be shared with the client. Resource: https://security.stackexchange.com/a/17435
1. **Auth tokens can be passed in many ways, but generally not in the request body (ITS EASIER TO DO IT THIS WAY THOUGH SO YOU CAN IGNORE THIS)** - You can pass auth tokens in requests in a bunch of ways. Request bodies are generally considered a bad place since GET requests shouldn't include a request body. Passing it in via the URL might also not be a good idea since those commonly show up in logs. Resource: https://stackoverflow.com/q/319530

## App Design

1. **Get all users endpoint** - is not needed.
1. **Token Refreshing** - If you wrote "the tokens expire after inactivity" then how do you keep track of inactivity? Do you send a new token to the client each time a request is made to the backend? If instead you meant "the tokens are valid for a period of 2 hours" then what happens when the tokens are invalid? Do you force logout or is there a background process which refreshes the token for you? Do you have endpoints for these actions?
1. **Pagination** - You should use cursors instead of page numbers since the DB implementation will make it very annoying to use page numbers.
