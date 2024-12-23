# Spec-dts

Spec-dts enables inferring the types from an OpenAPI spec without any code generation – just good old TypeScript type inference.

```sh
npm i spec-dts
```

## Features

- [x] Infer OpenAPI types from a JSON spec
- [x] No code generation
- [x] Simple API and usage

## Example Usage

```ts
import { ParseSpec } from "spec-dts";

// Import from file
import type { Spec } from "spec.d.ts

// Or:
type Spec = {
    // JSON spec here
};

type Api = ParseSpec<Spec>;

type GetPostsResult = Api["GET /posts"]["Output"]
```

## Table of Contents

- [Why?](#why)
- [How?](#how)
- [API Reference](#api-reference)
  - [`ParseSpec<T>`](#parsespect)
  - [`Parse[Part]<T>`](#parsext)

## Why?

When consuming APIs that offer an OpenAPI spec, we're forced to use bloated code generators which rarely fit our needs and frequently get in the way, undermining the utility of OpenAPI specs. Spec-dts offers a lightweight, types-only approach for utilising contracts defined in OpenAPI specs.

## How?

By representing an OpenAPI spec in `.d.ts` form, we can run type inference on it directly, without any code generation.

## API Reference

### `ParseSpec<T>`

> Derive an API definition from an OpenAPI Spec

We can directly infer the types of a JSON OpenAPI spec using the `ParseSpec<T>` type utility.

```ts
type API = ParseSpec<{
  // OpenAPI JSON
}>;

type GetPostsOperation = API["GET /posts"];
// { Query, Params, Headers, Body, Output }
```

We can directly paste in the OpenAPI contents here, considering JSON constitutes a valid TypeScript type.

#### More Scalable Approach

Manually pasting the document will suffice for many applications, but if we want to make this more automatic, we can simply fetch our OpenAPI document and save it in a `spec.d.ts` file (or any other name).

A simple unix shell script to do this could look like:

```sh
curl -s "<your-openapi-url>" |\ # Get openapi spec
 	sed 's/`/\\`/g' |\ # Escape backticks
  awk '{print "export type Spec = `"$0"`;"}' >> spec.d.ts # Save to file
```

### `Parse[Part]<T>`

On top of parsing the entire spec, we can parse individual parts, like schema, params or operations. These are the internally used types used to parse the spec as a whole.

```ts
import {
  // Schema
  ParseSchema,
  ParseSchemaObject,
  ParseSchemaReference,

  // Primitives
  ParseObject,
  ParseArray,
  ParseString,
  ParseBoolean,
  ParseNumber,
  ParseNull,
  ParseUnion,
  ParseIntersection,

  // Parameters
  ParseParameter,
  ParseParameters,
  ParseParameterReference,
  ParseParameterObject,

  // Body/Response
  ParseBody,
  ParseResponse,
  ParseResponses,
  ParseBodyOrResponseReference,
  ParseBodyOrResponseObject,

  // Operations
  ParseOperation,
  ParsePath,
} from "schema-dts";
```
