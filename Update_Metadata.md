## Update Metadata of Feature Service in AGOL using CSV File


#### Run this cell to connect to your GIS and get started:

```python
from arcgis.gis import GIS
import pandas as pd
gis = GIS("home")
```


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
  

