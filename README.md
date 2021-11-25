# sequelize-typescript-migration

This package is based on [sequelize-typescript](https://www.npmjs.com/package/sequelize-typescript), the TypeScript implementation package for Sequelize-based models. Originally this package was created by [kimjbstar](https://github.com/kimjbstar), but I decided to decouple its dependency to `sequelize-typescript` -- to prevent compile errors in my project, when this module was created.

`sequelize-typescript-migration` is based on [flexxnn's sequelize-auto-migrations](https://github.com/flexxnn/sequelize-auto-migrations) package (and its forks), but it's reworked to TypeScript.

## Prerequisites

To make the full use of this module, be sure that you have:
- [Sequelize ORM](https://www.npmjs.com/package/sequelize) installed in your project, and
- Basic knowledge of [how Sequelize migration works](https://sequelize.org/master/manual/migrations.html)

## How this works

This module scans models and its decorators to find changes, and then generates the migration code on-the-fly -- much like how `makemigration` works in Django framework. This way, you don't need to create migrations manually.

After the migration is generated, you can use `sequelize db:migrate` command -- just you would migrate as usual.

## Installation

Install with NPM:

```sh
npm i -D sequelize-typescript-migration
```

Or, with Yarn:

```sh
yarn add -D sequelize-typescript-migration
```

## Usage

Create a new TypeScript file, and use this template:

```ts
import { Sequelize } from "sequelize-typescript";
import { SequelizeTypescriptMigration } from "sequelize-typescript-migration";

const sequelize: Sequelize = new Sequelize({
  // Sequelize options here
});

await SequelizeTypescriptMigration.makeMigration(sequelize, {
  outDir: path.join(__dirname, "../migrations"),
  migrationName: "add-awesome-field-in-my-table"
  preview: false,
});
```

Then, compile with `tsc` command, along with the file name. The code above will generate Sequelize migrations into the migration path specified in `outDir` option. After the migration files are generated successfully, you can run the `sequelize db:migrate` command -- with the specified output, of course.

### Options

Required:
- `outDir` (string) - Output path for generated migration files

Optional:
- `migrationName` (string) - Name for generated migration files
- `preview` (boolean) - Similar to dry-run operation -- no files are generated if this is set to `true`
- `debug` (boolean) - Debugs when compiling is running when this is set to `true`
- `comment` (string) - A migration comment

## Example

See the [complete code](https://github.com/easterneas/sequelize-typescript-migration/tree/master/example).

## Issues

There are known issue(s):
- Sometimes, the undo (`down`) action may not work, and you should modify the tables manually. Maybe it's because of ordering of relations of models. I'll try to find the cause, and will create a fix later on.

## Documentation

Coming soon!
