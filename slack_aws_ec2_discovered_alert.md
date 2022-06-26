	1. Make sure AWS Config is recording
		a. AWS Config > Settings
		b. Recording = on
		c. General Settings > Resource types to record: EC2 Instance
	2. Create SNS Topic
		a. SNS > Topics > Create Topic
		b. Standard > Create a name > tag as necessary > Create topic
	3. Create Slack App/Webhook
		a. Settings & Administration > Workplace Settings > Configure Apps
		b. Search App Directory > type "Incoming Webhooks" > Add to Slack
		c. Post to channel > choose appropriate channel > click add webhooks integration
		d. note  "Post to channel", "webhook url", and "customize name"
                e. Save Settings
        4. Create Lambda function
		a. Lambda > Create Function > Author from Scratch
		b. Give Function a name
		c. Runtime = Python 3.6
		d. Create function
                e. Paste the following in the lambda_function.py (but update url, channel and username)

                ```
                #!/usr/bin/python3.6
                import urllib3
	        import json
		http = urllib3.PoolManager()
		def lambda_handler(event, context):
		    url = "SLACK_WEBHOOK_URL"
		    msg = {
		        "channel": "#CHANNEL",
		        "username": "WEBHOOK_USERNAME",
		        "text": event['Records'][0]['Sns']['Message'],
		        "icon_emoji": ""
		    }
		    
		    encoded_msg = json.dumps(msg).encode('utf-8')
		    resp = http.request('POST',url, body=encoded_msg)
		    print({
		        "message": event['Records'][0]['Sns']['Message'], 
		        "status_code": resp.status, 
		        "response": resp.data
                 })
                 ```

        5. add trigger > SNS > Select lambda created above > Add
	6. Create EventBridge rule
		a. EventBridge > Rules > Create rule
		b. Give it a name
		c. "Rule with an event Pattern" > Next
		d. Event Source : AWS events or EventBridge partner events
                e. Event Pattern > Custom Patters (JSON editor)  ***may need to tweak formatting a bit
                ```
                {
		  "source": ["aws.config"],
		  "detail-type": ["Config Configuration Item Change"],
		  "detail": {
		    "messageType": ["ConfigurationItemChangeNotification"],
		    "configurationItem": {
		      "resourceType": ["AWS::EC2::Instance"],
		      "configurationItemStatus": ["ResourceDiscovered"]
		    }
		  }
                }
               ```
                
                f. next
		g. Target 1 > AWS Service > SNS topic > Select SNS topic created > Additional Settings
		h. Configure target input > Input transformer > Configure Input transformer
		i. Target Input transformer:
			{
			  "awsAccountId": "$.detail.configurationItem.awsAccountId",
			  "awsRegion": "$.detail.configurationItem.awsRegion",
			  "configurationItemCaptureTime": "$.detail.configurationItem.configurationItemCaptureTime",
			  "resource_ID": "$.detail.configurationItem.resourceId",
			  "resource_type": "$.detail.configurationItem.resourceType"
			}
		```

		j. template
		i. "On <configurationItemCaptureTime> AWS Config service recorded a creation of a new <resource_type> with Id <resource_ID> in the account <awsAccountId> region <awsRegion>. For more details open the AWS Config console at https://console.aws.amazon.com/config/home?region=<awsRegion>#/timeline/<resource_type>/<resource_ID>/configuration"
		k. Confirm

References:
https://aws.amazon.com/premiumsupport/knowledge-center/config-email-resource-created/
https://aws.amazon.com/premiumsupport/knowledge-center/sns-lambda-webhooks-chime-slack-teams/
