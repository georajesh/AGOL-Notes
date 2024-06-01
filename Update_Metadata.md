## Update Metadata of Feature Service in AGOL using CSV File


#### Run this cell to connect to your GIS and get started:


```python
from arcgis.gis import GIS
import pandas as pd
gis = GIS("home")
```

    /opt/conda/lib/python3.9/site-packages/arcgis/gis/__init__.py:703: UserWarning: You are logged on as rdulal@flemingcollege.ca_Fleming with an administrator role, proceed with caution.
      warnings.warn(


#### Search Group Items


```python
search = gis.content.search(query="group:78aa453dfd244bdcb356128a62be9562", max_items=9999)
```

### Export Items List


```python
FS = []
for item in search:
    FS.append("{},{},{}".format(item.id, item.title, item.type))
    
```


```python
df = pd.DataFrame(FS, columns=["to_split"])
df[['item_id', 'item_name', 'item_type']] = df['to_split'].str.split(',', expand=True)
df.to_csv("/arcgis/home/Features.csv", columns=['item_id', 'item_name', 'item_type'])
print("File Export Successful.")
```

    File Export Successful.


### Import Metadata


```python
# Load the CSV file
file_path = '/arcgis/home/metadata.csv'  # Update with the actual path
metadata_df = pd.read_csv(file_path)

```


```python
# Function to update item metadata
def update_item_metadata(item_id, summary, tags, description, credits, terms_of_use):
    try:
        item = gis.content.get(item_id)
        if item:
            item.update(item_properties={
                'snippet': summary,
                'tags': tags.split(',') if pd.notna(tags) else [],
                'description': description,
                'credits': credits,
                'licenseInfo': terms_of_use
            })
            print(f"Updated item: {item_id}")
        else:
            print(f"Item not found: {item_id}")
    except Exception as e:
        print(f"Error updating item {item_id}: {e}")

# Iterate over the dataframe and update items
for index, row in metadata_df.iterrows():
    update_item_metadata(
        item_id=row['item_id'],
        summary=row['summary'],
        tags=row['tags'],
        description=row['description'],
        credits=row['credits'],
        terms_of_use=row['terms_of_use']
    )

```

    Updated item: 0a518c00de0e4edca4fa4e853c20bcc0	item_name
    Updated item: 2aa6842abf11459c8d5ff5037a0c74a0	item_name
    Updated item: 1d3e332d56aa45838db57deb9bb93d78	item_name
    Error updating item 8c2c2fd714f14bc49eb48729324dd613: You do not have permissions to access this resource or perform this operation.
    (Error Code: 403)
    Error updating item b45f0f04df1740ae93538b63840eab91: You do not have permissions to access this resource or perform this operation.
    (Error Code: 403)
    Updated item: 51cd47570dbd4518bfd5e699838ea982	item_name
    Updated item: 19bba47af9ca4eed9a3ac3406f807aa8	item_name
    Updated item: e8c24d8882a24bdfaa0d0474b47c62d1	item_name
    Updated item: edc25012004c4a44b9f5c95c8e0c31ea	item_name
    Updated item: c87c83ac3e3a40e3a38eecc0b1f805cf	item_name
    Error updating item a874213882e54d9d845532da66f10002: You do not have permissions to access this resource or perform this operation.
    (Error Code: 403)
    Updated item: 65c3c484f5fa4a5b900c4e2e10fb5e97	item_name
    Updated item: 76cbfb9799614c50b1449a6240e73326	item_name
    Error updating item d7d3bc242a0f4985adb165a76e3560ce: You do not have permissions to access this resource or perform this operation.
    (Error Code: 403)
    Updated item: 980cc61245c34cc485036d59bd72a8c3	item_name
    Updated item: e64a035651994d0f80a8a088a38b6ca1	item_name
    Error updating item 3a766d2bc1f74bc4b51168311fb43f2c: You do not have permissions to access this resource or perform this operation.
    (Error Code: 403)
    Updated item: f6a57a223c2842d199ca56a0cebe61cc	item_name
    Updated item: 8edd71556fba4c6b89604288f8c8e098	item_name
    Updated item: cb5b37a979924c4b83c1eb83cccba2ef	item_name
    Updated item: 30757230f1c841f1add333cd72a93b47	item_name
    Updated item: 90f61263cb5b45f3b773f29dd6c025d5	item_name
    Updated item: 18ca85b050534d229e18b97d7456985a	item_name
    Updated item: 662c45c1d15f435ab2a4c5e999e2ca35	item_name
    Updated item: b29c4d1e49d342449e7d086e4aa4d810	item_name
    Updated item: 2d5b90da39494d7aa7a7df6e504e3706	item_name
    Updated item: 6f372cb4f9ea488f82519d0afd863c7c	item_name



```python

```
