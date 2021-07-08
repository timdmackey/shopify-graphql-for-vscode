# Overview

Basic skeleton for setting up the [VSCode GraphQL Extension](https://marketplace.visualstudio.com/items?itemName=GraphQL.vscode-graphql) and/or the [VSCode GraphiQL Explorer Extension](https://marketplace.visualstudio.com/items?itemName=GabrielNordeborn.vscode-graphiql-explorer) to work with Shopify.

# Schemas

Latest Shopify schemas are included as of ***2021-07-07***.

Shopify doesn't make their entire GraphQL schema available publicly, rather it is available to any registered app (public or private). The returned schema is scoped to the permissions that the requesting app has, so to make things easier, the schemas in this repository have been generated with all permissions turned on. If you wish to generate your own schemas, [follow the official instructions here](https://github.com/Shopify/bugbounty-resources/blob/master/graphql/main_guide.md#accessing-full-schemas).

# Configuration

***Note:** The schema files add a lot of cruft to your workspace, so you'll likely want to exclude them from search. To do this, open the command pallete* (<kbd>⇧</kbd>+<kbd>⌘</kbd>+<kbd>P</kbd> or <kbd>F1</kbd>) *and select `Preferences: Open Workspace Settings`. Find the setting for `Search: Exclude`, and add the `schemas` folder to the list. For example, if the `schemas` folder is at the root of your workspace, the exclude pattern would be **`schemas/`***.

The **GraphiQL Explorer extension** only needs the API schema, so the configuration file is very simple. Simply copy the `.graphqlconfig` and `schema.json` files to the root of your project directory, and update `schemaPath` to use the correct path:

#### **`.graphqlconfig`**

```json
{
  "schemaPath": "./schemas/shopify-api-schema@2021-07.json"
}
```

The **GraphQL extension** uses [graphql-config](https://graphql-config.com/usage) for its configuration. Your graphql configuration file must be in the root of your project folder, or at the very least, at or above the folder level of your current file. The file can be in either YAML (`graphql.config.yml`) or JSON (`graphql.config.json`) format and should look like the following:

#### **`graphql.config.yml`**
```yaml
schema: './schemas/schema@2021-07.graphql.json'
extensions:
  endpoints:
    default: 
      url: 'https://auroraplasmadesign.myshopify.com/admin/api/2021-07/graphql.json'
      headers:
        Content-Type: 'application/json'
        X-Shopify-Access-Token: 'shppa_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
```
#### **`graphql.config.json`**
```json
{
  "schema": "./schemas/schema@2021-07.graphql.json",
  "extensions": {
    "endpoints": {
      "default": {
        "url": "https://auroraplasmadesign.myshopify.com/admin/api/2021-07/graphql.json",
        "headers": {
          "Content-Type": "application/json",
          "X-Shopify-Access-Token": "shppa_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
        }
      }
    }
  }
}
```

If you only want IntelliSense/Autocomplete, you can delete everything except for the `schema` line.

If you'd like to make live queries and mutation from VSCode, you will need to [create a private app](https://help.shopify.com/en/manual/apps/private-apps) on your store, with permissions enabled for all the features you want to access via the GraphQL API. Once you've created your private app, you will need to include the `extensions` object with the relevant information for your shopify store:
1. The url at `extensions.endpoints.default.url` should use the myshopify domain of your own store, and the api version you wish to use.
2. The `X-Shopify-Access-Token` header can be found in your Private APP credentials, under the name “Password.”

# Supporting

If this was helpful to you, consider dropping me a tip! [☕️ **Buy Me a Coffee** 😊](https://www.buymeacoffee.com/timdmackey)