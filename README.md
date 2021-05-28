# MS-blog-efficient
A blog app example using Express and React. Demonstrates use of microservices architecture efficiently by using an event bus and query service to rid of unnecessary multitudes of requests. PLEASE NOTE: This is not a production-ready application. It is only meant to serve as an introductory example to microservices architecture.

## Available Scripts

In all 6 of the directories under the root directory, run:

### `npm install` then `npm start` to start up the entire project.

## Basic Structure

The client uses React for the front end. Creating a post through front-end form submission will send a request to the "posts" service, which will emit an event to the event bus. The event bus will then reach out to all other services- comments, query, and moderation to communicate this event. The event bus keeps track of all events fired in a local data structure as well (in order to allow event syncing for the query service).
Creating a comment will also cause the event-bus to fire this event, which, along with any posts, will be sent to the query service to keep track of the posts and all subsequent comments. This allows for more efficient data retrieval for allowing the client to receive all post and comment data from ONLY the query service.
The moderation service handles the logic for determing which comments can stay and which ones are rejected (right now, it will reject any comment that contains the string "orange").

## Acknowledgements

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

This project is based off of Stephen Grider's Microservices with Node JS and React Udemy course: https://www.udemy.com/course/microservices-with-node-js-and-react/.