<a name="view-formviewtype-properties-formarmrequest"></a>
# view-formViewType-properties-formArmRequest
* [view-formViewType-properties-formArmRequest](#view-formviewtype-properties-formarmrequest)
    * [Definitions:](#view-formviewtype-properties-formarmrequest-definitions)
    * [Sample Snippet](#view-formviewtype-properties-formarmrequest-sample-snippet)

<a name="view-formviewtype-properties-formarmrequest-definitions"></a>
## Definitions:
<a name="view-formviewtype-properties-formarmrequest-definitions-an-object-with-the-following-properties"></a>
##### An object with the following properties
| Name | Required | Description
| ---|:--:|:--:|
|method|True|method for Arm Request. See [here](dx-enum-formArmRequest-method.md) for more information on method.
|path|True|path for Arm Request
|body|False|body for Arm Request
|fx.feature|False|
<a name="view-formviewtype-properties-formarmrequest-sample-snippet"></a>
## Sample Snippet
  ### Executing ARM request (command) for a resource

If you need to add a command to execute, for example, POST action for your resource, you need to add a command to your view and reference Form blade that implements ARM request scenario.

Supported methods: PUT, POST, PATCH

Patch is available from SDK 6.724.0.5. parameters

Form sample:

Takes resource id in parameters, specifies apiversion for the resource in resources, says armrequest as an action, constructs path using resources() function, passes body using Form inputs (could be any function, resources(), steps()…)

FormBladeArmRequest.Dx.json:

```json
{
  "$schema": "../../../Definitions/dx.schema.json",
  "view": {
    "kind": "Form",
    "parameters": [
      {
        "name": "id",
        "type": "key"
      }
    ],
    "resources": [
      {
        "id": "[parameters('id')]",
        "apiVersion": "2014-04-01"
      }
    ],
    "properties": {
      "title": "Form blade: please fill in values",
      "steps": [
      ],
      "armRequest": {
        "path": "[concat(resources().id, '/actionName?api-version=2014-04-01')]",
        "method": "POST",
        "body": "[parse(concat('{\"location\":', string(resources().location), '}'))]"
      }
    }
  }
}
```
Command:

If you parent blade is a declarative blade: add the below into commands section.

```json
      {
        "icon": "MsPortalFx.Base.Images.ArrowUp",
        "id": "formarmrequestCommand",
        "kind": "OpenBladeCommand",
        "displayName": {
          "property": "formArmRequest"
        },
        "blade": {
          "name": "FormBladeArmRequest_Dx",
          "inContextPane": true,
          "parameters": {
            "id": "[parameters('id')]"
          }
        }
      },
```
From SDK 7.4.0.5 you may customize primary button label(optional) in view properties. Default label is “Submit”.

```json
"properties": {
    "primaryButtonLabel": "Execute ARM request",
}
```

