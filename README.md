# Apigee Proxy: Notifications API
This Apigee proxy manages notifications, providing endpoints for various operations while ensuring security through token validation.

## Endpoints
| HTTP Method	 | Path	                            | Description                           |
|--------------|----------------------------------|---------------------------------------|
| GET          | /notifications                   | Returns all notifications             |
| GET          | /notifications/{id}              | Returns a specific notification       |
| DELETE       | /notifications/{id}              | Deletes a specific notification       |
| PATCH        | /notifications/{id}/mark-as-read | Marks a specific notification as read |

## Configuration Overview
- Base Path: The base path for this proxy is /notifications.
- Route Rule: Routes valid requests to a defined backend service.
- Token Validation: Included in the PreFlow to ensure authorization for all requests.

## Flows
1. Get All Notifications: Handles GET requests to retrieve all notifications.
2. Get Notification by ID: Handles GET requests for specific notifications.
3. Delete Notification by ID: Handles DELETE requests to remove notifications.
4. Mark Notification as Read: Handles PATCH requests to mark notifications as read.
5. Block All Other Requests: Rejects any requests that do not match the defined flows.

This proxy ensures secure access to notification management while blocking unauthorized requests.

## Repository
For more details on the Notification API, visit the [Notification API Repository](https://github.com/GilbertoJNJ/NotificationAPI).