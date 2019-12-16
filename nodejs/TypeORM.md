# TYPEORM

TypeORM is going to create database tables for you and find / insert / update / delete your data without the pain of having to write lots of hardly maintainable SQL queries.

## Create a model

Models in your app are your database tables.

```typescript
export class Photo {
  id: number;
  name: string;
  description: string;
  filename: string;
  views: number;
  isPublished: boolean;
}
```

To store things in the database, first it is needed to create dtabase tables, which are created from the models. Not all models, but only those defined as **entities**.

## Create an entity

**Entity** is a model decorated by an `@Entitiy` decorator. Using the example before:

```typescript
import { Entity } from 'typeorm';

@Entity()
export class Photo {
  id: number;
  name: string;
  description: string;
  filename: string;
  views: number;
  isPublished: boolean;
}
```

Now, a database table will be created for the Photo entity and it will be available to work with it.

## Adding table columns

```typescript
import { Entity, Column } from 'typeorm';

@Entity()
export class Photo {
  @Column()
  id: number;

  @Column()
  name: string;

  @Column()
  description: string;

  @Column()
  filename: string;

  @Column()
  views: number;

  @Column()
  isPublished: boolean;
}
```

Column types in t he database are inferred from the property types used, e.g. `number` will be converted into `integer`.

**IMPORTANT** : each database table must have a column with a primary key.

## Creating a primary column

Each entity must have at least one primary key column. This is a requirement and cannot be avoided.

```typescript
import { Entity, Column, PrimaryColumn } from 'typeorm';

@Entity()
export class Photo {
  @PrimaryColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  description: string;

  @Column()
  filename: string;

  @Column()
  views: number;

  @Column()
  isPublished: boolean;
}
```

## Creating an auto generated column

If and id column to be auto-generated can be done like this:

```typescript
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class Photo {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  description: string;

  @Column()
  filename: string;

  @Column()
  views: number;

  @Column()
  isPublished: boolean;
}
```

## Column data types

The next step is to fix the data types. By default, string is mapped to a varchart(255)-like type. Number is mapped to a integer-like type. To setup correctly the data types:

```typescript
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class Photo {
  @PrimaryGeneratedColumn()
  id: number;

  @Column({
    length: 100,
  })
  name: string;

  @Column('text')
  description: string;

  @Column()
  filename: string;

  @Column('double')
  views: number;

  @Column()
  isPublished: boolean;
}
```

Column types for the different Databases can be found here: <https://typeorm.io/#/entities/column-types>

## Creating a connection to the database

Once the entity is created, in the index.ts or app.ts file the connection must be created:

```typescript
import 'reflect-metadata';
import { createConnection } from 'typeorm';
import { Photo } from './entity/Photo';

createConnection({
  type: 'mysql',
  host: 'localhost',
  port: 3306,
  username: 'root',
  password: 'admin',
  database: 'test',
  entities: [Photo],
  synchronize: true,
  logging: false,
})
  .then(connection => {
    // here you can start to work with your entities
  })
  .catch(error => console.log(error));
```

We are using MySQL in this example, but you can use any other supported database. To use another database, simply change the type in the options to the database type you are using: mysql, mariadb, postgres, cockroachdb, sqlite, mssql, oracle, cordova, nativescript, react-native, expo, or mongodb. Also make sure to use your own host, port, username, password and database settings.

We added our Photo entity to the list of entities for this connection. Each entity you are using in your connection must be listed there.

Setting synchronize makes sure your entities will be synced with the database, every time you run the application.

## Loading all entities from the directory

To set up the whole directory, from where all entities will be connected and used in our connection:

```typescript
import { createConnection } from 'typeorm';

createConnection({
  type: 'mysql',
  host: 'localhost',
  port: 3306,
  username: 'root',
  password: 'admin',
  database: 'test',
  entities: [__dirname + '/entity/*.js'],
  synchronize: true,
})
  .then(connection => {
    // here you can start to work with your entities
  })
  .catch(error => console.log(error));
```

But be careful with this approach. If you are using ts-node then you need to specify paths to .ts files instead. If you are using outDir then you'll need to specify paths to .js files inside outDir directory. If you are using outDir and when you remove or rename your entities make sure to clear outDir directory and re-compile your project again, because when you remove your source .ts files their compiled .js versions aren't removed from output directory and still are loaded by TypeORM because they are present in the outDir directory.

# Creating and insterting a photo into the database

````typescript
import {createConnection} from "typeorm";
import {Photo} from "./entity/Photo";

createConnection(/*...*/).then(connection => {

    let photo = new Photo();
    photo.name = "Me and Bears";
    photo.description = "I am near polar bears";
    photo.filename = "photo-with-bears.jpg";
    photo.views = 1;
    photo.isPublished = true;

    return connection.manager
            .save(photo)
            .then(photo => {
                console.log("Photo has been saved. Photo id is", photo.id);
            });

}).catch(error => console.log(error));```
````

## Using async/await syntax

```typescript
import { createConnection } from 'typeorm';
import { Photo } from './entity/Photo';

createConnection(/*...*/)
  .then(async connection => {
    let photo = new Photo();
    photo.name = 'Me and Bears';
    photo.description = 'I am near polar bears';
    photo.filename = 'photo-with-bears.jpg';
    photo.views = 1;
    photo.isPublished = true;

    await connection.manager.save(photo);
    console.log('Photo has been saved');
  })
  .catch(error => console.log(error));
```

## Using Entity Manager

Using entity manager you can manipulate any entity in your app.

```typescript
import { createConnection } from 'typeorm';
import { Photo } from './entity/Photo';

createConnection(/*...*/)
  .then(async connection => {
    /*...*/
    let savedPhotos = await connection.manager.find(Photo);
    console.log('All photos from the db: ', savedPhotos);
  })
  .catch(error => console.log(error));
```

`savedPhotos` will be an array of Photo objects with the data loaded from the database.

## Using Repositories

Refactor the code to use `Repository` instead of `EntityManager`. Each entity has its own repository which handles all operations with its entity. When you deal with entities a lot, Repositories are more convenient to use than EntityManagers:

```typescript
import { createConnection } from 'typeorm';
import { Photo } from './entity/Photo';

createConnection(/*...*/)
  .then(async connection => {
    let photo = new Photo();
    photo.name = 'Me and Bears';
    photo.description = 'I am near polar bears';
    photo.filename = 'photo-with-bears.jpg';
    photo.views = 1;
    photo.isPublished = true;

    let photoRepository = connection.getRepository(Photo);

    await photoRepository.save(photo);
    console.log('Photo has been saved');

    let savedPhotos = await photoRepository.find();
    console.log('All photos from the db: ', savedPhotos);
  })
  .catch(error => console.log(error));
```

More info about repositories: <https://typeorm.io/#/working-with-repository/>

## Loading from the database

```typescript
import { createConnection } from 'typeorm';
import { Photo } from './entity/Photo';

createConnection(/*...*/)
  .then(async connection => {
    /*...*/
    let allPhotos = await photoRepository.find();
    console.log('All photos from the db: ', allPhotos);

    let firstPhoto = await photoRepository.findOne(1);
    console.log('First photo from the db: ', firstPhoto);

    let meAndBearsPhoto = await photoRepository.findOne({
      name: 'Me and Bears',
    });
    console.log('Me and Bears photo from the db: ', meAndBearsPhoto);

    let allViewedPhotos = await photoRepository.find({ views: 1 });
    console.log('All viewed photos: ', allViewedPhotos);

    let allPublishedPhotos = await photoRepository.find({ isPublished: true });
    console.log('All published photos: ', allPublishedPhotos);

    let [allPhotos, photosCount] = await photoRepository.findAndCount();
    console.log('All photos: ', allPhotos);
    console.log('Photos count: ', photosCount);
  })
  .catch(error => console.log(error));
```

## Updating in the database

```typescript
import { createConnection } from 'typeorm';
import { Photo } from './entity/Photo';

createConnection(/*...*/)
  .then(async connection => {
    /*...*/
    let photoToUpdate = await photoRepository.findOne(1);
    photoToUpdate.name = 'Me, my friends and polar bears';
    await photoRepository.save(photoToUpdate);
  })
  .catch(error => console.log(error));
```

## Removing from the database

```typescript
import { createConnection } from 'typeorm';
import { Photo } from './entity/Photo';

createConnection(/*...*/)
  .then(async connection => {
    /*...*/
    let photoToRemove = await photoRepository.findOne(1);
    await photoRepository.remove(photoToRemove);
  })
  .catch(error => console.log(error));
```

## Creating a one-to-one relation

PhotoMetadata class is supposed to container the photo's additional meta-information:

```typescript
import {
  Entity,
  Column,
  PrimaryGeneratedColumn,
  OneToOne,
  JoinColumn,
} from 'typeorm';
import { Photo } from './Photo';

@Entity()
export class PhotoMetadata {
  @PrimaryGeneratedColumn()
  id: number;

  @Column('int')
  height: number;

  @Column('int')
  width: number;

  @Column()
  orientation: string;

  @Column()
  compressed: boolean;

  @Column()
  comment: string;

  @OneToOne(type => Photo)
  @JoinColumn()
  photo: Photo;
}
```

Here, we are using a new decorator called @OneToOne. It allows us to create a one-to-one relationship between two entities. type => Photo is a function that returns the class of the entity with which we want to make our relationship. We are forced to use a function that returns a class, instead of using the class directly, because of the language specifics. We can also write it as () => Photo, but we use type => Photo as a convention to increase code readability. The type variable itself does not contain anything.

We also add a @JoinColumn decorator, which indicates that this side of the relationship will own the relationship. Relations can be unidirectional or bidirectional. Only one side of relational can be owning. Using @JoinColumn decorator is required on the owner side of the relationship.

## Save a one-to-one relation

```typescript
import { createConnection } from 'typeorm';
import { Photo } from './entity/Photo';
import { PhotoMetadata } from './entity/PhotoMetadata';

createConnection(/*...*/)
  .then(async connection => {
    // create a photo
    let photo = new Photo();
    photo.name = 'Me and Bears';
    photo.description = 'I am near polar bears';
    photo.filename = 'photo-with-bears.jpg';
    photo.isPublished = true;

    // create a photo metadata
    let metadata = new PhotoMetadata();
    metadata.height = 640;
    metadata.width = 480;
    metadata.compressed = true;
    metadata.comment = 'cybershoot';
    metadata.orientation = 'portait';
    metadata.photo = photo; // this way we connect them

    // get entity repositories
    let photoRepository = connection.getRepository(Photo);
    let metadataRepository = connection.getRepository(PhotoMetadata);

    // first we should save a photo
    await photoRepository.save(photo);

    // photo is saved. Now we need to save a photo metadata
    await metadataRepository.save(metadata);

    // done
    console.log(
      'Metadata is saved, and relation between metadata and photo is created in the database too'
    );
  })
  .catch(error => console.log(error));
```

## Inverse side of the relationship

Relations can be unidirectional or bidirectional. Currently, our relation between PhotoMetadata and Photo is unidirectional. The owner of the relation is PhotoMetadata, and Photo doesn't know anything about PhotoMetadata. This makes it complicated to access PhotoMetadata from the Photo side. To fix this issue we should add an inverse relation, and make relations between PhotoMetadata and Photo bidirectional. Let's modify our entities:

```typescript
import {
  Entity,
  Column,
  PrimaryGeneratedColumn,
  OneToOne,
  JoinColumn,
} from 'typeorm';
import { Photo } from './Photo';

@Entity()
export class PhotoMetadata {
  /* ... other columns */

  @OneToOne(
    type => Photo,
    photo => photo.metadata
  )
  @JoinColumn()
  photo: Photo;
}
```

```typescript
import { Entity, Column, PrimaryGeneratedColumn, OneToOne } from 'typeorm';
import { PhotoMetadata } from './PhotoMetadata';

@Entity()
export class Photo {
  /* ... other columns */

  @OneToOne(
    type => PhotoMetadata,
    photoMetadata => photoMetadata.photo
  )
  metadata: PhotoMetadata;
}
```

photo => photo.metadata is a function that returns the name of the inverse side of the relation. Here we show that the metadata property of the Photo class is where we store PhotoMetadata in the Photo class. Instead of passing a function that returns a property of the photo, you could alternatively simply pass a string to @OneToOne decorator, like "metadata". But we used this function-typed approach to make our refactoring easier.

Note that we should use @JoinColumn decorator only on one side of a relation. Whichever side you put this decorator on will be the owning side of the relationship. The owning side of a relationship contains a column with a foreign key in the database.


