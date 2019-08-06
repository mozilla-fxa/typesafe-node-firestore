# typesafe-node-firestore

[![npm version](https://badge.fury.io/js/typesafe-node-firestore.svg)](https://badge.fury.io/js/typesafe-node-firestore)

A library of typescript interfaces that extend existing firestore classes, adding type safety and a better autocomplete experience.

This library is for use with `@google-cloud/firestore`, while the library its based on is intended for the browser-based Javascript
Firestore library.

Based on [typesafe-firestore](https://github.com/chaseholdren/typesafe-firestore).

## Installation

```shell
npm install typesafe-node-firestore --save-dev
```

## Usage

You most likely want to import `TypedCollectionReference` and create your collections as shown below.

```typescript
import * as firestore from "@google-cloud/firestore";
import { TypedCollectionReference } from "typesafe-node-firestore";

interface Author {
  penName: string;
  shortBiography: string;
  posts: string[];
}

// Create a new client
const firestore = new Firestore();

const AuthorCollection = firestore.collection(
  "authors"
) as TypedCollectionReference<Author>;
```

And then you can use your typesafe collection the same ways you would use the regular firestore library.

```typescript
// OK
AuthorCollection.add({
  penName: "",
  shortBiography: "",
  posts: []
});

// TS2322: Type 'number' is not assignable to type 'string'.
AuthorCollection.add({
  penName: 11,
  shortBiography: "",
  posts: []
});

// OK
AuthorCollection.where("penName", "<=", "Barfunk");

// TS2345: Argument of type '"realName"' is not assignable to parameter of type '"penName" | "shortBiography" | "posts" | FieldPath'.
AuthorCollection.where("realName", "<=", "Barfunk");
```

### License

typesafe-node-firestore is [MIT licensed](./LICENSE).
