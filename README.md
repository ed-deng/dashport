<p align="center">
  <img src="https://i.imgur.com/3FndGDl.png" alt="Dashport logo"/>
</p>

<h1 align="center">
  <a href='https://www.dashport.org/'>Dashport</a>
</h1>

_<h3 align="center">Authentication middleware for <a href='https://deno.land/x/dashport'>Deno</a></h3>_

<p align="center">
  </br>
  <a href="https://github.com/oslabs-beta/dashport/blob/main/LICENSE">
      <img alt="License" src="https://img.shields.io/github/license/oslabs-beta/dashport?color=light-blue">
  </a>
  <a href="https://github.com/oslabs-beta/dashport/issues">
      <img alt="Open issues" src="https://img.shields.io/github/issues-raw/oslabs-beta/dashport?color=yellow">
  </a>
  <a href="https://github.com/oslabs-beta/dashport/graphs/commit-activity">
      <img alt="Last commit" src=  "https://img.shields.io/github/last-commit/oslabs-beta/dashport?color=red">
  </a>
  <a href="https://github.com/oslabs-beta/dashport/stargazers">
      <img alt="GitHub stars" src="https://img.shields.io/github/stars/oslabs-beta/dashport?color=blue">
  </a>
</p>

# Features

- A Dashport class that handles authentication and serialization.
- A local strategy module.
- Strategy modules that allow developers to use third-party OAuth 2.0
  - [x] Google
  - [x] Facebook
  - [x] Github
  - [x] LinkedIn
  - [x] Spotify
- Written in TypeScript.

# Overview

<<<<<<< HEAD

# Dashport is a module that simplifies adding authentication middleware for Deno. Currently, the server framework being utilized is Oak, but Dashport has been modularized so that future frameworks can be easily added.

Dashport is a module that simplifies authentication in Deno. Currently, the server framework being utilized is Oak, but Dashport has been modularized so additional frameworks can be easily added as competitors to Oak appear.

> > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

Dashport was inspired by [Passport](http://www.passportjs.org/), the golden standard of authentication middleware for Node.js.

# Getting Started

<<<<<<< HEAD

# To get started, import Dashport. For easier modularity, Dashport can be imported into its own file for configurations.

To get started, import Dashport. For easier configuration, import Dashport into its own file.

> > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
import Dashport from "https://deno.land/x/dashport/mod.ts";
```

<<<<<<< HEAD
When Dashport is instantiated, pass in the server framework being used (Dashport currently only supports Oak). Then begin adding configurations for a serializer, deserializer, and strategy. Any errors returned from serializers and deserializers should be instances of 'Error'.
=======
Next, instantiate Dashport, passing in the name of the server framework being used (Dashport currently only supports Oak). Then begin adding configurations for a serializer, deserializer, and strategy. Any errors returned from serializers and deserializers should be instances of 'Error'.

> > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
// 'dashportconfig.ts' file

import Dashport from "https://deno.land/x/dashport/mod.ts";
import GoogleStrategy from "https://deno.land/x/dashport_google/mod.ts";

const dashport = new Dashport("oak");

dashport.addSerializer("serializer-1", (userInfo) => {
  const serializedId = Math.floor(Math.random() * 1000000000);
  userInfo.id = serializedId;

  try {
    const exampleUser = await exampleDbCreateUpsert(userInfo);
    return serializedId;
  } catch (err) {
    return err;
    // or return new Error(err);
  }
});

dashport.addDeserializer("deserializer-1", (serializedId) => {
  try {
    const exampleUserInfo = await exampleDbFind({ id: serializedId });
    return userInfo;
  } catch (err) {
    return err;
    // or return new Error(err);
  }
});

dashport.addStrategy(
  "goog",
  new GoogleStrategy({
    client_id: "client-id-here",
    redirect_uri: "redirect-uri-here",
    response_type: "response-type-here",
    scope: "scopes-wanted-here",
    client_secret: "client-secret-here",
    grant_type: "grant-type-here",
  })
);

export default dashport;
```

<<<<<<< HEAD
An initialization step for Dashport then needs to be added into the server file.

````typescript
import { Application, Router } from "https://deno.land/x/oak@v6.3.1/mod.ts";
import dashport from "./dashportconfig.ts";
=======
Dashport then needs to be initialized in the server file.

```typescript
import { Application, Router } from 'https://deno.land/x/oak/mod.ts';
import dashport from './dashportconfig.ts';
>>>>>>> b0d2a8c65c448bae8f1382fc99c6c601402e56aa

const app = new Application();
const router = new Router();

app.use(dashport.initialize);

app.use(router.routes());
app.use(router.allowedMethods());
````

<<<<<<< HEAD
After the initialization is added, Dashport is now set up to authenticate. Dashport's authenticate method acts as middleware, so if the framework is Oak, it can be used like below:
=======
Dashport is now ready to authenticate. Dashport's authenticate method acts as middleware, so it can be used like so with Oak:

> > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
router.get(
  "/privatepage",
  dashport.authenticate("goog"),
  async (ctx: any, next: any) => {
    ctx.body = "This is a private page!";
  }
);
```

<<<<<<< HEAD
After authentication, Dashport will have serialized an ID and manipulated user information based on the developer's defined serializer, and have created a session. In order to get the user information in another route, Dashport's deserialize property can be used as middleware. If the framework is Oak, deserialize will store either the user information or an Error on **ctx.locals** for the next middleware to access.
=======
After authentication, Dashport will have serialized the user information that was returned using the developer's defined serializer and created a session. In order to get the user information in another route, Dashport's deserialize property can be used as middleware. If the framework being used is Oak, deserialize will store either the user information or an Error on **ctx.locals** for the next middleware to access.

> > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
router.get(
  "/user-favorites",
  dashport.deserialize,
  async (ctx: any, next: any) => {
    const displayName = ctx.locals.displayName;
    ctx.body = `Welcome ${displayName}!`;
  }
);
```

<<<<<<< HEAD
In order to end a session, a log out button can be routed to an endpoint that uses Dashport's logOut property as middleware. If the framework is Oak, it can be used like below:
=======
In order to end a session, a log out button can be routed to an endpoint that calls Dashport's logOut property. Here's an example use with Oak:

> > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
router.get('/log-out',
  dashport.logOut,
  async (ctx: any, next: any) => {
    ctx.body = `You've logged out";
  }
)
```

# Methods

## initialize

- Functionality depends on the server framework that was passed in when instantiating Dashport.
  <<<<<<< HEAD
- # initialize is an async middleware function that creates a persistent Dashport object across multiple HTTP requests. For Oak, Dashport takes advantage of the persistent ctx.state and adds a Dashport key to it. This bypasses the need to do any monkey patching.
- Initialize is an asynchronous middleware function that creates a persistent Dashport object across multiple HTTP requests. For Oak, Dashport takes advantage of the persistent ctx.state and adds a Dashport key to it. This bypasses the need to do any monkey patching.
  > > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
// Oak example

import { Application, Router } from "https://deno.land/x/oak/mod.ts";
import Dashport from "https://deno.land/x/dashport/mod.ts";

const app = new Application();
const dashport = new Dashport("oak");

// use addSerializer, addDeserializer, and addStrategy, to add a serializer, deserializer, and strategy, to the Dashport instance

app.use(dashport.initialize);
```

## authenticate

- Functionality depends on the server framework that was passed in when instantiating Dashport.
  <<<<<<< HEAD
- # authenticate is the async middleware function that powers Dashport. It takes in one argument that is the name of the strategy to be used. Authenticate checks if a session exists. If a session does not exist, it begins the authentication process for the strategy specified. If the authentication is successful, the first serializer added by the developer will be
- Authenticate is the asynchronous middleware function that powers Dashport. It takes in one argument: the name of the strategy to be used. Authenticate first checks if a session exists. If a session does not exist, it begins the authentication process for the specified strategy.
  > > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
// Oak example

import { Application, Router } from "https://deno.land/x/oak/mod.ts";
import Dashport from "https://deno.land/x/dashport/mod.ts";

const app = new Application();
const router = new Router();
const dashport = new Dashport("oak");

// use addSerializer, addDeserializer, and addStrategy, to add a serializer, deserializer, and strategy, to the Dashport instance

app.use(dashport.initialize);

app.use(router.routes());
app.use(router.allowedMethods());

router.get(
  "/privatepage",
  dashport.authenticate("name-of-strategy-added"),
  async (ctx: any, next: any) => {
    ctx.body = "This is a private page!";
  }
);
```

## addSerializer

<<<<<<< HEAD

- # Dashport handles only authentication. If authentication is successful, it will pass the obtained user information to a serializer. It is up to the developer to define serializer functions that specify what to do with user information. User information will be passed in the form of Dashport's defined [AuthData](#authdata) interface.
- After a successful authentication, Dashport will pass the obtained user information to a serializer. It is up to the developer to define serializer functions that specify what to do with user information. User information will be passed in the form of Dashport's defined [AuthData](#authdata) interface.
  > > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa
- addSerializer is a function that takes two arguments. The first argument is the name a developer wants to call their serializer and the second argument is the serializer function. Serializer functions need to
  1. Accept one argument: the user data in the form of an object.
  2. Specify what the developer wants to do with the user data (store it in a database, add some info to response object, etc).
  3. Specify how to create a serialized ID.
  4. Return the serialized ID or an Error.

```typescript
dashport.addSerializer("serializer-1", (userInfo) => {
  const serializedId = Math.floor(Math.random() * 1000000000);
  userInfo.id = serializedId;

  try {
    const exampleUser = await exampleDbCreateUpsert(userInfo);
    return serializedId;
  } catch (err) {
    return err;
    // or return new Error(err);
  }
});
```

## removeSerializer

<<<<<<< HEAD

- # removeSerializer is a function that takes one argument. It will remove a serializer by name that was added.
- removeSerializer is a function that takes one argument: the name of the serializer to be removed.
  > > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
dashport.removeSerializer("serializer-1");
```

## addDeserializer

<<<<<<< HEAD

- # Deserializers are developer-defined functions that take in a serialized ID and return the user information that the developer wants to access in the next middleware. The shape will be how the developer decided to store it in their serializer function.
- Deserializers are developer-defined functions that take in a serialized ID and return the user information that the developer wants to access in the next middleware. The shape of the return value is up to the developer.
  > > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa
- addDeserializer is a function that takes two arguments. The first argument is the name a developer wants to call their deserializer and the second argument is the deserializer function. Deserializer functions need to
  1. Take in one argument: the serialized ID.
  2. Specify what the developer wants to do with the serialized ID to obtain user info (e.g. fetch the user info from a database).
  3. Return the user info or an Error.

```typescript
dashport.addDeserializer('deserializer-1', (serializedId) => {
  try {
    const userInfo = await exampleDbFind({ id: serializedId });
    return userInfo;
  catch(err) {
    return err;
    // or return new Error(err);
  }
})
```

## deserialize

- Functionality depends on the server framework that was passed in when instantiating Dashport.
  <<<<<<< HEAD
- # deserialize is an async middleware function that checks if a session exists. If a session exists, it checks if the session IDs match. If they do, it will execute the first deserializer added by the developer and store the user information for the next middleware to use. In Oak, if the deserialization is successful, the user information is stored on **ctx.locals**. If the deserialization is not successful, an Error will be stored on **ctx.locals**.
- deserialize is an asynchronous middleware function that checks if a session exists. If a session exists, it checks if the session IDs match. If they do, it will execute the first deserializer added by the developer and store the user information for the next middleware to use. In Oak, if the deserialization is successful, the user information is stored on **ctx.locals**. If the deserialization is not successful, an Error will be stored on **ctx.locals**.
  > > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
router.get(
  "/user-favorites",
  dashport.deserialize,
  async (ctx: any, next: any) => {
    const displayName = ctx.locals.displayName;
    ctx.body = `Welcome ${displayName}!`;
  }
);
```

## removeDeserializer

- removeDeserializer is a function that takes one argument. It will remove a deserializer by name that was added.

```typescript
dashport.removeDeserializer("deserializer-1");
```

## addStrategy

<<<<<<< HEAD

- Strategies are the variety of modules that can be imported with Dashport to allow third-party OAuth. They specify the logic for how to get authenticated and authorized to access user information from a third-party OAuth. By following only the few rules listed below, anyone can make a strategy.
- addStrategy is a function that takes two arguments. The first argument is the name the developer wants to call the strategy. The second argument is a new instance of the strategy with the options passed in. Strategy classes need to
  1. # Have a router method that ultimately returns an Error or the user information returned by the third-party OAuth in the form of Dashport's defined [AuthData](#authdata) interface. AuthData needs to have a **userInfo** property in the form of [UserProfile](#userprofile) and an optional **tokenData** property in the form of [TokenData](#tokendata).
- Strategies are the modules that can be imported with Dashport to allow third-party OAuth. They specify the authentication logic for the given OAuth service provider.
- addStrategy is a function that takes two arguments. The first argument is the name the developer wants to call the strategy. The second argument is a new instance of the strategy with its options passed in. Strategy classes need to:
  1. Have a router method that ultimately returns an Error or the user information returned by the third-party OAuth in the form of Dashport's defined [AuthData](#authdata) interface. AuthData needs to have a **userInfo** property in the form of [UserProfile](#userprofile) and a **tokenData** property in the form of [TokenData](#tokendata).
     > > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa
  2. Take in any options that are needed for the specific third-party OAuth to authenticate.

```typescript
dashport.addStrategy(
  "goog",
  new GoogleStrategy({
    client_id: "client-id-here",
    redirect_uri: "redirect-uri-here",
    response_type: "response-type-here",
    scope: "scopes-wanted-here",
    client_secret: "client-secret-here",
    grant_type: "grant-type-here",
  })
);
```

## removeStrategy

<<<<<<< HEAD

- # removeStrategy is a function that takes one parameter. It will remove a strategy by name that was added.
- removeStrategy is a function that takes one parameter: the name of the strategy to remove.
  > > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
dashport.removeStrategy("goog");
```

## logOut

- Functionality depends on the server framework that was passed in when instantiating Dashport.
  <<<<<<< HEAD
- # logOut is an async middleware function that ends a session. If a user logs out and logOut is used, the user will have to reauthenticate. If the framework is Oak, it can be used like below:
- logOut is an asynchronous middleware function that ends a session. If a user logs out and logOut is used, the user will have to reauthenticate. Here is an example use with Oak:
  > > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
router.get('/log-out',
  dashport.logOut,
  async (ctx: any, next: any) => {
    ctx.body = `You've logged out";
  }
)
```

# AuthData

<<<<<<< HEAD

# When a strategy successfully authenticates a user, the information given by the third-party provider should be returned in the form AuthData. The object should have an optional [tokenData](#tokendata) property and a required userInfo property in the form of [UserProfile](#userprofile). This contains the information for the [authenticate](#authenticate) method to use. The interface for AuthData is as below:

When a strategy successfully authenticates a user, the information given by the third-party provider should be returned on the AuthData object. This object should have a [tokenData](#tokendata) property and a userInfo property in the form of [UserProfile](#userprofile). This contains the information for the [authenticate](#authenticate) method to use. The interface for AuthData is:

> > > > > > > b0d2a8c65c448bae8f1382fc99c6c601402e56aa

```typescript
interface AuthData {
  tokenData: TokenData;
  userInfo: UserProfile;
}
```

# TokenData

Any relevant token data the developer wishes to receive from the third-party OAuth should be stored in an object with the below interface:

```typescript
interface TokenData {
  access_token: string;
  expires_in: number;
  scope: string;
  token_type: string;
  id_token: string;
}
```

# UserProfile

Since every OAuth provider returns information in different names and shapes, it is up to each strategy to conform the data returned into Dashport's defined UserProfile interface. This should contain all data the developer wishes to use.

```typescript
export interface UserProfile {
  provider: string;
  providerUserId: string;
  displayName?: string;
  name?: {
    familyName?: string;
    givenName?: string;
    middleName?: string;
  };
  emails?: Array<string>;
}
```

# Stretch Features

- Merge deserialize's functionality into authenticate.
- Currently only the first serializers and deserializers added are able to be used by Dashport. Add the option to use specific serializers and deserializers by name.
- Extend serializerization and deserializerization process to be able to use multiple serializers and deserializers.
- Add more strategies.
- Add support for other server frameworks.
