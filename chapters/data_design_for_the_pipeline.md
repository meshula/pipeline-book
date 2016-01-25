# Data Design for the Pipeline

## Uniformity

Uniformity is important for being able to inexpensively update the pipeline. Inexpensively implies being able to update the pipeline without investing a lot of engineering time into the change. A single data format and a single reading, writing, and processing library gives you the economy of implementing things only once, and having only a single set of data transfer quirks to learn about and deal with throughout the entire pipeline, instead of a quirk in every area of expertise. All the data of the same kind should be stored in the same way.

The XML example here is of a scene description. The scene is composed of a set of objects.


```xml
<scene>
   <objects>
      <object="foo"> 
         <attrs>...</attrs>
         <relationships>...</relationships> 
         <contains>...</contains>
      </object>
      ... more objects ...
   </objects> 
</scene>
```

Each object in the scene has the same structure. It has a set of attributes, a set of relationships with other objects, and a set of contained objects. Relationships typically encode dependencies between the object and other referenced objects, and a description of the relationship, for example, parenting. From this simple starting point a full scene description can be built out.

## Independence

The next tier of the data description is data about the data. This is the schema. A schema assigns meaning to the objects. If part of the schema describes a transform, for example, given the schema, you're going to be able to find all the transforms anywhere in the data.

Semantics assign meanings to the attributes. For example, if you have an attribute, and its semantic type is transform, a tool could automatically create a manipulator for an object whenever the object is selected.

Rules assign meanings to the relationships. If a relationship is “this is parent of that”, the rule specifies that deep under the covers, a transform hierarchy needs to be wired up.

To support pipeline reordering, factor data transformations properly. Don't throw away data that later stages might need. For example, a mesh might start out as a nice Catmull-Clark patch surface with a lot of high vertex count patches. By the time it goes out to the rendering pipeline, it might have been tri-stripped. A lot of really interesting information about that mesh has been lost. That information might otherwise have been used efficiently later for other calculations. The data format should allow for retention of important things.

## Tolerance

Pipeline elements should be tolerant. If garbage is read in, it should be possible to write it back out again without destroying it. An early example is the AIFF chunk format — the old audio interchange file format. Every chunk had a size and an identifier. If the parser didn't recognize the id, it burned right past it. There's a flaw in the scheme however. If there are unknown chunks, there is no way to tell if there are dependencies on data within those chunks that cross chunk boundaries. It is easily possible to corrupt a file if care isn't taken. The relationship rules should specify dependencies between data, so that even if you don't know what is in a chunk, you can at least warn a user that an edit could potentially break the data, and give options such as abandoning an edit, or removing chunks that could potentially become corrupt.

Watch for separable but dependent data. The light mapper and the sub-mesher are likely in separate tools, but if something is re-submeshed, the light maps will need to be regenerated. The pipeline must understand dependencies to avoid tripping things up.

## Longevity

A well designed system has longevity. You can open up files years later, add water, and reconstitute them fairly reliably if the data formats are stable and robust.

## Summary

A data design based on these principals pays dividends on many fronts. The data can have a long life. Data transfer can be lossless. Dependencies can be encoded and tracked. Common data store access libraries can be developed which reduce the number of quirks one needs to deal with between various tools. Finally, applications can be dynamically bound at run time through common protocols for real time tweaking interfaces and similar enhancements.