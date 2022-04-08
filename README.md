# The Threads API

The Threads API allows to post threads to forums via a JSON API.

## Authentication

You will be issued an api key and all requests to the API must include this key as a bearer token in the Authorization header e.g. `Authorization: Bearer <your_api_key>`.

You can create a new API key and find your existing keys at https://trythreads.com/apiKeys

## Ping endpoint

To test out authentication, you can use the ping endpoint. As an example, using curl:

`curl -H "Authorization: Bearer <your_api_key>" https://trythreads.com/api/public/ping`

Which should return a 200 with the name you gave your key. 

## Post Thread

You can create a thread by making a POST request to `https://trythreads.com/api/public/postThread`. You'll need to include the following in the body (as JSON. Make sure `Content-Type: application/json` is included in your request)

- `forumID (string)`. This is ID of the channel you want to post to. The API uses the word `forum` in place of `channel`. An easy way to find a `forumID` for a channel is to navigate to that channel on the Threads website. The URL will be `trythreads.com/<your_forum_id>`
- `body (Array<string>)`. These are the blocks of the thread body. To format a block, you can send markdown.

As an example, curl:

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://trythreads.com/api/public/postThread \
  -d '{"forumID":<your_forum_id>,"blocks":["# First Block", "Second block"]}'
```

## Delete Thread

You can delete any thread that the bot posted used the `deleteThread` endpoint. You just pass in the ThreadID that you got from the `postThread` response. Alternatively if you navigate to any thread on web, the URL is `https://trythreads.com/<thread_id>`

Example curl:

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://threads.com/api/public/deleteThread \
  -d '{"threadID":<thread_id>}'
```
