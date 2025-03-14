---
layout: post
title: PPR
type: issues
comments: True
---


### **Personalized Project Reference**  

#### **1st Code Segment: Student-Developed Procedure**  
This defines a procedure that performs a key function in the Car Chat feature. It meets the requirements because:  
 **Defines a procedure (`create`)**  
 **Uses a parameter (`self`)**  
 **Implements sequencing (storing data), selection (error handling), and iteration (committing to the database)**  

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
```
![Description](https://i.postimg.cc/D0KqPkq9/Screenshot-2025-03-11-at-11-28-49-AM.png)

---  

#### **2nd Code Segment: Calling the Procedure**  
This calls the `create` method when a user submits a new chat message. It shows how the procedure is **integrated into the program**.  

```python
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
        new_message.create()  # Calls the create method to store the message
        return new_message.read(), 201
    except Exception as e:
        return {'error': str(e)}, 400
```
![Description](https://i.postimg.cc/6pTGR5k7/Screenshot-2025-03-11-at-11-30-17-AM.png)

---  

### **Why These Segments Work for PPR**  
 **First segment**: Defines a **student-developed procedure (`create`)** that includes **sequencing which adds to the database, selection which handles the error, and iteration commiting changes to the database**.  
 **Second segment**: Calls the **procedure (`create`)** in the `post` method, showing how it’s used within the program.  

These two segments align with CPT requirement

### ** This part of your API retrieves all messages stored in the database and returns them as a list:

python
Copy code
def get(self):
    messages = carChat.query.all()  # Fetches all messages from the database
    return [msg.read() for msg in messages], 200  # Converts them into a list format
Here, messages is a list that stores all message objects from the database.
The return [msg.read() for msg in messages] part transforms the list of objects into a readable format before sending it as a response.


### **Code Segment 2: Using Data from the List
This part demonstrates adding a new message to the database (modifying the list):

python
Copy code
@token_required()
def post(self):
    parser = reqparse.RequestParser()
    parser.add_argument('message', required=True, help="Message cannot be blank")
    
    args = parser.parse_args()
    current_user = g.current_user  # Retrieves the logged-in user

    new_message = carChat(
        message=args['message'],
        user_id=current_user.id
    )
    try:
        new_message.create()  # Adds the message to the database (list of messages)
        return new_message.read(), 201  # Returns the new message in a readable format
    except Exception as e:
        return {'error': str(e)}, 400
This segment adds new data to the list (carChat.query.all() from the first segment).
new_message.create() effectively updates the collection of messages, demonstrating CRUD functionality.

