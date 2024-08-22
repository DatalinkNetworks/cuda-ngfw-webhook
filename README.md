# cuda-ngfw-webhook
Deploy a webhook handler workflow for Barracuda NGFW events

### 1) Create Webhook Handler
Create a new trigger to receive webhooks from the Barracuda NGFW
![image](https://github.com/user-attachments/assets/a85ddc06-1f04-49a7-9d88-b70844f055eb)


### 2) Parse the JSON Request
Parse the body into a custom, useful JSON object
![image](https://github.com/user-attachments/assets/de3d98ff-d170-4de6-9664-68964f625ee0)

Content:

    {
      "title": "@{triggerOutputs()['body/title']}",
      "text": "@{triggerOutputs()['body/text']}",
      "hexcolor": "@{triggerOutputs()['body/themeColor']}",
      "message": "@{triggerOutputs()['body/sections'][0]['facts'][0]['value']}",
      "rating": "@{triggerOutputs()['body/sections'][0]['facts'][1]['value']}",
      "category": "@{triggerOutputs()['body/sections'][0]['facts'][2]['value']}",
      "layer": "@{triggerOutputs()['body/sections'][0]['facts'][3]['value']}",
      "class": "@{triggerOutputs()['body/sections'][0]['facts'][4]['value']}",
      "author": "@{triggerOutputs()['body/sections'][0]['facts'][5]['value']}",
      "time": "@{triggerOutputs()['body/sections'][0]['facts'][6]['value']}"
    }
    
Schema:

    {
        "type": "object",
        "properties": {
            "title": {
                "type": "string"
            },
            "text": {
                "type": "string"
            },
            "hexcolor": {
                "type": "string"
            },
            "message": {
                "type": "string"
            },
            "rating": {
                "type": "string"
            },
            "category": {
                "type": "string"
            },
            "layer": {
                "type": "string"
            },
            "class": {
                "type": "string"
            },
            "author": {
                "type": "string"
            },
            "time": {
                "type": "string"
            }
        }
    }

### 3) Send an AdaptiveCard object
Create a template using https://adaptivecards.io or use this one 
![image](https://github.com/user-attachments/assets/7f6a7dbd-6081-4a60-8a74-c768c91b6268)

    {
        "type": "AdaptiveCard",
        "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
        "version": "1.4",
        "body": [
            {
                "type": "TextBlock",
                "text": "@{body('Parse_NGFW_Event')?['title']}",
                "wrap": true,
                "separator": true,
                "size": "Medium",
                "weight": "Bolder",
                "spacing": "Small"
            },
            {
                "type": "TextBlock",
                "text": "@ @{body('Parse_NGFW_Event')?['time']}",
                "wrap": true,
                "spacing": "None",
                "size": "Small",
                "weight": "Lighter",
                "color": "Accent"
            },
            {
                "type": "TextBlock",
                "text": "**@{body('Parse_NGFW_Event')?['text']}:**",
                "wrap": true
            },
            {
                "type": "TextBlock",
                "text": "@{body('Parse_NGFW_Event')?['message']}",
                "wrap": true,
                "spacing": "None",
                "fontType": "Default",
                "color": "Light",
                "size": "Default",
                "weight": "Default"
            },
            {
                "type": "ColumnSet",
                "columns": [
                    {
                        "type": "Column",
                        "width": "stretch",
                        "items": [
                            {
                                "type": "TextBlock",
                                "text": "**Category:** @{body('Parse_NGFW_Event')?['category']}",
                                "wrap": true,
                                "spacing": "None"
                            },
                            {
                                "type": "TextBlock",
                                "text": "**Rating:** @{body('Parse_NGFW_Event')?['rating']}",
                                "wrap": true,
                                "spacing": "None"
                            }
                        ],
                        "spacing": "None",
                        "horizontalAlignment": "Left"
                    },
                    {
                        "type": "Column",
                        "width": "stretch",
                        "items": [
                            {
                                "type": "TextBlock",
                                "text": "**Class:** @{body('Parse_NGFW_Event')?['class']}",
                                "wrap": true,
                                "spacing": "None"
                            },
                            {
                                "type": "TextBlock",
                                "text": "**Layer:** @{body('Parse_NGFW_Event')?['layer']}",
                                "wrap": true,
                                "spacing": "None"
                            }
                        ]
                    }
                ],
                "style": "emphasis",
                "spacing": "Medium"
            }
        ]
    }

### End Result:

![image](https://github.com/user-attachments/assets/8a9d6433-5108-41b7-a480-1d7c4989d18f)
