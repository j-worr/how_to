## Configuring email on Jenkins with SES

### Pre-requisite(s)
1. AWS Account
2. SES already configured ( https://docs.aws.amazon.com/ses/latest/DeveloperGuide/setting-up-email.html)
3. SES SMTP credentials ( https://docs.aws.amazon.com/ses/latest/DeveloperGuide/smtp-credentials.html)
  
  
### Steps
1. Access Jenkins
2. Click “Manage Jenkins” then “Configure System”
3. Scroll to “Jenkins Location” and input a “noreply” email address for “System Admin e-mail address. I used something like noreply@my.domain.***

***Without setting this value, an error will occur: “554 Message rejected: Email address is not verified. The following identities failed the check in region US-EAST-1: address not configured yet <nobody@nowhere>, nobody@nowhere“

4. Scroll all the way down to “E-mail Notification” section
5. Apply SES SMTP settings:
```
• SMTP Server = email-smtp.REGION.amazonaws.com
• Check box to “Use SMTP Authentication” and fill in required data:
• User Name: {Use SMTP credentials created when setting up SES}
• Password: {Use SMTP credentials created when setting up SES}
• Check box for “Use SSL”
• SMTP Port: 465
• Add a “Reply-To Address”; in my case, I added the value used in step #3.
• Click “Apply” to save settings
• Check box to “Test configuration by sending test e-mail”
• Test e-mail recipient: {your email address}
• Click “Test Configuration” button
• If successful, you will see “Email was successfully sent” message
• Click “Save” button
```
From <https://valenbb.wordpress.com/2018/01/01/setup-email-notifications-from-jenkins-using-aws-ses/> 


===============================================================================================


Email at the end of a Jenkins Job (draft)


1. Manage Jenkins > Configure System > populate Extended Email Notification same as Email notification section for (SMTP Server, SMTP Port, Credentials)
2. Add Post-build Action > Editable Email Notification
3. Project From: noreply@domain.com <---this needs to be an email address it will send from
4. Advanced Settings > Triggers > X Failure -Any
5. Add Trigger > Always (make sure you have the right groups)
6. Save
	
===============================================================================================
