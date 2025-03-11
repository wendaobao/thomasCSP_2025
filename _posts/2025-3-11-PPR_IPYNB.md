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

---  

### **Why These Segments Work for PPR**  
 **First segment**: Defines a **student-developed procedure (`create`)** that includes **sequencing which adds to the database, selection which handles the error, and iteration commiting changes to the database**.  
 **Second segment**: Calls the **procedure (`create`)** in the `post` method, showing how it’s used within the program.  

These two segments align with CPT requirement

