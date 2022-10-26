# ClueWeb22 API

## Getting Started
This API can be used with the ClueWeb22 dataset.  Depending on what data was purchased, the structure of ClueWeb22 will look like this:
- ClueWeb22/html
  - The html directory contains the original html of the webpage at the time that it was crawled.  The html is in WARC format.  For each WARC file, there is also an offset file to access documents idividually.
- ClueWeb22/txt
  - The text directory contains only the text (no html markup) from html nodes that have been marked as primary content.  The text is in json format.
- ClueWeb22/vdom
  - The vdom directory contains semantic annotations such as table, list, etc and node annotations such as the x and y position of the node.  The vdom is in protobuf format, and requires this API to access.  The protobuf files are stored in zip archives.
- ClueWeb22/inlink
  - The inlink directory contains the links that point to each ClueWeb document.  Some documents may not have inlinks.  The inlinks are in json format.
- ClueWeb22/outlink
  - The outlink directory contains the links each ClueWeb document point to.  Some documents may not have outlinks.  The outlinks are in json format.

## Using the ClueWeb22Api
Each method of the API, will require certain types of data, which are noted.  The API can be initialized with a ClueWeb22 ID and the root directory of the ClueWeb22 dataset.  The API works with the structure that ClueWeb22 was distributed in.  
  
To get started initialize the API:
```
from ClueWeb22Api import ClueWeb22Api

cw22id = clueweb22-en0000-00-00004
root_path = [BASE_DIRECTORY]/ClueWeb22
clueweb_api = ClueWeb22Api(cw22id, root_path)
```

### How to get the HTML
This method returns the HTML for a ClueWeb ID.  
Requires: ClueWeb22/html

```
html = clueweb_api.get_html_from_warc()
```
Note: The ClueWebAPI is not required to get the html from the WARC files.

### How to get the Primary Content of the page with HTML annotation
This function returns the primary content of the page and HTML annotations such as Title, Table, and List.  The annotations are returned as a list with the node ids and the offsets in the text where they begin and end.  
Requires: ClueWeb22/html and ClueWeb22/vdom

```
clueweb_api.get_primary_content_with_annotations()
```

### How to get Node Features
These functions return the vdom node features such as position x, position y, and list item. 
Requires: ClueWeb22/html and ClueWeb22/vdom  

#### How to get Node Features by Node ID  
This function returns just the node IDs and their vdom features.  
```
clueweb_api.get_node_features()
```

#### How to get Node Features with Node IDs and Node text
This function returns the node ID, node text, and node vdom features.  This will take more time than getting the node vdom features alone.  There is one optional parameter: is_primary which is set by default to True.  Setting the is_primary parameter returns only nodes that are considered primary to the page.
```
clueweb_api.get_node_features_with_text(is_primary=True)
```

### How to get Primary Text 
These functions return the plain text of what is considered the primary content of the page in json format.  
Requires: ClueWeb22/txt
```
clueweb_api.get_clean_text()
```

### How to get Inlinks
This function returns the inlinks for a page in json format.  
Requires: ClueWeb22/inlink
```
clueweb_api.get_inlinks()
```

### How to get Outlinks
This function returns the outlinks for a page in json format.  
Requires: ClueWeb22/outlink
```
clueweb_api.get_outlinks()
```
