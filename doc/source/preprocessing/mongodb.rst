MongoDB Backend
================

MongoDB can be downloaded from mongodb.org. DSI2 uses PyMongo to connect
to mongodb.

MongoDB Organization
---------------------
``dsi2`` relies on a database called "dsi2" in your mongodb instance.  There are a number
of collections inside dsi2 that are used.
  
"dsi2.scans"
  This collection holds non-identifiable information about the individuals in the database.
  Information such as which experiment they were a part of, age, gender, etc. is included
  in this collection.  Generally scans is the first database queried, then scan_id's from
  this query are used in spatial queries during analysis.
  
"dsi2.coordinates"
  This collection contains the mapping from spatial index to streamline id. By querying
  a set of spatial indices, mongodb will return the set of all streamline ids that 
  intersect those spatial coordinates. These streamline ids can be used to query the
  other collections such as atlases and streamlines
  
"dsi2.streamlines"
  This collection contains serialized binary data that can be loaded as a numpy array.
  You can search a streamline id from a specific subject and get its spatial trajectory.
  
"dsi2.atlases"
  This contains the unique atlases that have been used to label streamlines.
  
"dsi2.connections2"
  **Needs a better name!!** This collection contains the lists of connection ids for
  each streamline in a dataset.  This is useful because the connection ids are ints
  and small, so upon loading a dataset, one can simply query that atlas's connection
  ids for a scan and hold them in memory instead of performing repeated joins in the
  mongo client.
  
"dsi2.connections":
  To query a specific connection (a pair of regions from a specific atlas) you 
  query this database with a subject id and connection id from the atlas. The 
  streamline ids connecting that region pair are returned.
  