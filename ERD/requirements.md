Relationships Between Entities:

    The Airbnb ER diagram includes six entities: User, Property, Booking, Payment, Review, and Message. The relationships between these entities are defined by foreign keys and reflect how users interact with properties, bookings, payments, reviews, and messages. Below, I’ll list and explain each relationship in detail.

1. User to Property (Hosts Relationship)

       Definition: A User can host multiple Properties, but each Property is hosted by exactly one User.
   
       Cardinality: One-to-Many (1:N).
   
       Implementation: In the Property table, the host_id attribute is a foreign key that references User(user_id). This means every Property record must have a valid user_id from the User table, identifying the host.

Explanation:

    In Airbnb, some users act as hosts who list properties (e.g., apartments, houses) for rent. A single host can list multiple properties (e.g., a host might own several vacation homes). However, each property is managed by only one host, ensuring clear ownership.
    For example, if Jane (a User) is a host, she might list two properties: “Cozy Cabin” and “Beach House.” Both properties’ host_id would point to Jane’s user_id. No other user can claim to host these properties.

Why It Matters:
    
    This relationship ensures that properties are tied to a responsible host, who manages pricing, availability, and guest communication. It’s critical for accountability and for guests to know who they’re renting from.

Constraints:
    
    The foreign key constraint on host_id ensures that only valid users (existing user_id values) can be hosts.
    The role attribute in the User table (ENUM: guest, host, admin) likely identifies which users can host properties (those with role = host).

3. Property to Booking (Booked Relationship)

       Definition: A Property can have multiple Bookings, but each Booking is associated with exactly one Property.
   
       Cardinality: One-to-Many (1:N).
   
       Implementation: In the Booking table, the property_id attribute is a foreign key that references Property(property_id). Each Booking record must include a valid property_id to indicate which property is being booked.

Explanation:

    A property listed on Airbnb can be booked by many guests over time. For example, a property like “Cozy Cabin” might have bookings for different dates by different users (e.g., one booking for June 1–3, another for June 5–7). However, each booking is tied to one specific property—you can’t book multiple properties in a single booking record.
    For instance, if John books “Cozy Cabin” for a weekend, the Booking record will have a property_id that matches the “Cozy Cabin” property_id.

Why It Matters:

    This relationship is the core of Airbnb’s booking system, allowing properties to be rented out multiple times while keeping track of which property each booking pertains to. It ensures that availability and pricing are managed per property.

Constraints:

    The foreign key constraint on property_id ensures that bookings are only made for valid, existing properties.
    The start_date and end_date in the Booking table further define the time period for which the property is booked, preventing overlapping bookings (though this would require additional application logic).
    
5. User to Booking (Books Relationship)

        Definition: A User can make multiple Bookings, but each Booking is made by exactly one User.
   
        Cardinality: Many-to-One (N:1).
   
        Implementation: In the Booking table, the user_id attribute is a foreign key that references User(user_id). Each Booking record includes a user_id to identify the guest who made the booking.

Explanation:

    In Airbnb, users who are guests can book properties for their stays. A single user might make multiple bookings over time (e.g., booking a cabin for one trip and an apartment for another). However, each booking is associated with one guest—the person who initiated the reservation.
    For example, if Sarah (a User) books “Cozy Cabin” for June and “Beach House” for July, two Booking records will be created, both with Sarah’s user_id.

Why It Matters:

    This relationship tracks who is responsible for each booking, which is essential for payment processing, communication with the host, and managing user activity. It ensures that Airbnb knows which guest is staying at which property.
    
Constraints:
    
    The foreign key constraint on user_id ensures that only registered users can make bookings.
    The role attribute in the User table (ENUM: guest, host, admin) likely restricts booking to users with role = guest or possibly host (if hosts can book other properties).
    
7. Booking to Payment (Paid Relationship)

        Definition: Each Booking has exactly one Payment, and each Payment is associated with exactly one Booking.
   
        Cardinality: One-to-One (1:1).
   
        Implementation: In the Payment table, the booking_id attribute is a foreign key that references Booking(booking_id). Each Payment record must include a valid booking_id to link it to a specific booking.

Explanation:

    When a guest books a property, they make a payment to cover the cost (e.g., the total price for the stay). Each booking has one associated payment record, which details the amount paid, the payment method, and when it was made. Similarly, each payment is tied to one specific booking—you can’t have a payment that applies to multiple bookings or a booking with multiple payments in this model.
    For example, if John books “Cozy Cabin” and pays $300, a Payment record is created with the booking_id of that booking, an amount of $300, and a payment_method (e.g., credit_card).

Why It Matters:
    
    This relationship ensures that every booking is financially accounted for, linking payments directly to bookings. It’s crucial for tracking revenue, processing refunds, and verifying that guests have paid.
    
Constraints:

    The foreign key constraint on booking_id ensures that payments are only recorded for valid bookings.
    The payment_method (ENUM: credit_card, paypal, stripe) restricts payment options to predefined values, ensuring consistency.
    
10. Property to Review (Reviewed Relationship)

        Definition: A Property can have multiple Reviews, but each Review is associated with exactly one Property.
        Cardinality: One-to-Many (1:N).

Implementation:
        
        In the Review table, the property_id attribute is a foreign key that references Property(property_id).
        Each Review record must include a valid property_id to indicate which property is being reviewed.

Explanation in Plain English:
    
    After staying at a property, guests can leave reviews to share their experience (e.g., rating the cleanliness or comfort). A single property can receive many reviews from different guests who have stayed there. However, each review is specific to one property—you can’t review multiple properties in one review record.
    For example, if “Cozy Cabin” is reviewed by Sarah (rating 5) and John (rating 4), two Review records are created, both with the property_id of “Cozy Cabin.”

Why It Matters:

    Reviews are a key feature of Airbnb, helping future guests choose properties based on past experiences. This relationship allows properties to accumulate feedback while keeping reviews tied to the correct property.

Constraints:
    
    The foreign key constraint on property_id ensures that reviews are only written for existing properties.
    The rating attribute (INTEGER, CHECK: 1–5) ensures that reviews have valid ratings between 1 and 5.

11. User to Review (Reviews Relationship)

        Definition: A User can write multiple Reviews, but each Review is written by exactly one User.
        Cardinality: Many-to-One (N:1).

Implementation:

    In the Review table, the user_id attribute is a foreign key that references User(user_id).
    Each Review record includes a user_id to identify the guest who wrote the review.

Explanation:

    Guests who stay at properties can leave reviews, and a single guest might review multiple properties they’ve stayed at over time. However, each review is written by one specific user, ensuring that feedback is attributed to the correct person.
    For example, if Sarah stays at “Cozy Cabin” and “Beach House,” she might write two reviews, one for each property. Both Review records will have Sarah’s user_id.

Why It Matters:

    This relationship tracks which users provide feedback, allowing Airbnb to verify that reviews come from actual guests (likely those with a booking history). It also helps build user profiles with their review history.

Constraints:
    
    The foreign key constraint on user_id ensures that only registered users can write reviews. Application logic (not specified in the schema) might further restrict reviews to users who have completed a booking for the property.

13. User to Message (Sends Relationship)

        Definition: A User can send multiple Messages, but each Message is sent by exactly one User.
    
        Cardinality: Many-to-One (N:1).
    
        Implementation: In the Message table, the sender_id attribute is a foreign key that references User(user_id). Each Message record includes a sender_id to identify the user who sent the message.

Explanation:

    Airbnb allows users (hosts and guests) to communicate via messages, such as discussing booking details or property questions. A single user can send many messages to different recipients (e.g., a guest asking multiple hosts about availability). However, each message has one sender—the user who wrote and sent it.

    For example, if Jane (a host) sends a message to John (a guest) about check-in instructions, the Message record will have Jane’s user_id as the sender_id.

Why It Matters:
    
    Messaging is critical for coordination between hosts and guests, and this relationship ensures that the sender of each message is clearly identified. It supports communication features like chat threads.

Constraints:
    
    The foreign key constraint on sender_id ensures that only registered users can send messages.

15. User to Message (Receives Relationship)

        Definition: A User can receive multiple Messages, but each Message is received by exactly one User.
    
        Cardinality: Many-to-One (N:1).
    
        Implementation: In the Message table, the recipient_id attribute is a foreign key that references User(user_id). Each Message record includes a recipient_id to identify the user who received the message.

Explanation:

    Just as users can send messages, they can also receive messages from other users. A single user might receive many messages (e.g., a host getting inquiries from multiple guests). However, each message is directed to one specific recipient—the user it was sent to.
    For example, if John (a guest) receives a message from Jane (a host) about check-in instructions, the Message record will have John’s user_id as the recipient_id.

Why It Matters:
    
    This relationship ensures that messages reach the intended recipient, supporting clear and direct communication. It allows Airbnb to organize message inboxes for users.

Constraints:

    The foreign key constraint on recipient_id ensures that only registered users can receive messages.
    The dual foreign keys (sender_id and recipient_id) in the Message table create a self-referential relationship within the User entity, allowing users to communicate with each other.

Summary of Relationships:
    Here’s a concise overview of the relationships for clarity:


    User hosts Property (1:N): One User can host many Properties; each Property has one host.
    Property booked Booking (1:N): One Property can have many Bookings; each Booking is for one Property.
    User books Booking (N:1): One User can make many Bookings; each Booking is made by one User.
    Booking paid Payment (1:1): Each Booking has one Payment; each Payment is for one Booking.
    Property reviewed Review (1:N): One Property can have many Reviews; each Review is for one Property.
    User reviews Review (N:1): One User can write many Reviews; each Review is written by one User.
    User sends Message (N:1): One User can send many Messages; each Message is sent by one User.
    User receives Message (N:1): One User can receive many Messages; each Message is received by one User.
    How These Relationships Reflect Airbnb’s Functionality
    These relationships collectively model the core operations of Airbnb:

    Hosting and Booking: Users host properties, and other users book them, with bookings tracking the who, what, and when.
    Payments: Bookings are financially secured through payments, ensuring hosts are paid.
    Reviews: Guests provide feedback on properties, building trust and informing future bookings.
    Messaging: Hosts and guests communicate to coordinate stays, enhancing user experience.
    Each relationship is implemented through foreign keys, ensuring data integrity (e.g., no booking can exist without a valid property or user). The cardinality reflects real-world constraints (e.g., one property per booking, one payment per booking). Constraints like ENUM and CHECK further enforce valid data (e.g., valid booking statuses, rating ranges).



# alx-airbnb-database
Module called DataScape: Mastering Database Design

    [User]
| user_id (PK, UUID) |

| first_name (VARCHAR, NOT NULL) |

| last_name (VARCHAR, NOT NULL) |

| email (VARCHAR, UNIQUE, NOT NULL) |

| password_hash (VARCHAR, NOT NULL) |

| phone_number (VARCHAR, NULL) |

| role (ENUM: guest, host, admin, NOT NULL) |

| created_at (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP) |
    |
    | hosts (1:N)
    |
        
    [Property]
| property_id (PK, UUID) |

| host_id (FK, references User.user_id) |

| name (VARCHAR, NOT NULL) |

| description (TEXT, NOT NULL) |

| location (VARCHAR, NOT NULL) |

| pricepernight (DECIMAL, NOT NULL) |

| created_at (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP) |

| updated_at (TIMESTAMP, ON UPDATE CURRENT_TIMESTAMP) |
    |
    | booked (1:N)
    |

    [Booking]
| booking_id (PK, UUID) |

| property_id (FK, references Property.property_id) |

| user_id (FK, references User.user_id) |

| start_date (DATE, NOT NULL) |

| end_date (DATE, NOT NULL) |

| total_price (DECIMAL, NOT NULL) |

| status (ENUM: pending, confirmed, canceled, NOT NULL) |

| created_at (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP) |
    |            \
    | paid (1:1)  \ books (N:1)
    |              \
    [Payment]         [User]
| payment_id (PK, UUID) |

| booking_id (FK, references Booking.booking_id) |

| amount (DECIMAL, NOT NULL) |

| payment_date (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP) |

| payment_method (ENUM: credit_card, paypal, stripe, NOT NULL) |


    [Property]
        |
        | reviewed (1:N)
        |
    [Review]
        | review_id (PK, UUID) |

| property_id (FK, references Property.property_id) |

| user_id (FK, references User.user_id) |

| rating (INTEGER, CHECK: 1-5, NOT NULL) |

| comment (TEXT, NOT NULL) |

        | created_at (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP) |
        |
        | reviews (N:1)
        |
      [User]

    [User]
      |
      | sends (N:1)
      |
    [Message]
        | message_id (PK, UUID) |

        | sender_id (FK, references User.user_id) |

        | recipient_id (FK, references User.user_id) |

        | message_body (TEXT, NOT NULL) |

    | sent_at (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP) |
        |
        | receives (N:1)
        |
      [User]


Explanation of Each Part of the ER Diagram
1. Entities and Attributes
Each entity corresponds to a table in the database specification. Attributes are listed as provided, with primary keys underlined and foreign keys noted.

  User:
    
    Attributes: user_id (PK, UUID), first_name, last_name, email (UNIQUE), password_hash, phone_number (NULL), role (ENUM), created_at.

    Placement: The User entity is central because it interacts with most other entities (hosts properties, makes bookings, sends messages, etc.). The UNIQUE   constraint on email ensures no two users have the same email.

  Property:
    
    Attributes: property_id (PK, UUID), host_id (FK to User), name, description, location, pricepernight, created_at, updated_at.
    Placement: Connected to User via host_id, as a User (host) owns the Property. Non-null constraints ensure essential details are provided.

  Booking:

    Attributes: booking_id (PK, UUID), property_id (FK to Property), user_id (FK to User), start_date, end_date, total_price, status (ENUM), created_at.
    Placement: Links Property and User, representing a guest booking a property. The ENUM constraint limits status to valid values.

  Payment:
    
    Attributes: payment_id (PK, UUID), booking_id (FK to Booking), amount, payment_date, payment_method (ENUM).
    Placement: Tied to Booking via booking_id, as payments are made for specific bookings. The ENUM ensures valid payment methods.

  Review:
  
    Attributes: review_id (PK, UUID), property_id (FK to Property), user_id (FK to User), rating (CHECK 1–5), comment, created_at.
    Placement: Connects Property and User, as users review properties. The CHECK constraint ensures valid ratings.

  Message:
    
    Attributes: message_id (PK, UUID), sender_id (FK to User), recipient_id (FK to User), message_body, sent_at.
    Placement: Links two Users (sender and recipient), representing communication between hosts and guests.

2. Relationships and Cardinality

         Relationships are derived from foreign keys and the logical structure of the Airbnb app.

        User hosts Property (1:N): A User (host) can host multiple Properties, but each Property has one host (host_id). Represented as a line from User to Property with “hosts” label and 1:N cardinality.

       Property booked Booking (1:N): A Property can have multiple Bookings, but each Booking is for one Property (property_id). Line from Property to Booking with “booked” label and 1:N cardinality.

        User books Booking (N:1): A User (guest) can make multiple Bookings, but each Booking is made by one User (user_id). Line from User to Booking with “books” label and N:1 cardinality.

        Booking paid Payment (1:1): Each Booking has one Payment, and each Payment is tied to one Booking (booking_id). Line from Booking to Payment with “paid” label and 1:1 cardinality (assuming one payment per booking).

        Property reviewed Review (1:N): A Property can have multiple Reviews, but each Review is for one Property (property_id).
       Line from Property to Review with “reviewed” label and 1:N cardinality.

        User reviews Review (N:1): A User can write multiple Reviews, but each Review is written by one User (user_id). Line from User to Review with “reviews” label and N:1 cardinality.

        User sends Message (N:1): A User can send multiple Messages, but each Message has one sender (sender_id). Line from User to Message with “sends” label and N:1 cardinality. User receives Message (N:1):
        A User can receive multiple Messages, but each Message has one recipient (recipient_id).
        Line from User to Message with “receives” label and N:1 cardinality.
   
3. Constraints

       Constraints are incorporated as follows:

        NOT NULL: Ensures required fields (e.g., User.first_name, Property.name) are mandatory.
        UNIQUE: User.email ensures unique email addresses.
        CHECK: Review.rating is constrained to 1–5.
        ENUM: Limits values for User.role, Booking.status, and Payment.payment_method.
        Foreign Keys: Ensure referential integrity (e.g., Property.host_id must reference a valid User.user_id).

4. Indexing

        Indexes on primary keys (user_id, property_id, etc.) and additional fields (email, property_id in Booking) are not shown in the ER diagram but are noted in documentation for query optimization.

        How Each Entity and Attribute Was Placed
        User: Placed centrally due to its role in hosting, booking, reviewing, and messaging. Attributes like email (UNIQUE) and role (ENUM) reflect user identification and type (guest, host, admin).
        Property: Connected to User via the “hosts” relationship. Attributes like pricepernight and location are essential for listing details.
        Booking: Acts as a bridge between User and Property, with attributes like start_date and end_date defining the booking period.
        Payment: Tied to Booking, with payment_method (ENUM) specifying how payments are made.
        Review: Links User and Property, with rating (CHECK) ensuring valid feedback.
        Message: Connects two Users (sender and recipient), with message_body capturing communication.
