# geojson-folders

A container format for [GeoJSON](https://datatracker.ietf.org/doc/html/rfc7946)
that adds support for folders.

## Abstract

Hierarchical folders are a common and practical way of
organizing data. Most operating systems support folders. One
geospatial format also supports folders: KML.

GeoJSON is nearly usable as an go-between for other geospatial
formats, but the lack of a folder concept makes it difficult
to represent the same data in KML and GeoJSON, and also to
build a geospatial interface with folders.

## Status

This pattern is in use with [Placemark](https://www.placemark.io/).
This repository is an effort to spark a conversation, before any
specification is versioned or released. The Placemark organization
would aim to have a stable specification of `geojson-folders`
to make intercompatibility possible.

## Example

```json
{
  "type": "root",
  "children": [
    {
      "type": "folder",
      "meta": {
        "name": "Test"
      },
      "children": [
         // ...features and folders
      ]
    }
    // ...features
  ]
}
```

This format is based on a tree of folders that can contain
ordered features and folders.

- The root of the tree has a member `"type"` with the value `"root"`.
  This can be used to differentiate geojson-folders data from other
  GeoJSON data.
- The root contains a `"children"` member that has an array with
  ordered features and folders as children.
- Each item in the `"children"` array can either be a feature, marked
  by `"type": "Feature"`, or a folder, marked by `"type": "folder"`
- Folders contain a member `"meta"`, which is an object of arbitrary
  JSON data.

## Compatibility with GeoJSON

This implementation has a folder representation that is not valid as
GeoJSON. It is trivial to flatten a `geojson-folders` structure into
valid JSON, and utilities can be provided that do so, but this data
cannot be treated immediately as GeoJSON.

An alternate proposal would be to represent folders as a property of
each feature. However, this has drawbacks:

- Folders would require unique identifiers.
- The order of features in folders would be more complex.
- Nesting folders would be more complex.
- There's no obvious place for the `"meta"` information to go.

## Comparison to existing collection types

There are existing collection types in GeoJSON - GeometryCollection
and multi types, like MultiLineString. However, these do not represent
groups of features. Features with GeometryCollections must share
the same properties between all of the geometries in the GeometryCollection.

## Nesting properties

This specification aims to achieve all the capabilities of KML's folder
structure:

- Features can be at the "root level", thus not in any folder. The root
  is not a folder.
- Folders can be arbitrarily nested.
- Folders can have their own metadata.

## Relationship to other standards

It would be preferable to invent as little as possible in this format.
One consideration is changing the `meta` field of the folder object
to `data` in order to correspond closer to the [unist](https://github.com/syntax-tree/unist)
specification and ecosystem of tools.

## Reference implementations

- [togeojson’s kmlWithFolders](https://github.com/placemark/togeojson) method
- [toKML’s foldersToKML](https://github.com/placemark/tokml) method
