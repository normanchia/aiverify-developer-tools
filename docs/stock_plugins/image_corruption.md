# Image Corruption Toolbox
(aiverify.stock.image-corruption-toolbox) [[source](https://github.com/IMDA-BTG/aiverify/tree/main/stock-plugins/aiverify.stock.image-corruption-toolbox)]

## Description
This plugin tests the robustness of AI models to natural corruptions. 

There are four different broad groups of corruptions that are packaged in this plugin. Each of these broad groups of corruptions also have more specific corruption functions indicated in brackets below:
- General (Gaussian, Poisson, Salt and Pepper Noise)
- Blur (Defocus, Gaussian, Glass, Horizontal Motion, Vertical Motion, Zoom Blur)
- Digital (Brightness Up and Down, Contrast Up and Down, Compression, Random Tilt, Saturate)
- Environmental (Rain, Fog, Snow)

The toolbox generates corrupted images based on the uploaded test data at 5 different severity levels for each corruption function. The accuracy of the model is calculated with the new corrupted datasets.

## Plugin Content
- Algorithms
  
|          Name           |                                                                                             Description                                                                                             |
| :---------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|    Blur Corruptions     |     Algorithm that adds blur corruptions (defocus, gaussian, glass, horizontal motion, vertical motion and zoom Blur) to images at 5 severity levels, and calcualtes the accuracy of the model      |
|   Digital Corruptions   | Algorithm that adds digital corruptions (brightness up and down, contrast up and down, compression, random tilt, saturate) to images at 5 severity levels, and calcualtes the accuracy of the model |
| Environment Corruptions |                             Algorithm that adds environmental corruptions (rain, fog and snow) to images at 5 severity levels, and calcualtes the accuracy of the model                             |
|   General Corruptions   |                Algorithm that adds environmental corruptions (gaussian, poisson and salt and pepper noise) to images at 5 severity levels, and calcualtes the accuracy of the model                 |


- Widgets

| Name                                                                                                                                                                                                                                                                                                             | Description                                                                                       |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Introduction                                                                                                                                                                                                                                                                                                     | To provide an introduction to the Image Corruption Toolbox                                        |
| Understanding Line Chart                                                                                                                                                                                                                                                                                         | To guide your users on reading the generated line charts                                         |
| Line Chart (Blur Corruptions)                                                                                                                                                                                                                                                                                    | To generate line chart to visualise the accuracy results when blur corruptions are applied        |
| <ul> <li> Samples (Blur: Defocus Blur) </li> <li>Samples (Blur: Gaussian Blur) </li> <li>Samples (Blur: Glass Blur) </li> <li> Samples (Blur: Horizontal Motion Blur) </li> <li>Samples (Blur: Vertical Motion Blur) </li> <li>Samples (Blur: Zoom Blur)</li> </ul>                                              | To generate sample images for the blur corruptions                                                |
| Line Chart (Digital Corruptions)                                                                                                                                                                                                                                                                                 | To generate line chart to visualise the accuracy results when digital corruptions are applied     |
| <ul> <li> Samples (Digital:  Brightness Up) </li> <li>Samples (Digital:  Brightness Down) </li> <li>Samples (Digital:  Contrast Up </li> <li> Samples (Digital:  Contrast Down </li> <li>Samples (Digital:  Saturate) </li> <li>Samples (Digital: Compression)</li> <li>Samples (Digital: Compression)</li></ul> | To generate sample images for the digital corruptions                                             |
| Line Chart (Environmental Corruptions)                                                                                                                                                                                                                                                                           | To generate line chart to visualise the accuracy results when environment corruptions are applied |
| <ul><li> Samples (Environment: Rain) </li><li> Samples (Environment: Fog)</li>   <li> Samples (Environment: Snow)</li></ul>                                                                                                                                                                                      | To generate samples for the environment corruptions                                               |
| Line Chart (General Corruptions)                                                                                                                                                                                                                                                                                 | To generate line chart to visualise the accuracy results when general corruptions are applied     |
| <ul><li> Samples (General: Gaussian) </li><li> Samples (General: Poisson)</li>   <li> Samples (General: Salt and Pepper)</li></ul>                                                                                                                                                                               | To generate sample images for the general corruptions                                             |
| Recommendation                                                                                                                                                                                                                                                                                                   | To provide recommendations for robustness (image corruptions) testing |

## Using the Plugin in AI Verify
### Data Preparation
- Image dataset ([Tutorial for Preparation](../how_to/prepare_image.ipynb))
- Annotated Ground Truth Dataset ([Tutorial for Preparation](../how_to/prepare_image.ipynb))

### Additional Requirements
ImageMagick (Details for installation can be found [here](https://docs.wand-py.org/en/0.6.11/guide/install.html#))

### Algorithm User Input(s)
Note: These inputs are the same for all the algorithms in this plugin (Blur Corruptions, Digital Corruptions, Environmental Corruptions and General Corruptions)

|                Input Field                |                                                                            Description                                                                             |   Type   |
| :---------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------: |
|        Annotated ground truth path        |                                      An uploaded dataset containing image file names and the corresponding ground truth label                                      | `string` |
| Name of column containing image file name |                                   Key in the name of the column containing the file names in the annotated ground truth dataset                                    | `string` |
|  Seed for selection of data for display   | Some of the plugins selects a random sample data for display. The random seed for this selection can be changed, if desired. The default value we are using is 10. |  `int`   |


### Sample use of the widgets

![ICT sample](../images/image_corruption_toolbox_sample.png)


### More details
<details>
<summary> Algorithm input schema </summary>

```json
{
    "title": "Algorithm Plugin Input Arguments",
    "description": "A schema for algorithm plugin input arguments",
    "type": "object",
    "required": [
        "annotated_ground_truth_path",
        "file_name_label",
        "set_seed"
    ],
    "properties": {
        "annotated_ground_truth_path": {
            "title": "Annotated ground truth path",
            "description": "Select the dataset containing image file names and corresponding ground truth labels",
            "type": "string",
            "ui:widget": "selectDataset"
        },
        "file_name_label": {
            "title": "Name of column containing image file names",
            "description": "Key in the name of the column containing the file names in the annotated ground truth dataset",
            "type": "string"
        },
        "set_seed": {
            "title": "Seed for selection of data for display",
            "description": "Change to a specific seed for random selection the sample data for display if desired",
            "default": 10,
            "type": "integer"
        }
    }
}
```

</details>

<details>
<summary>Algorithm output schema </summary>

```json
{
    "title": "Algorithm Plugin Output Arguments",
    "description": "A schema for algorithm plugin output arguments",
    "type": "object",
    "required": [
        "results"
    ],
    "minProperties": 1,
    "properties": {
        "results": {
            "description": "Results from the unadverserial robustness algorithms",
            "type": "array",
            "minItems": 1,
            "items": {
                "type": "object",
                "required": [
                    "corruption_group",
                    "corruption_function",
                    "accuracy",
                    "display_info"
                ],
                "properties": {
                    "corruption_group": {
                        "description": "Broad corruption group",
                        "type": "string"
                    },
                    "corruption_function": {
                        "description": "Name of corruption algorithm",
                        "type": "string"
                    },
                    "accuracy": {
                        "description": "Accuracies starting from no corruption to higher levels of severities",
                        "items": {
                            "type": "object",
                            "minProperties": 1,
                            "patternProperties": {
                                "^severity": {
                                    "type": "number"
                                }
                            }
                        }
                    },
                    "display_info": {
                        "description": "Information for the display of sample images",
                        "type": "object",
                        "items":{
                            "minProperties": 6,
                            "patternProperties": {
                                "^severity": {
                                    "type": "array"
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
```

</details>