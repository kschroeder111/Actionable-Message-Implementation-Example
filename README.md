# Actionable Message Implementation Example

How to build a flow that uses actionable messages in Outlook. The who process of developing an adaptive card solution is a bit of hack that is not documented very well. Hopefully this information will save you time putting it all together. 

### Email Delivery using Actionable Messages
In this example we will be looking at 3 delivery states. What I mean is you can send the adaptive card and have it render in outlook or teams. Second delivery is an HTTP get or post from a button or link in the adaptive card. And the final delivery state is the ‘autoInvokeAction’ that is triggered every ticket the email is click on to be read. So a complete solution would have 3 different adaptive card syntax. First one does not have ‘autoInvokeAction’ and a button to trigger an inline update. The second card would not have the post. (or it could again but it gets complicated.) but rather the ‘autoInvokeAction’. Then finaly the endpoint of the second card would update the card with fresh data via a get/post from autoInvokeAction. 


# Step 1 – Design your base card 

There is an adaptable card generic online ide for card generation. There also is one specific to actionable messages which is 1.4 version of adaptive cards. The syntax of the card is slightly different for each delivery platform. 

TIPS: 

•	There are no error messages when your automation runs around the adaptive card syntax. You will get a generic 400 error. If you copy your adaptive card syntax under the RUN screen and past in AM Designer and it will give you more information on your syntax error. I found myself copying back and forth. 

•	You can add images to adaptive cards in two ways. 
1.	Via url. (http…) 
2.	Via Base64 encoded string. (data:image/png;base64,…)

•	Adaptive card height and width are fixed. Roughly 500x600. Anything longer is applied after the FOLD. (Meaning you need to use a scrollbar.) Each platform is slightly different. For Outlook you will have limited vertical space. Its best to keep your images on the thin side and remove extra spaces. 

Adaptive Card Designer | Actionable Message Designer (Recommended)

The way the IDE works is there is no login or save. You simply hit control-A (select all) and copy to clipboard. (control-C) 

Step 2 – Create your flow and add a text variable init or compose. 

Microsoft Power Automate or Logic App Flow is used to send and process adaptive cards. (Or actionable messages for Outlook.) 

•	Create a variable or compose item and store your adaptive card syntax there. 
•	If you wish to support dynamic cards in email you will need a second Flow to receive the post message when the user click the button or link.

IMPORTANT:

•	Your card will not work until you complete all steps including below. 

POST vs GET

If you post you need to specify a blank basic password or PAT. If you wish to use GET you do not need to auth. Both have different Flow syntax requirements. 

POST Syntax

GET Syntax

Static Card vs Dynamic Card



Step 3 – Copy Card to variable in flow

Step 4 – Replace OriginatorID with one you create for  your purposes. 

Step 5 – Relace your Header 


Troubleshooting

There are two errors you will get. 

401 – Unauthorized for various reasons. 

If you get a 401, verify your OriginatorID has been created and is updated at the top of your adaptive card syntax. Then verify your POST header as above. Note; you can use GET and bypass the header authentication. But parsing the url is required and its easier to add the header. Also note you can sign your card with a cert. 

400 – This is the most evil error I have ever faced. It basically is a catchall and says you messed up. 

The actionable message debugger is useless. Almost. Just view source and you get the same value. Anyways the 400 error will never have a log you can check. The best I have been able to determine is the issue usually is in the json variables you post from your card does not match the json variables the flow receiving the post. Secondly you will get a 400 error if the Flow receiving the POST has an error. In that case you can view the error in PAO. 

502 – Trappable Power Automate Error

The steps are very similar for Cards in Teams. There you can use the Adaptive Card format beyond 1.4. Actionable Messages are adaptive cards but 1.4 level. 

# Get Help
### Goolge Keyword Search


### AI Prompts


