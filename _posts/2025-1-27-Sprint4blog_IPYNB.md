---

---

## Sprint 4 individual blog




## **Purpose of Our Group's Program**
The purpose of our group program is to build a website that will allow users to share their thoughts, stories, and experiences on cars through various features.

## **Purpose of My Individual Feature**
My individual feature focuses on developing a chat function for users to communitcate with eachother.


## **Using db_init, db_restore, db_backup**
- **Data Creation**: Use `db_init` to set up the initial database with test data. ./scripts/db_init.py
- **Data Recovery**: Use `db_restore` to recover data from backup
- **Data Backup**: Use `db_backup` 


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

## **Methods in Class**
- The `create`, `read`, `update`, and `delete` methods in the `CarChat` model:
    ```python
        def create(self):
        """
        The create method adds the object to the database and commits the transaction.
        
        Uses:
            The db ORM methods to add and commit the transaction.
        
        Raises:
            Exception: An error occurred when adding the object to the database.
        """
        try:
            db.session.add(self)
            db.session.commit()
        except Exception as e:
            db.session.rollback()
            raise e
    
    def read(self):
        """Returns chat message data with user details"""
        user = User.query.get(self._user_id)
        if user:
            user_data = user.read()
            username = user_data.get("name", "Unknown User")  # Get actual username
        else:
            username = "Unknown User"
            
        return {
            'id': self.id,
            'message': self._message,
            'username': username,  # Include username in response
            'user_id': self._user_id,
            'timestamp': self._timestamp.isoformat() if self._timestamp else None  # Changed to ISO format
        }

    def update(self):
        """
        The update method commits the transaction to the database.
        
        Uses:
            The db ORM method to commit the transaction.
        
        Raises:
            Exception: An error occurred when updating the object in the database.
        """
        try:
            db.session.commit()
        except Exception as error:
            db.session.rollback()
            raise error
        
    def delete(self):
        """
        The delete method removes the object from the database and commits the transaction.
        
        Uses:
            The db ORM methods to delete and commit the transaction.
        
        Raises:
            Exception: An error occurred when deleting the object from the database.
        """    
        try:
            db.session.delete(self)
            db.session.commit()
        except Exception as error:
            db.session.rollback()
            raise error

    ```


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

## **List Requests**
- **Use of Lists and Dictionaries**: 
  - The `CarChat` model, the `read` method returns a dictionary representation of a message:
    ```python
    def read(self):
        return {
            'id': self.id,
            'message': self._message,
            'user_id': self._user_id,
            'timestamp': self._timestamp.isoformat() if self._timestamp else None
        }
    ```


## **Database Queries**
- **Extracting Python Lists**: 
  `CarChatResource` class, the `get` method retrieves all messages:
    ```python
    def get(self):
        messages = CarChat.query.all()
        return [msg.read() for msg in messages], 200
    ```


## **Method/Procedure in Class**
  - **Code Reference**: The `put` method in `CarChatResource` contains sequencing (checking if the message exists), selection (checking user authorization), and iteration,

## **Parameters and Return Type**
- parameters (body of the request) and the return type (using `jsonify`) of the function that handles API requests.
  - **Code Reference**: The `put` method takes a JSON body with the message ID and new content, returning a JSON response:
    ```python
    return jsonify(message.read()), 200
    ```


