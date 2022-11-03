# The Threads API

The Threads API allows you to post threads to channels via a JSON API.

## Authentication

You can create a new API key and find your existing keys at https://threads.com/apiKeys

When you create a new API key, we create a new Bot user for the key. The Bot user has the same posting privileges as any member in your organization.

All requests to the API must include this key as a bearer token in the Authorization header e.g. `Authorization: Bearer <your_api_key>`.

## Ping endpoint

To test out authentication, you can use the ping endpoint. As an example, using curl:

`curl -H "Authorization: Bearer <your_api_key>" https://threads.com/api/public/ping`

Which should return a 200 with the name you gave your key. 

## Post Thread

You can post a thread to a channel by making POST requests to `https://threads.com/api/public/postThread`. You'll need to include the following in the body as JSON. Make sure `Content-Type: application/json` is included in your request.

### Required Arguments:

#### 1. 
`channel (string)`. This is the name of the channel you want to post to.

  OR

`channelID (string)`. This is ID of the channel you want to post to.  An easy way to find a channelID for a channel is to navigate to that channel on the Threads website. The URL will be `threads.com/<your_channel_id>`

#### 2. 
`blocks (Array<string>)`. These are the blocks of the thread body. To format a block, you can send markdown.

#### Examples:

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://threads.com/api/public/postThread \
  -d '{"channel":<your_channel_name>,"blocks":["# First Block", "Second block"]}'
```

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://threads.com/api/public/postThread \
  -d '{"channelID":<your_channel_id>,"blocks":["# First Block", "Second block"]}'
```

## Delete Thread

You can delete any thread that the bot posted using the `deleteThread` endpoint. You just pass in the `threadID` that you got from the `postThread` response. Alternatively if you navigate to any thread on web, the URL is `https://threads.com/<thread_id>`

### Required Arguments:

#### 1.
`threadID (string)`. This is ID of the thread you want to delete

#### Example:

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://threads.com/api/public/deleteThread \
  -d '{"threadID":<thread_id>}'
```

## List your channels

You can list the channels visible to your Bot user. These include public channels in your organization.

Example curl:

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://threads.com/api/public/channels
```

## Post Chat Message

You can post a message to a chat by making POST requests to `https://threads.com/api/public/postChatMessage`. You'll need to include the following in the body as JSON. Make sure `Content-Type: application/json` is included in your request.

### Required Arguments:

#### 1.
`chat (string)`. This is the name of the chat you want to post to.

OR

`chatID (string)`. This is ID of the chat you want to post to.  An easy way to find a chatID for a chat is to navigate to that chat on the Threads website. The URL will be `threads.com/messages/<your_chat_id>`

#### 2.
`body (string)`. This is the body of the message. To format the body, you can send markdown.

#### Examples:

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://threads.com/api/public/postChatMessage \
  -d '{"chat":<your_chat_name>,"body":"# My Message"}'
```

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://threads.com/api/public/postChatMessage \
  -d '{"chatID":<your_chat_id>,"body":"# My Message"}'
```

## Delete Chat Message

You can delete any message that the bot posted using the `deleteChatMessage` endpoint. You just pass in the `messageID` that you got from the `postChatMessage` response. Alternatively if you navigate to any thread on web, the URL is `https://threads.com/<your_chat_id>?messageID=<your_message_id>`

### Required Arguments:

#### 1.
`messageID (string)`. This is ID of the message you want to delete

#### Example:

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://threads.com/api/public/deleteChatMessage \
  -d '{"messageID":<your_message_id>}'
```


## Upload a file

You can upload a file which can be used in a subsequent `postThread` or `postChatMessage` request using the `uploadFile` endpoint.
Only uploading images and video mimetypes are allowed.

### Required Arguments:

#### 1.
`data`. The local location of the file 


#### Example:
```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: multipart/form-data" \
  -X POST https://threads.com/api/public/uploadFile \
  -F "data=@/path/to/your/file"  
```


## Markdown
We support some markdown for formatting your threads and chat messages. You can use the fileID from the uploadFile API to embedding an image or video file as a standalone block.

#### Examples:

```bash
curl \
  -H "Authorization: Bearer <your_api_key>" \
  -H "Content-Type: application/json" \
  -X POST https://threads.com/api/public/postThread \
  -d \
"
{
  \"channel\": \"<your_channel_name>\",
  \"blocks\": [
    \"embedded link https://twitter.com\",
    \"[named twitter link](https://twitter.com)\",
    \"*bold text*\",
    \"/italic/\",
    \"_underline_\",
    \"-strikethrough-\",
    \"\`inline code\`\",
    \"* list item1 \n * list item2\"
  ]
}
"
```

```bash
curl \
-H "Authorization: Bearer <your_api_key>" \
-H "Content-Type: application/json" \
-X POST https://threads.com/api/public/postThread \
-d '{"channel":<your_channel_name>,"blocks":["# My First Block", "embedded file below", "<!12345678910|>"]}'
```
