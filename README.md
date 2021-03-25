# pinpoint-journey-events-attribution

**Background**

Marketing campaigns come with associated cost. Marketers use KPIs to assess their effectiveness and calculate ROI. While Pinpoint provides certain metrics such as email open/read, it does not allow marketers to attribute any custom events to Campaigns or Journeys. The latter results to a marketing spend without the possibility of ROI calculation and not knowing what works well for customers. 

**Ideal state**

When the marketer is launching an email campaign, they should have a KPI, while the email itself should contain a CTA with a goal aligned to the KPI above such as purchase, subscription or code activation. Email campaign goals are usually on site events and if the customer has clicked or read the email, then that customer's events should be attributed to that email campaign. In the end of an email campaign, the marketer should be able to assess its effectiveness and calculate its ROI.

**Solution**

The solution is enabling marketers to attribute Pinpoint custom events following a customer's interaction with an email, SMS or custom channel. Additionally marketers are able to define a lookback window on a Pinpoint application level. The solution is utilising Cognito for its user attributes' storage, Pinpoint Journeys, CloudWatch, Lambda and DynamoDB. The solution is applicable only for Pinpoint Journeys and the customer's custom events can be attributed only under one marketing campaign at a time. If a customer interacts with a new marketing campaign while they are already in one, then the new one will overwrite the old and any new custom events will be attributed to the new email campaign. 

**Considerations**

1)	You will need to install Amplify SDK for sending events to Pinpoint and Cognito for user management
2)	All Pinpoint users should have a user attribute with the Cognito username
  a. ![alt text](https://github.com/Pioank/pinpoint-journey-events-attribution/blob/main/Images/CognitoUserID.JPG)
3)	Create a custom Cognito user attribute named “campaign”
4)	All Pinpoint custom events sent with Amplify should include an event attribute named “campaign” = Cognito user attribute “campaign”
5)	The solution is deployed on a Pinpoint application level
6)	Applicable only for Pinpoint Journeys and requires a multivariate split step within the journey design
7)	Push notification channel requires additional work 
8)	Supports only one campaign per user at a time and if a user enters to a second campaign then the new campaign will overwrite the old one

## Architecture
![alt text](https://github.com/Pioank/pinpoint-journey-events-attribution/blob/main/Images/SolutionArchitecture.JPG)

## Business Logic User Tagging
![alt text](https://github.com/Pioank/pinpoint-journey-events-attribution/blob/main/Images/BusinessLogic-UserTagging.JPG)

## Business Logic User Expiration
![alt text](https://github.com/Pioank/pinpoint-journey-events-attribution/blob/main/Images/BusinessLogic-UserExpiration.JPG)
