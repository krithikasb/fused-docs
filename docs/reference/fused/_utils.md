---
sidebar_label: _utils
title: fused._utils
---

## \_PublicUtils Objects

```python
class _PublicUtils()
```

A class designed to dynamically access utils of public UDFs, fetching their configuration
from the server.

**Methods**:

  __init__(self, cache_key: Any = None): Initializes the _Public instance with an optional cache key.
  __getattribute__(self, key: str) -&gt; Union[Any, AttrDict]: Attempts to access class attributes, falling back to fetching the attribute&#x27;s configuration from a server if not found.
  __getitem__(self, key: str) -&gt; AttrDict: Fetches the configuration for the given key from a server and returns a utility object for execution.

#### \_\_init\_\_

```python
def __init__(cache_key: Any = None)
```

Initializes the _Public instance with an optional cache key.

**Arguments**:

- `cache_key` _Any, optional_ - A key used for caching purposes. Defaults to None.

#### utils

A module to access utility functions located in the UDF called &quot;common&quot;.

They can be imported by other UDFs with `common = fused.public.common`. They contain common operations such as:
- read_shape_zip
- url_to_arr
- get_collection_bbox
- read_tiff_pc
- table_to_tile
- rasterize_geometry


**Examples**:

  
  # This example shows how to access the `geo_buffer` function from the `common` UDF.
  import fused
  import geopandas as gpd
  
  gdf = gpd.read_file(&#x27;https://www2.census.gov/geo/tiger/TIGER_RD18/STATE/11_DISTRICT_OF_COLUMBIA/11/tl_rd22_11_bg.zip&#x27;)
  gdf_buffered = fused.public.common.geo_buffer(gdf, 10)
  
  
  # This example shows how to load a table with `table_to_tile`, which efficiently loads a table by filtering and adjusting based on the provided bounding box (bbox) and zoom level.
  table_path = &quot;s3://fused-asset/infra/census_bg_us&quot;
  gdf = fused.public.common.table_to_tile(
  bbox, table_path, use_columns=[&quot;GEOID&quot;, &quot;geometry&quot;], min_zoom=12
  )
  
  
  # This example shows how to use `rasterize_geometry` to place an input geometry within an image array.
  geom_masks = [
  rasterize_geometry(geom, arr.shape[-2:], transform) for geom in gdf.geometry
  ]
  
  
  Public UDFs are listed at https://github.com/fusedio/udfs/tree/main/public
