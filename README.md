# Webhook Documentation for OneMetric
This document provides detailed information on how to set up and use webhooks with the OneMetric API. Webhooks allow you to receive real-time event notifications from OneMetric by configuring a URL endpoint on your server that OneMetric can send data to whenever specific events occur.

Overview
OneMetric supports webhooks to notify external systems about various events related to user activities and market-generated alerts. This documentation will guide you through setting up webhooks and provides information about available API endpoints, request formats, and data structures.

Table of Contents
Setting Up a Webhook
Supported Events
Event Payload Structure
Examples
Security Recommendations
Setting Up a Webhook
To receive event notifications, you need to provide OneMetric with a webhook URL. This URL should be publicly accessible and able to accept POST requests from OneMetric’s servers.

Define Your Webhook URL: Set up an endpoint on your server that can handle incoming POST requests. Make sure it is secure and follows your organization's security practices.
Register the Webhook URL with OneMetric: You can register your webhook URL with OneMetric using the "Details from user" API endpoint.
API: Register Webhook URL
URL: https://onemetric.in/api/webhook/register

Method: POST

Request Body:

json
Copy code
{
  "webhook_url": "https://yourdomain.com/webhook-endpoint"
}
Description: This API registers your webhook URL with OneMetric so that it will receive events as they occur.

Response:

Success: 200 OK with a message confirming registration.
Failure: 4XX error with details on what went wrong.
Supported Events
OneMetric currently supports several event types. Each event will have a unique event_type and may include a sub_type for additional specificity. Events will be sent to your webhook URL as they occur, using a POST request.

Common Event Types and Descriptions
Event Type	Sub Type	Description
MARKET_GENERATED_ALERT	PRICE_ALERT	Triggered when a specific price alert condition is met.
MARKET_GENERATED_ALERT	VOLUME_ALERT	Triggered when a specific volume alert condition is met.
USER_ACTIVITY	LOGIN	Sent when a user logs into their account.
USER_ACTIVITY	LOGOUT	Sent when a user logs out of their account.
TRANSACTION	NEW_PURCHASE	Sent when a new transaction is completed.
TRANSACTION	REFUND	Sent when a refund transaction occurs.
STOCK_WATCHLIST	STOCK_ADDED	Triggered when a user adds a stock to their watchlist.
STOCK_WATCHLIST	STOCK_REMOVED	Triggered when a user removes a stock from their watchlist.
USER_PROFILE	DETAILS_UPDATED	Sent when a user updates their profile information.
Note: Additional events may be supported in the future. Please refer to this documentation regularly for updates.

Event Payload Structure
Each event payload sent to your webhook endpoint will have a standardized structure. This ensures consistency across different event types and allows your system to process these notifications effectively.

Example Payload Structure
json
Copy code
{
  "event_type": "MARKET_GENERATED_ALERT",
  "sub_type": "PRICE_ALERT",
  "time_stamp": "2024-10-01 10:20:00",
  "details": {
    "ltp": 30,
    "close": 20,
    "symbol": "XYZ",
    "alert_threshold": 25,
    "price_change_percentage": 5.0
  },
  "user": {
    "id": "user_123456",
    "name": "John Doe",
    "email": "johndoe@example.com"
  }
}
Field Descriptions
event_type: The type of event being reported (e.g., MARKET_GENERATED_ALERT, USER_ACTIVITY, TRANSACTION).
sub_type: A subtype that further specifies the event (e.g., PRICE_ALERT, VOLUME_ALERT).
time_stamp: The date and time when the event occurred, in YYYY-MM-DD HH:MM:SS format (UTC).
details: An object containing event-specific data, which varies depending on the event_type and sub_type. Common fields within details include:
ltp (Last Traded Price)
close (Closing Price)
symbol (Stock symbol or asset identifier)
alert_threshold (Alert threshold value, if applicable)
price_change_percentage (Percentage change, if applicable)
user: Information about the user associated with the event (for user-specific events).
id: Unique identifier for the user.
name: Name of the user.
email: Email address of the user.
Examples
Example 1: Market Generated Alert (Price Alert)
json
Copy code
{
  "event_type": "MARKET_GENERATED_ALERT",
  "sub_type": "PRICE_ALERT",
  "time_stamp": "2024-10-01 10:20:00",
  "details": {
    "ltp": 30,
    "close": 20,
    "symbol": "ABC",
    "alert_threshold": 25,
    "price_change_percentage": 10.0
  }
}
Example 2: User Login Event
json
Copy code
{
  "event_type": "USER_ACTIVITY",
  "sub_type": "LOGIN",
  "time_stamp": "2024-10-01 11:45:00",
  "user": {
    "id": "user_789012",
    "name": "Jane Smith",
    "email": "janesmith@example.com"
  }
}
Example 3: Stock Watchlist Updated (Stock Added)
json
Copy code
{
  "event_type": "STOCK_WATCHLIST",
  "sub_type": "STOCK_ADDED",
  "time_stamp": "2024-10-01 12:00:00",
  "details": {
    "symbol": "DEF",
    "watchlist_name": "My Favorites"
  },
  "user": {
    "id": "user_123456",
    "name": "John Doe",
    "email": "johndoe@example.com"
  }
}
Security Recommendations
To ensure the security of your webhook endpoint and data, we recommend the following best practices:

Verify Source: Validate that requests are coming from OneMetric's IP addresses or include a shared secret token.
Use HTTPS: Ensure that your webhook URL uses HTTPS to encrypt data in transit.
Rate Limiting: Implement rate limiting on your endpoint to prevent abuse or accidental overload.
Request Validation: Verify the structure of incoming requests to match the documented payload structure.
Respond with HTTP 2xx: Send an HTTP 2xx response to acknowledge receipt of events. Failing to do so may result in retries from OneMetric.
Error Handling and Retries
If your endpoint does not respond with a 2xx status code, OneMetric will attempt to resend the event. Subsequent retry intervals and limits will depend on your subscription level with OneMetric.

For critical systems, make sure to handle these requests promptly and respond with the appropriate status code to avoid multiple retries.

Conclusion
Webhooks provide a powerful way to receive timely notifications about important events from OneMetric. By following this guide, you can set up and process webhook events efficiently to integrate OneMetric data into your applications and workflows.

For further assistance or if you have any questions, please reach out to our support team at support@onemetric.in.






