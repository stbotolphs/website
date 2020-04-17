# Testing functionality

The CMS uses a number of external services for functionality. You can create a
test page which exercises most of these by following the steps in this video:

![](./testing.gif)

The following services are tested by this page.

## Relational data

Relational data, such as records of pages, etc should be stored in a PostgreSQL
relational database.

## Object storage

Images should be persisted in and served from an object store.

## Email

The "contact us" form should send email to the configured send address correctly.
