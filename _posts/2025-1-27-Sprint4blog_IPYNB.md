---

---

## Sprint 4 individual blog




## **Purpose of Our Group's Program**
The purpose of our group program is to build a website that will allow users to share their thoughts, stories, and experiences on cars through various features.

## **Purpose of My Individual Feature(s)**
My individual feature focuses on developing a chat function for users to communitcate with eachother.


## **Using db_init, db_restore, db_backup**
- **Data Creation**: Use `db_init` to set up the initial database with test data. ./scripts/db_init.py
- **Data Recovery**: Use `db_restore` to recover data from backups, demonstrating how the application can recover from data loss.
- **Data Backup**: Use `db_backup` to create backups of the current database state.



- **Frontend Interaction**: Users can input messages through a text field in the chat interface. When they submit the form, a POST request is sent to the API to create a new message.
  - **Code Reference**: 
    ```javascript
    async function sendMessage(message) {
        const messageData = {
            "message": message,
            "user_id": 1  // Using the same user_id as shown in Postman
        };

        const response = await fetch(apiUrl, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(messageData)
        });
    }
    ```


## **Formatting Response Data (JSON) from API into DOM**
- In the `displayMessage` function:
    ```javascript
    function displayMessage({ text, type, time, userId, id }) {
        const messageDiv = document.createElement('div');
        messageDiv.innerHTML = `
            <div class="message-header">
                <span class="user-id">${userId}</span>
                <span class="timestamp">${timeString}</span>
            </div>
            <div class="message-text">${text}</div>
        `;
        chatBox.appendChild(messageDiv);
    }
    ```

## **Database Queries**
- **Extracting Python Lists**: 
  - Mention how queries from the database provide a Python list (rows).
  - **Code Reference**: In your `CarChatResource` class, the `get` method retrieves all messages:
    ```python
    def get(self):
        messages = CarChat.query.all()
        return [msg.read() for msg in messages], 200
    ```

## **Methods in Class**
- The `create`, `read`, `update`, and `delete` methods in the `CarChat` model:
    ```python
    def create(self):
        db.session.add(self)
        db.session.commit()

    def update(self):
        db.session.commit()

    def delete(self):
        db.session.delete(self)
        db.session.commit()
    ```

## **API Class**
- Discuss the API class code block used to perform GET, POST, PUT, and DELETE methods for managing chat messages.
  - **Code Reference**: The `CarChatResource` class in `api/carChat.py` defines these methods.

## **Method/Procedure in Class**
- Discuss a method or procedure in the class that contains sequencing, selection, and iteration, explaining how it processes data.
  - **Code Reference**: The `put` method in `CarChatResource` contains sequencing (checking if the message exists), selection (checking user authorization), and iteration (if applicable).


## **Call to Algorithm Request**
- The `fetch` call in the frontend JavaScript that sends a PUT request to the `/car_chat/<message_id>` endpoint:
    ```javascript
    async function editMessage(id, newMessageContent) {
        const response = await fetch(`http://127.0.0.1:8887/car_chat/${id}`, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ id, message: newMessageContent })
        });
    }
    ```


## **Changing Datas**
- How changing data or methods triggers different responses
  ```python
  @token_required()  # Ensure the user is authenticated
    def put(self):
        # Obtain the current user
        current_user = g.current_user
        # Obtain the request data
        data = request.get_json()
        
        # Find the current message from the database table(s)
        message = CarChat.query.get(data['id'])
        
        if message is None:
            return jsonify({"error": "Message not found"}), 404
        
        # Check if the current user is the owner of the message
        if message._user_id != current_user.id:
            return jsonify({"message": "You are not authorized to edit this message"}), 403
        
        # Update the message content
        message._message = data['message']  # Update with new message content
        message.update()  # Call the update method to save changes
        
        # Return response with updated message data
        return jsonify(message.read()), 200
    ```



***s


## **Input/Output Requests**
### **Demo Ways to Input to Your Full Stack Feature**
- **Frontend Interaction**: Users can input messages through a text field in the chat interface. When they submit the form, a POST request is sent to the API to create a new message.
  - **Code Reference**: 
    ```javascript
    async function sendMessage(message) {
        const messageData = {
            "message": message,
            "user_id": 1  // Using the same user_id as shown in Postman
        };

        const response = await fetch(apiUrl, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(messageData)
        });
    }
    ```

- **API Request and Response**: 
  - **Postman Example**: Demonstrate a raw API request to the `/car_chat` endpoint and the corresponding RESTful response, including error codes and JSON responses.

## **Using db_init, db_restore, db_backup**
- **Data Creation**: Use `db_init` to set up the initial database with test data.
- **Data Recovery**: Use `db_restore` to recover data from backups, demonstrating how the application can recover from data loss.
- **Data Backup**: Use `db_backup` to create backups of the current database state.

## **List Requests**
- **Use of Lists and Dictionaries**: 
  - Discuss how lists (rows) and dictionaries (columns) are used in the database.
  - **Code Reference**: In your `CarChat` model, the `read` method returns a dictionary representation of a message:
    ```python
    def read(self):
        return {
            'id': self.id,
            'message': self._message,
            'user_id': self._user_id,
            'timestamp': self._timestamp.isoformat() if self._timestamp else None
        }
    ```

## **Formatting Response Data (JSON) from API into DOM**
- Discuss how the API response data is formatted as JSON and how it is integrated into the Document Object Model (DOM) for display in the chat application.
  - **Code Reference**: In the `displayMessage` function:
    ```javascript
    function displayMessage({ text, type, time, userId, id }) {
        const messageDiv = document.createElement('div');
        messageDiv.innerHTML = `
            <div class="message-header">
                <span class="user-id">${userId}</span>
                <span class="timestamp">${timeString}</span>
            </div>
            <div class="message-text">${text}</div>
        `;
        chatBox.appendChild(messageDiv);
    }
    ```

## **Database Queries**
- **Extracting Python Lists**: 
  - Mention how queries from the database provide a Python list (rows).
  - **Code Reference**: In your `CarChatResource` class, the `get` method retrieves all messages:
    ```python
    def get(self):
        messages = CarChat.query.all()
        return [msg.read() for msg in messages], 200
    ```

## **Methods in Class**
- Discuss the methods in the `CarChat` class that handle CRUD operations (Create, Read, Update, Delete) for chat messages.
  - **Code Reference**: The `create`, `read`, `update`, and `delete` methods in the `CarChat` model:
    ```python
    def create(self):
        db.session.add(self)
        db.session.commit()

    def update(self):
        db.session.commit()

    def delete(self):
        db.session.delete(self)
        db.session.commit()
    ```

## **Algorithmic Code Request**
- Show the definition of code blocks that handle requests, including the logic for processing incoming data.
  - **Code Reference**: The `put` method in `CarChatResource` handles the editing of messages:
    ```python
    @token_required()
    def put(self):
        data = request.get_json()
        message = CarChat.query.get(data['id'])
        message._message = data['message']
        message.update()
    ```

## **API Class**
- Discuss the API class code block used to perform GET, POST, PUT, and DELETE methods for managing chat messages.
  - **Code Reference**: The `CarChatResource` class in `api/carChat.py` defines these methods.

## **Method/Procedure in Class**
- Discuss a method or procedure in the class that contains sequencing, selection, and iteration, explaining how it processes data.
  - **Code Reference**: The `put` method in `CarChatResource` contains sequencing (checking if the message exists), selection (checking user authorization), and iteration (if applicable).

## **Parameters and Return Type**
- Discuss the parameters (body of the request) and the return type (using `jsonify`) of the function that handles API requests.
  - **Code Reference**: The `put` method takes a JSON body with the message ID and new content, returning a JSON response:
    ```python
    return jsonify(message.read()), 200
    ```

## **Call to Algorithm Request**
- Show the definition of code blocks that make requests to the API.
  - **Code Reference**: The `fetch` call in the frontend JavaScript that sends a PUT request to the `/car_chat/<message_id>` endpoint:
    ```javascript
    async function editMessage(id, newMessageContent) {
        const response = await fetch(`http://127.0.0.1:8887/car_chat/${id}`, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ id, message: newMessageContent })
        });
    }
    ```

## **Handling Data**
- Discuss how the call/request to the method with the algorithm (using `fetch` to the endpoint) is structured.
- Explain the return/response from the method with the algorithm (using `fetch`) and how the data is handled.
  - **Code Reference**: The response handling in the `editMessage` function in the frontend.

## **Changing Data or Method Triggers**
- Show how changing data or methods triggers different responses, specifically addressing normal conditions and error conditions.
  - **Code Reference**: Discuss how the `put` method handles both successful updates and error conditions (e.g., message not found, unauthorized access).