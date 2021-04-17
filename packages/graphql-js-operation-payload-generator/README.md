# graphql-js-operation-payload-generator

A graphql-js AST port of [Relay’s MockPayloadGenerator](https://relay.dev/docs/guides/testing-relay-components/#mock-payload-generator-and-the-relay_test_operation-directive).

```ts
import { graphql } from "@graphitation/graphql-js-tag";
import { generate } from "@graphitation/graphql-js-operation-payload-generator";
import { buildSchema } from "graphql";

const schema = buildSchema(/* ... */);

const query = graphql`
  query SomeQuery {
    user(id: "my-id") {
      ...SomeFragment
    }
  }

  fragment SomeFragment on User {
    id
    name
    profile_picture {
      uri
      width
      height
    }
  }
`;

const payload = generate(query, schema);
console.log(payload);
```

This will output something like the following:

```json
{
  "data": {
    "__typename": "Query",
    "node": {
      "__typename": "User",
      "id": "<mock-id-2>",
      "name": "<mock-value-for-field-\\"name\\">",
      "profile_picture": {
        "__typename": "Image",
        "uri": "<mock-value-for-field-\\"uri\\">",
        "width": 42,
        "height": 42
      }
    }
  }
}
```