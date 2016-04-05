# LoL file format details #
Much of my understanding of these formats come from examining renticaltape's code for c++ and maya.  You can find his post on the leaguecraft.com forums http://forum.leaguecraft.com/showthread.php?tid=7440 and the official LoL forums http://www.leagueoflegends.com/board/showthread.php?t=155064

## LoL Mesh format (.skn files) ##
LoL mesh files contain the following information useful for modeling
  * Mesh vertex locations
  * Mesh vertex normals
  * Mesh vertex UV coordinates
  * Mesh vertex bone weights for animation


|**Overall File Structure**| _Notes_|
|:-------------------------|:-------|
|Header Block              |        |
|number of materials       |_not present if matHeader == 0 in header_ |
|Material Block            | _not present if matHeader == 0 in header_|
|count block               |        |
|index block               |        |
|vertex block              |        |


All mesh files begin with a header block.
|Field|Type|Size (bytes)|Description|
|:----|:---|:-----------|:----------|
|magic number|int |4           |A unique identifier|
|numObjects|short|2           |Number of objects in the mesh (Usually 1)|
|matHeader|short|2           |Are material headers present? (1:yes, 0:no)|
|     |    |8           |total      |

If matHeader == 1 then there will be an int with the number of material blocks present in the file
|field|Type|Size (bytes)|Description|
|:----|:---|:-----------|:----------|
|numMaterials|int |4           |Number of material blocks|
|     |    |4           |total      |

if matHeader == 1 and numMaterials > 0 there will be a number of materials blocks:
|field|Type|Size (bytes)|Description|
|:----|:---|:-----------|:----------|
|name |`char[64]`|64          |Name of material|
|startVertex|int |4           |First vertex belonging to this material|
|numVertices|int |4           |Number of vertices in this material|
|startIndex|int |4           |First index belonging to this material|
|numIndices|int |4           |Number of indicies belonging to this material|
|     |    |80          | total     |

if matHeader == 0 the above block will be absent from the file.


count block
|Field|type|Size (bytes)|Description|
|:----|:---|:-----------|:----------|
|numIndicies|int |4           |Number of indecies in the mesh|
|numVertices|int |4           |number of vertices in the mesh|
|     |    |8           |total      |
the values obtained here should match the sums of the numIndices and numVertices for the material blocks if present

index block
|Field|Type|Size (Bytes)|Description|
|:----|:---|:-----------|:----------|
|indices|`short[numIndicies]`|`2*numIndices`|Array of vertex indices|

vertex block
|Field|Type|Size (Bytes)|Description|
|:----|:---|:-----------|:----------|
|verteces|`vertex[numVertices]`|`52*numVertices`|Array of vertex values|

Each vertex has the structure
|Field|Type|Size (Bytes)|Description|
|:----|:---|:-----------|:----------|
|position|`float[3]`|12          |xyz position of vertex|
|boneIdx|`char[4]`|4           |Array of 4 bone number ID's|
|weights|`float[4]`|16          |Corresponding bone weights|
|normals|`float[3]`|12          |vertex normals|
|texcoords|`float[2]`|8           |vertex UV coordinates|
|     |    |52          |total      |


## LoL Skeleton format (.skl files) ##
LoL Skeleton files observe the simple format:
|Header|
|:-----|
|Bone Array|

The header block:
|Field|Type|Size (Bytes))|Description|
|:----|:---|:------------|:----------|
|version|`char[8]`|8            |File format version|
|numObjects|int |4            |Number of objects|
|skeletonHash|int |4            |Unique id(?)|
|numElements|int |4            |Number of bones|
|     |    |20           |total      |

Each bone has the following structure:
|Field|Type|Size (Bytes)|Description|
|:----|:---|:-----------|:----------|
|name |`char[32]`|32          |Name of the bone|
|parent|int |4           |Parent bone ID.  -1 implies no parent|
|scale|float|4           |Scale of bone (?)  Of armature(?)|
|matrix|`float[12]`|48          |3x4 Affine bone matrix in row major format (first four values belong to first row, 2nd four to 2nd row, 3rd four to 3rd row)|
|     |    |52          | total     |

Bone ID's are implicit to their order in the file starting at 0.  So the first bone has an ID of 0, second bone an ID of 1, etc.