# Other Features

Like a web browser, Other Chat is a User Agent. That is, an agent which acts on _behalf of the user_! This document describes how Other Chat's extensible platform (aka [Features](https://github.com/other-xyz/other.js.pseudo.js/blob/master/README.md)) operate on behalf of the user.

## User bill of rights

### The input box is sacred

It's our equivalent of the browser's URL bar. It lets the user know who they are, where they are and how to interact with the channel in a predictable way. Beyond changing a theme color, its elements may not be changed, resized or customized in any other way. It's always visible and nothing may draw over it (with the exception of fullscreen apps and media). The sidebar may fall into this same category, but that's still TBD.

### Never send a message on user's behalf

The "send" button is the _only_ way to send a message. All message modifications, insertions and completions stage the content for the user to send (current proposal: for media, selecting adds +1 to the send button; you add text in the normal input field; and you can select multiple types of media each with +1 to the send button).

Bots are bots and humans are humans. If a Feature posts on behalf of a user, they are clearly marked as coming from an Identity owned by that Feature. A renderer may choose how to display the Feature's identity and/or the user it acted on behalf of. (e.g. clicking on them reveals which Feature is posting. This function stacks as a word-of-mouth discovery mechanism for new Features).

### Maximize safety, minimize kingmakers

There are pros and cons to open, democratic models like the web vs closed, kingmaker models like app stores. We aim for a hybrid approach that capitalizes on the strengths while minimizing the downsides of each.

- All Features must abide by a ToS.
- All Features which utilize Other's servers require a verified developer account. This is our app store equivalent.
- All Features which utilize permission guarded capabilities must be run on Other's servers. It's possible we may allow users to override this similar to how macOS allows non-verified apps to be installed with user consent.
- A verified developer may host any Feature on Other's servers. There may be a fee structure. There may be reviews.
- Any Feature which uses only driveby permissions may be hosted on third-party servers. This is our open web equivalent.
- It is possible for third party channels to do aggregation of driveby safe Features (e.g. third party search engines).

## Applicability

Any Feature may be applied at four distinct levels: `channel`, `identity`, `account` and `builtin`.
- Where in conflict, they take precedence in that order. For example, a channel's color scheme overrides an identity's, which overrides an account's.
- Where compatible, they're additive. For example, an identity's word tokens are added to the builtin hotwords.

## Installation

Channel Features may only be installed by channel owners (ie. the creator and their delegates). Identity and Account Features are obviously installed by the identity/account owner. Builtin Features ship with the client (we'll likely come up w/ a scheme to update them more frequently than the client itself which supports A/B testing).

Installation may be initiated by:
  1. Clicking a link from within the client.
  1. Clicking a link from outside the client (e.g. a web browser) which deep links into the client.
  1. Attaching a Feature URL to a message or channel (e.g. a message may declare that a particular renderer should be installed to view it). However, all messages must be serializable to a primitive (mostly textual) representation. In some cases, this allows graceful degradation when the Feature isn't installed. In the most degenerate case, it just displays a link to install that Feature.

Features which don't require permissions are applied automatically, otherwise the user is prompted to grant its permissions. Channel Feature permissions are accepted by the channel owner. Their behaviors should be safe for channel visitors.

UI for determining the level to apply the Feature to is TBD.

## Capabilities

### Driveby

Driveby capabilites are those that, like the open web, are considered safe to apply without security or privacy considerations. They must not expose any information about the user and be idempotent with respect to the Chatternet. Features may use all of these capabilities without the need to prompt the user. If the user doesn't like the Feature, they may simply remove it.

- Provide simple, declarative themes (e.g primary color, accent color, sound set, icon set).
- Provide IME buttons. Builtin buttons are provided that hook device capabilities like camera roll, audio input, etc.
- Customize button slots in the IME bar (e.g. donger could be a first class citizen next to audio input, or giphy could just replace audio input).
- Suggest (ChatComplete)
- Render a message which the Feature itself sent
- Post messages to the channel with its own identity

### Conditionally permission guarded

Safe for driveby when combined with **only** the above "driveby" capabilities and **not** each other or the below "permission guarded" capabilities. Otherwise, permission guarded.

- Access the network
- Read channel messages

### Permission guarded

Capabilities which require user consent.

- Post messages to the channel with its own identity on behalf of the user
- Configure channel properties (e.g. who can post, readonly)
- Render a message list
- Render an arbitrary message
- Run in background on server (probably as Lambda)
- Run in background on client

### Examples

- A [Twitter feed](https://github.com/other-xyz/other.js.pseudo.js/blob/master/apps/twitter.pseudo.js) could be implemented as a Channel Feature which polls on the server and posts messages to the channel.
- A Starbucks ordering app could be implemented as a message list renderer which always displays an ordering UI above the message list (or perhaps doesn't even display the message list).
- An extended-local store. A la Instagram boutiques. Imagine an IME button that nav to #knifeman/store with a set of knives with buy buttons.
- [Rock, Paper, Scissors](https://github.com/other-xyz/other.js.pseudo.js/blob/master/extras/rock-paper-scissors.pseudo.js) could be implemented as a pure logic Feature, which requires another (probably builtin) Feature renderer that displays a customizable set of action buttons on a message. This has the advantage of lower barrier to entry through code reuse, greater UI consistency and potential for more performance implementation by using native widgets instead of web.
- TBD: [Moar, many moar](https://docs.google.com/document/d/1qhH74PRk9RdMLszV2PVMZniIgtpVxyG6e4-1fcA5Qmc/edit#)

## Development

All third party Features are implemented in JavaScript. Builtin Features may also be implemented in any native language in order to expose native capabilities, however, we try to implement as many in JS as possible in order to share code and eat our own dogfood.

We aim to make the lowest friction development environment possible. Everything should be view sourcable, forkable, hackable. Zero to hello world should be < 2 minutes. Designing such a system is out of scope of this document.

## TBD
- @aza: Features also apply at the message level and Feature level
- @aza: How do ChannelSets fit?
- @tonygentilcore: Explain Rechat via containers with references. Entities may then hang off of the referenced entity or the container.
- @tonygentilcore: Explain using references for content (like a Twitter feed) whose source of truth isn't in courier.