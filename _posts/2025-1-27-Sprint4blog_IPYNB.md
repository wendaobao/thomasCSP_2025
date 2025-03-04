---

---


## **Purpose of Our Group's Program**
The purpose of our group program is to build a website that will allow users to share their thoughts, stories, and experiences on cars through various features.

## **Purpose of My Individual Feature**
The Car Chat feature in our project allows users to engage in real-time conversations about cars. Users can send, update, and delete messages while ensuring authentication and security through JWT-based authorization. This feature is built using Flask, SQLAlchemy, and RESTful APIs to provide an interactive chat experience.

## **CPT Requirement Highlights**

This project aligns with College Board CPT (Create Performance Task) requirements by demonstrating:

1. Program Purpose & Function

The Car Chat Feature enables user interaction through a structured messaging system, allowing real-time discussions on car-related topics.

Users can create, update, and delete messages, ensuring full CRUD functionality.

```
@token_required()  # Ensures only authenticated users can send messages
def post(self):
    parser = reqparse.RequestParser()
    parser.add_argument('message', required=True, help="Message cannot be blank")
    args = parser.parse_args()

    current_user = g.current_user  # Retrieves the logged-in user

    new_message = carChat(
        message=args['message'],
        user_id=current_user.id  # Links message to the user
    )
    
    try:
        new_message.create()  # Stores the message in the database
        return new_message.read(), 201
    except Exception as e:
        return {'error': str(e)}, 400
```

2. Algorithm Implementation

The backend processes API requests with Flask, handling user authentication, message storage, and updates.

User messages are processed through functions that validate input, update databases, and return JSON responses.

```
def get(self):
    messages = carChat.query.all()  # Fetch all messages from the database
    return [msg.read() for msg in messages], 200  # Returns JSON response
```

3. Data Abstraction

Messages are stored in a structured SQL database using SQLAlchemy models, ensuring organized and retrievable data.

Relationships between users and messages are defined using foreign keys to maintain proper data integrity.

```
class carChat(db.Model):
    __tablename__ = 'carChats'

    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    _message = db.Column(db.String(255), nullable=False)
    _user_id = db.Column('user_id', db.Integer, db.ForeignKey('users.id'))  # Foreign key links message to user
    _timestamp = db.Column('_timestamp', db.DateTime, default=datetime.utcnow)

    def __init__(self, message, user_id):
        self._message = message
        self._user_id = user_id
        self._timestamp = datetime.utcnow()

    def read(self):
        """Returns chat message data with user details"""
        user = User.query.get(self._user_id)
        return {
            'id': self.id,
            'message': self._message,
            'username': user.name if user else "Unknown User",
            'timestamp': self._timestamp.isoformat()
        }
```
4. Managing Complexity

The program breaks functionality into modular components, including separate models for users and messages.

API routes are well-structured, allowing smooth communication between the frontend and backend.

```
carChat_api = Blueprint('carChat_api', __name__, url_prefix='/api')
api = Api(carChat_api)

api.add_resource(carChatResource, '/carChat')
```

5. Development Process

The project follows an iterative development process, incorporating debugging, user testing, and real-world API interactions.

Continuous improvements, such as UI enhancements and error handling, demonstrate a structured approach to development.

```
try:
    message.delete()
    return {'message': 'Deleted successfully'}, 200
except Exception as error:
    db.session.rollback()  # Prevents database corruption
    return {'error': str(error)}, 500
```


## **Methods in Class**
- The `create`, `read`, `update`, and `delete` methods in the `CarChat` model:
- My backend uses API to handle responses for all the main actions: post, get, put, and delete. When we send a new message, it triggers a function to refresh the page, and then it uses the get method to update the page with the latest data from the backend database. The get method simply updates the chat with the current messages. For the put method, it sends a request to the server, and once the server responds, it updates the the message accordingly to the ID given to each individual message. The delete method removes a row from both the database and the displays. 
- This fulfills the performance task by having at least one procedure that contributes to the program's intended purpose. For example, 
    - Procedure Name: create()
    - Return Type: dict (Returns the stored chat message details in dictionary format) 
    - Parameters: self (instance of CarChat)
    - Purpose: Stores a new chat message in the database.
Ensures that the message is committed and available for retrieval.
Returns the saved message details for confirmation.
Handles errors by rolling back changes if insertion fails.

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
## **Algorithm Requests**
The put() method in CarChat contains sequencing, selection, and iteration, making it a valid algorithm for the performance task.
- Sequencing:
The method retrieves messages using CarChat.query.all().
- Selection:
If no messages exist, it returns an error message.
- Iteration:
It iterates over each message using list comprehension [msg.read() for msg in messages].

```python
    class CarChatResource(Resource):  # Renamed to avoid confusion with model
    def get(self):
        messages = CarChat.query.all()
        return [msg.read() for msg in messages], 200

    def post(self):
        parser = reqparse.RequestParser()
        parser.add_argument('message', required=True, help="Message cannot be blank")
        parser.add_argument('user_id', type=int, required=True, help="User ID cannot be blank")
        args = parser.parse_args()

        new_message = CarChat(
            message=args['message'],
            user_id=args['user_id']
        )
        try:
            new_message.create()
            return new_message.read(), 201
        except Exception as e:
            return {'error': str(e)}, 400
    
        
    @token_required()  # Ensure the user is authenticated
    def delete(self):
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
            return jsonify({"message": "You are not authorized to delete this message"}), 403
        
        # Delete the message using the ORM method defined in the model
        message.delete()
        
        # Return response
        return jsonify({"message": "Message deleted successfully"}), 200

  # Ensure the user is authenticated
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


## **Using db_init, db_restore, db_backup**
- **Data Creation**: Use `db_init` to set up the initial database with test data. ./scripts/db_init.py
- **Data Recovery**: Use `db_restore` to recover data from backup
- **Data Backup**: Use `db_backup` 


**Frontend Interaction**: Users can input messages through a text field in the chat interface. When they submit the form, a POST request is sent to the API to create a new message.
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
     if (response.ok) {
                        return await response.json(); // Return the message data including ID
                    } else {
                        console.error('Error sending message:', response.statusText);
                    }
                } catch (error) {
                    console.error('Error:', error);
                }
                return null;
            }
    ```


## **Formatting Response Data (JSON) from API into DOM**
- The displayMessage function formats the response data received from the API (in JSON format) and creates a corresponding HTML structure to display the message in the chat interface. 
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
  ```





## **Input/Output Requests**
- **Frontend Interaction**: Users can input messages through a text field in the chat interface. When they submit the form, a POST request is sent to the API to create a new message.
 
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


## **Parameters and Return Type**
- parameters (body of the request) and the return type (using `jsonify`) of the function that handles API requests.
- The `put` method takes a JSON body with the message ID and new content, returning a JSON response:
    ```python
    return jsonify(message.read()), 200
    ```


