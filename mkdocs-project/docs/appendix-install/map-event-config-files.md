# Map Event Configuration Files #

A map configuration file created by GeoProcessor software
can contain an event handler section for GeoLayerView,
with event handler configuration similar to the following.
The event handler is added to a GeoMapProject using the GeoView
[`SetGeoLayerViewEventHandler`](http://software.openwaterfoundation.org/geoprocessor/latest/doc-user/command-ref/SetGeoLayerViewEventHandler/SetGeoLayerViewEventHandler/) command.
An `eventType` of `hover` is associated with transient popup that shows attributes for the layer feature over which hovering occurs.
An `eventType` of `click` is associated with modal popup that can be configured to show attributes for the layer feature
that was clicked on, and optionally buttons that cause other actions such as displaying a graph.

```
              "eventHandlers": [
                {
                  "eventType": "click",
                  "name": null,
                  "description": "",
                  "properties": {
                    "popupConfigPath": "graphs/diversion-popup-config.json"
                  }
                }
              ]
```

The popup configuration file specified by `popupConfigPath` property
indicates the appearance and functionality of the popup.
Currently, the GeoProcessor does not provide a command to create a popup configuration file
and therefore it must be created with a text editor.
The following is an example of a popup configuration file.
In this case the popup will list all attributes for the feature that has been
clicked on and display buttons with each of the labels shown.

```
{
  "id" : "diversion-popup-config",
  "name": "Diversion popup configuration",
  "description":  "List main attributes and provide buttons to graph time series.",
  "layerAttributes" : {
    "include" : [ "*" ],
    "exclude" : [],
    "formats": []
  },
  "actions": [
      {
        "action" : "graph",
        "label" : "Demand",
        "productPath" : "graphs/diversion-DiversionDemand-graph-config.json"
      },
      {
        "action" : "graph",
        "label" : "Historical",
        "productPath" : "graphs/diversion-DiversionHistorical-graph-config.json"
      },
      {
        "action" : "graph",
        "label" : "Available Flow",
        "productPath" : "graphs/diversion-Available_Flow-graph-config.json"
      },
      {
        "action" : "graph",
        "label" : "Combination",
        "productPath" : "graphs/diversion-combination-graph-config.json"
      }
  ]
}
```

For example the popup will display similar to the following:

**<p style="text-align: center;">
![click-popup](images/click-popup.png)
</p>**

**<p style="text-align: center;">
Example Popup for Click Event (<a href="../images/click-popup.png">see full-size image</a>)
</p>**

The main properties of a popup configuration file are described in the following table.

**<p style="text-align: center;">
Popup Configuration File Main Properties
</p>**

| **Property**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | **Description** | **Default** |
| -- | -- | -- |
| `id` | Identifier for the popup configuration, which can be used internally by software. | |
| `name` | Name for the popup configuration that can be displayed, for example to allow a user to select a configuration file. | |
| `description` | Description for the popup configuration that can be displayed, for example to help a user understand the popup. | |
| `layerAttributes` | Indicates which attributes to display and how to format those attributes (see below). | |
| `actions` | A list of actions that can be triggered by selecting buttons displayed in the popup (see below). | |

The following table describes properties for `layerAttributes`.

**<p style="text-align: center;">
Popup Configuration File `layerAttributes` Properties
</p>**

| **Property** | **Description** | **Default** |
| -- | -- | -- |
| `include` | An array of string patterns indicating which layer attributes to include in the popup. | Include all attributes. |
| `exclude` | An array of string patterns indicating which layer attributes to exclude in the popup, which will be processed after `include`. | Include all attributes. |
| `formats` | An array of properties defining how to format attributes.  **Need to define.** | Use default string representation of attributes. |

**<p style="text-align: center;">
Popup Configuration File `actionss` Properties
</p>**

| **Property**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | **Description** | **Default** |
| -- | -- | -- |
| `action`<br>**required**</br> | The action type, indicating what will happen with the popup button is pressed:<ul><li>`graph` (**Proposed:** change this to `displayGraph`) - display a graph specified by TSTool JSON graph configuration file</li><li>**Proposed:** `displayTextFile` - display a text file</li></ul>. | None - must be specified. |
| **Proposed:** `chartPackage` | The chart package to use for graphs:<ul><li>`ChartJS`</li><li>`plotly`</li></ul> | `ChartJS` |
| `label`<br>**required**</br> | The button label shown in the popup. | None - must be specified. |
| **Proposed:** `modal` | Whether or not the popup window is modal: `false` or `true`. | `true` |
| **Proposed:** `outputComponent` | Output destination:<ul><li>`Popup` - display as popup (see also `modal`)</li><li>**Proposed:** `Tab` - display in a new tab</li><li>**Proposed:** `Window` - display in a new Window</li></ul> | `Popup` |
| `productPath`<br>**required**</br> | The path to a TSTool JSON graph configuration file for graph. | None - must be specified for `graph` action. |