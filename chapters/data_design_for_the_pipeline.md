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
