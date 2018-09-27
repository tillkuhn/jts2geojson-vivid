## Introduction

This is a fork from [bjornharrtell/jts2geojson](https://github.com/bjornharrtell/jts2geojson) with the only difference that the Objects instantiated by `GeoJSONReader` are based on the (older) `com.vividsolutions:jts` library that comes as a transitive dependency of *hibernate-spatial* and doesn't ship with its own GeoJSON Converter classes. Maybe newer versions will make this project obsolete.

This Java library can convert JTS geometries to GeoJSON and back. Its API is similar to other io.* classes in JTS.

[![Build Status](https://travis-ci.org/bjornharrtell/jts2geojson.svg?branch=master)](https://travis-ci.org/bjornharrtell/jts2geojson)

## Maven

```xml
<dependency>
    <groupId>org.wololo</groupId>
    <artifactId>jts2geojson</artifactId>
    <version>0.12.0</version>
</dependency>
```

## Usage

```java

  GeoJSONWriter writer = new GeoJSONWriter();
  GeoJSON json = writer.write(geometry);
  String jsonstring = json.toString();

  GeoJSONReader reader = new GeoJSONReader();
  Geometry geometry = reader.read(json);
  
```

## Features and FeatureCollections

JTS does not have anything like GeoJSON Feature or FeatureCollection but they can be parsed by this library. Example:

```java

  // parse Feature or FeatureCollection
  Feature feature = (Feature) GeoJSONFactory.create(json);
  FeatureCollection featureCollection = (FeatureCollection) GeoJSONFactory.create(json);
  
  // parse Geometry from Feature
  GeoJSONReader reader = new GeoJSONReader();
  Geometry geometry = reader.read(feature.getGeometry());
  geometry = reader.read(featureCollection.getFeatures()[0].getGeometry());
  
  // create and serialize a FeatureCollection
  List<Features> features = new ArrayList<Features>();
  Map<String, Object> properties = new HashMap<String, Object>();
  features.add(new Feature(geometry, properties);
  GeoJSONWriter writer = new GeoJSONWriter();
  GeoJSON json = writer.write(features);

```
