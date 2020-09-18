# QGIS Dataset QA Workbench

![github-pages](https://github.com/kartoza/qgis_dataset_qa_workbench/workflows/github%20pages/badge.svg)

A QGIS3 plugin for assisting in dataset quality assurance workflows.

This plugin allows loading checklists with steps that should be verified 
when doing dataset quality assurance (QA). Checklist items can be automated 
by using QGIS Processing algorithms

## Table of contents

1. [Installation](#installation)
1. [Quickstart](#quickstart)

   1. [Add sample checklist repository](#0-add-sample-checklist-repository)
   1. [Choose checklist](#1-choose-checklist-to-perform-validation-with)
   1. [Perform validation](#2-perform-validation-of-a-resource)
   1. [Generate report](#3-generate-validation-report)
   
1. [Creating a new checklist](#creating-a-new-checklist)
1. [Sharing checklists and Processing algorithms with other users](#sharing-checklists-and-processing-algorithms-with-other-users)
1. [Development](#development)
1. [Attribution](#attribution)


## Installation

This plugin can be installed directly by QGIS. It is published in the 
[official QGIS plugins repository]. Use the QGIS plugin manager 
(navigate to _Plugins -> Manage and Install Plugins..._) and search for 
_Dataset QA Workbench_. Then install the plugin.

NOTE: Be sure to have the _Show also experimental plugins_ checkbox checked, 
in the plugin manager _Settings_ section.

We recommend also installing the [QGIS Resource Sharing] plugin, since it
is able to share the checklists that are required by  our plugin.


## Quickstart

### Add sample checklist repository

After having installed both this plugin and the [QGIS Resource Sharing] 
plugin, add a new repo to QGIS Resource Sharing:
   
1. Navigate to _Plugins -> Resource Sharing -> Resource Sharing_

1. On the QGIS Resource Sharing dialog, go to _Settings -> Add repository..._

1. Add the following repository:
   
   -  Name: QGIS Dataset QA Workbench demo
   -  URL: https://github.com/kartoza/qgis_dataset_qa_workbench.git
   -  Authentication: (Leave it blank)
     
1. The new _QGIS Dataset QA Workbench demo_ repository shall now be 
   displayed by the Resource Sharing plugin
   
1. Navigate to the  _All collections_ section and look for an entry named
   _QGIS Dataset QA Workbench demo_
     
1. Press the _Install_ button. The Resource Sharing plugin proceeds to 
   download and install some sample checklists to the 
   `{qgis-user-profile-dir}/checklists` directory.
     
     
### Choose checklist to perform validation with
     
1. Open the QGIS Dataset QA Workbench dock (navigate to 
   _Plugins -> Dataset QA Workbench -> Dataset QA Workbench_ or click the 
   plugin's icon <img src="resources/clipboard-check-solid.svg" width="20" />) and 
   navigate to the _Choose Checklist_ tab
   
1. Inside the plugin dock, navigate to _Choose Checklist -> Choose..._. 

   In the dialog that opens, select one of the existing checklists. Take into
   account the dataset type that it is applicable to (_document_, _vector_ or 
   _raster_) and the artifact that it applies to (_dataset_, _metadata_ or 
   _style_). Click the _OK_ button to close this dialog. The checklist is loaded 
   and is ready to use.
   
1. Depending on the loaded checklist and its dataset and artifact types, select 
   if you want to:

   - a) Validate one of the currently loaded layers. If so, be sure to select 
     it on the list of layers shown in the plugin dock
     
   - b) Validate an external file, by indicating its path on the local filesystem
   
   Upon selecting one of these options, both the _Perform Validation_ and 
   _Generate Report_ tabs become selectable
   
   
### Perform validation of a resource

Move over to the _Perform Validation_ tab where you are presented with a list 
of checklist steps to be validated. For each of these checks:

1. Read the _description_ in order to understand what the current check is 
   about
   
1. A checklist check can be validated in one of two ways:

   - Manually - Follow the instructions provided by the _guide_ section. 
     These should be detailed and practical enough in order to allow you to 
     properly validate the current checklist check.
     
     
   - Automatically - If applicable, the validation may be performed by 
     pressing one of the two buttons present on the _automation_ section.
     
     -  _Run_ - Perform validation by using whatever predefined parameters have 
        been used by the checklist's designer
        
     -  _Configure and run..._ - Configure the check's validation parameters 
        and then run the validation procedure
        
1. After performing validation, you may optionally click the 
   _Validation notes_ section and type down any relevant notes about the 
   process.


### Generate validation report

After having validated all of the checklist's checks, move over to the 
_Generate Report_ tab. This tab displays a summary of the validation process, 
with information related to:

- the dataset being validated
- the overall validation result
- result of each check

1. Customize the report's _Validated by_ field

   By default, the report uses whatever value is automatically generated by
   QGIS in its global `user_full_name` variable. If you wish to provide a 
   different name:
   
   1. Navigate to _Settings -> Options... -> Variables
   
   2. Define a new variable named `dataset_qa_workbench`
   
   3. Set an appropriate value. The plugin will use it as the author of 
      validation reports
      
1. If applicable, the checklist may specify a post-validation action. In this 
   case, the _Run post validation_ and _Configure and run post validation..._ 
   buttons will be enabled.
   
   Post validation actions may be used for providing confirmation of the 
   validation procedure to some third-party. Some examples include emailing the 
   validation report to a list of recipients or POSTing the report to some 
   centralized host by using a suitable REST API
   
1. If you are validating a loaded layer, the _Add validation report to layer 
   metadata_ button will be enabled. In this case you have the option to 
   include the validation report in the layer's metadata. This modifies 
   existing layer metadata in two ways:
   
   - The full validation report is appended to the end of the metadata's 
     _Abstract_ 
     field. Note that additional presses of the _Add validation report to 
     layer metadata_ button cause the new report to be appended to whatever 
     was already written on the Abstract field (including any previous 
     validation reports that might be there)
     
   - A new line is also appended to the metadata's _History_ section. This
     include's the validation report's generation timestamp and the overal
     validation result
     
1. Finally, the report may also be saved as a PDF file by selecting an 
   appropriate destination path in the _Save validation report to_ text box
   and then pressing the _Save_ button

   
## Creating new checklists

Checklists are stored locally on the QGIS user profile directory (accessible 
from QGIS by navigating to _Settings -> User Profiles -> 
Open Active Profile Folder..._) under the _checklists_ directory.

Installing a new checklist is simply a matter of placing a suitable file in 
this directory.

Checklists are stored in [json] format. They are defined as JSON objects and 
must conform to a predefined [checklist schema]. Example checklist definition:

```json
{
  "name": "Sample checklist with action for emailing validation report",
  "description": "This is just a sample checklist - be sure to delete it\n\nThis also demonstrates sending emails with the report of validation",
  "dataset_type": "vector",
  "validation_artifact_type": "dataset",
  "checks": [
    {
      "name": "geometry is valid",
      "description": "Layer's geometry does not have invalid geometries.",
      "guide": "Navigate to Vector -> Geometry tools -> Check Validity... and run the validity analysis tool. Afterwards check that there are no features on the `invalid output` layer",
      "automation": {
        "algorithm_id": "qgis:checkvalidity",
        "artifact_parameter_name": "INPUT_LAYER",
        "output_name": "INVALID_COUNT",
        "negate_output": true
      }
    },
    {
      "name": "CRS is EPSG:4326",
      "description": "Layer's Coordinate Reference System is lat-lon on WGS84 datum (i.e. EPSG code 4326)",
      "guide": "Open the layer properties dialog, then navigate to the 'information' tab (should be the first one) and in the section called 'Information from provider' check if the 'CRS' field has a value of 'EPSG:4326 - WGS84 - Geographic'",
      "automation": {
        "algorithm_id": "dataset_qa_workbench:crschecker",
        "artifact_parameter_name": "INPUT_LAYER",
        "output_name": "OUTPUT",
        "negate_output": false,
        "extra_parameters": {
          "INPUT_CRS": "EPSG:4326"
        }
      }
    }
  ],
  "report": {
    "algorithm_id": "dataset_qa_workbench:reportmailer"
  }
}
```

This plugin's code repository also features a 
[collection of sample checklists] that may be studied in order to get a better
grasp on how to define new checklists.

Each checklist has the following mandatory properties:

- `name` - Name of the checklist. This is used as the checklist identifier in 
  the QGIS UI, therefore a checklist's name must be unique;
  
- `description` - A short text explaining what the checklist is about;

- `dataset_type` - The type of dataset that this checklist operates on. It 
  must be one of:
    
  - `document`;
  - `raster`;
  - `vector`.
    
- `validation_artifact_type` - The type of artifact that this checklist 
  operates on. It must be one of:
    
  - `dataset`;
  - `metadata`;
  - `style`.
  
A checklist may also have the following optional properties:

- `checks` - A checklist may have a list of checks, that describe each 
  individual validation step. 
  
  - Each checklist check is defined as a JSON object. It has the following 
    mandatory properties:

    - `name` - Name of the checklist step;
    - `description` - A short description explaining what the check is about;
    - `guide` - Small text specifying how a human operator might go ahead and 
      validate this check.

  - A checklist check may also have the following optional properties:
  
    - `automation` - A JSON object that contains the configuration for the 
      automated execution of this validation check.
      
      The `automation` object has the following mandatory properties:
      
      - `algorithm_id` - Identifier of the QGIS Processing algorithm used to 
        perform automation. It takes the form `provider:algorithm` (e.g. 
        `qgis:checkvalidity`). It can be retrieved from the QGIS Processing 
        toolbox by resting the mouse pointer on top of the desired algorithm
        
      The `automation` object may have the following optional properties:
      
      - `artifact_parameter_name` - Name of the algorithm parameter that 
        specifies which of the algorithm's parameters represents the artifact 
        currently being validated (e.g. `INPUT`, `INPUT_LAYER`). If not 
        specified, it will default to `INPUT_LAYER` - This value may be 
        retrieved from the Processing algorithm by opening up the algorithm’s 
        dialog and resting the mouse pointer on the relevant input;
        
      - `output_name` - Name of the algorithm parameter that specifies which 
        one of the algorithm's outputs holds the result of the validation. If
        not specified, it will default to `OUTPUT`. This value may also be 
        retrieved from the algorithms dialog, in a similar 
        fashion as the `artifact_parameter_name` property;
        
      - `negate_output` - Whether to interpret a falsy result coming 
        from the processing algorithm as a sign of success in the validation. 
        This is sometimes desirable. Example: the `qgis:checkvalidity` 
        algorithm will return zero invalid features if a layer does not have 
        any invalid geometries. In this case, the zero must be interpreted as 
        a successful validation;
        
      - `extra_parameters` - Any additional parameters necessary for running 
        the processing algorithm. These may be used for configuring other 
        stuff related to the algorithm, such as the geometry validity 
        method (in the case of the `qgis:checkvalidity` algorithm). They are 
        passed straight to Processing. This property must be a JSON object.

- `report` - A JSON object with configuration of for a post validation action.
  This action is implemented by means of an additional Processing algorithm, 
  which is fed the generated validation report as an input, was well as any 
  other parameters that may be needed. A `report` has the following mandatory 
  properties:
  
  - `algorithm_id` - Identifier of the QGIS Processing algorithm (or model) 
    used to perform the post validation. It is specified in a similar way as 
    the `check.algorithm_id` property, specified above
    
  A `report` may also have the following optional properties:
  
  - `extra_parameters` - Any additional parameters necessary for running 
    the processing algorithm. These may be used for configuring other 
    stuff related to the algorithm and have a similar description as the one
    mentioned above for the `check.extra_parameters` property


### Processing algorithms suitable for use in checklist steps

In order to be suitable for use as an automated validation step, a Processing
algorithm must define some output that can be used for attesting whether
the step succeeds or not. This means that it must be possible to convert the
output to a `True/False` value.

Most default QGIS Processing algorithms simply output a map layer with their 
results. These are not suitable for use in automated checklist validation 
steps as there is no clear way to determine the success condition for the 
validation. Other algorithms, like the `qgis:checkvalidity` algorithm, output
both map layers and suitable numeric output results. These are suitable for 
using for automated validation.

The QGIS Dataset QA Workbench algorithm provides some custom Processing 
algorithms that are specifically tailored for automated validation. These
are also likely to increase in number in the future, as new version of the 
plugin are released. The current list of algorithms is:

- `dataset_qa_workbench:crschecker` - Allows checking if a layer's CRS 
  matches an expected value
  
- `dataset_qa_workbench:xmlchecker` - Allows checking if an XML file has the
  specified elements/attributes/values
  
  
You may also design your own custom Processing algorithms and then distribute
them via the [QGIS Resource Sharing] plugin so that users can use them together
with checklists from QGIS Dataset QA Workbench plugin.


### Processing algorithms suitable for use in post validation actions

In a similar fashion to the algorithms used for automating validation, the
Processing algorithms that can be used to execute post validation actions also
have specific requirements. It is not possible to reuse the standard QGIS 
Processing algorithms for this purpose. This plugin also ships with some 
suitable post validation algorithms, and it is also likely that future releases
will expand on the list. Current algorithms suitable for post validation 
actions:

- `qa_dataset_workbench:reportmailer` - Allows sending a copy of the 
  validation report via email
  
- `qa_dataset_workbench:reportposter` - Allows posting the validation report
  to an online server that implements a suitable REST API.


### Validating custom checklists

The structure of checklists is formally defined in a [JSON Schema] file that is 
part of the plugin’s source code. This file can be inspected at:

https://raw.githubusercontent.com/kartoza/qgis_dataset_qa_workbench/master/schemas/checklist-check.json 

JSON Schema files can be used to validate that a specific json object 
validates the schema. As such, it is desirable that every custom checklist be 
validated against the schema in order to ensure it works with the 
QGIS Dataset QA Workbench plugin. 

This validation may be done by either:

- using the `jsonschema` python package. Example:

  ```
  # validating checklist with local jsonschema package
  pipenv run jsonschema -i checklist-file.json schemas/checklist-check.json
  ```

- using any online json schema validator, such as the one available at:

  https://www.jsonschemavalidator.net/ 

- [Your IDE] might also support validating json files with a json schema.


## Sharing checklists and Processing algorithms with other users

This plugin leans on the capabilities provided by the [QGIS Resource Sharing] 
plugin and thus users are able to leverage that to share:

- Checklists
- Processing algorithms
- Processing models

We recommend setting up a git repository with the following structure:

```
my-shareable-qgis-resources/
  metadata.ini
  collections/
    my-shareable-collection/
      checklists/
        checklist1.json
        checklist2.json
      processing/
        algorithm1.py
        algorithm2.py
      models/
        model1.model3
        model2.model3
```

Then put all checklists, algorithms, etc that are to be shared in there and
simply provide the git repo's URL to your users. Consult the documentation of
the [QGIS Resource Sharing] plugin for further information on this.


## Development

This plugin uses [poetry] and [typer] for development.

An easy way to get started is to (fork and) clone this repo, install poetry 
and install it!

```
sudo apt install pyqt5-dev-tools

poetry install
```

### Installing locally

Call the `install` task:

```
poetry run python pluginadmin.py install
```


### Running tests

```
poetry shell
cd ~/.local/share/QGIS/QGIS3/profiles/default/python/plugins
python -m pytest -v -x ~/dev/qgis_dataset_qa_workbench
```



## Attribution

This plugin uses icons from the [Font Awesome] project. Icons are used as-is, 
without any modification, in accordance with their [license].

[official QGIS plugins repository]: https://plugins.qgis.org/
[QGIS Resource Sharing]: https://github.com/QGIS-Contribution/QGIS-ResourceSharing
[json]: https://www.json.org/json-en.html
[checklist schema]: https://raw.githubusercontent.com/kartoza/qgis_dataset_qa_workbench/master/schemas/checklist-check.json
[collection of sample checklists]: https://github.com/kartoza/qgis_dataset_qa_workbench/tree/master/collections/dataset-qa-workbench-demo-checklists/checklists
[Font Awesome]: https://fontawesome.com/
[license]: https://fontawesome.com/license
[poetry]: https://python-poetry.org/
[typer]: https://typer.tiangolo.com/
[jsonschema]: https://json-schema.org/
[Your IDE]: https://www.jetbrains.com/help/pycharm/json.html#ws_json_schema_add_custom
