# Book Rental CRM Project

This project is a **Book Rental CRM** application developed using Salesforce to streamline and automate the book rental process. The application includes features for managing customers, billing, and automated reminders for overdue books, providing an efficient solution for book rental businesses.

## Objectives

The project aims to achieve the following business goals:
- Automate the book rental process to minimize manual work.
- Track and manage customer rental history and billing information.
- Provide reminders for overdue books to ensure timely returns.
- Generate reports on book rentals and customer data for better decision-making.

### Specific Outcomes
- Automated billing and late fee calculations based on rental and return dates.
- Easy management of customer and book data.
- Implementation of reminders for overdue books.
- Efficient tracking of payment statuses and rental fees.

## Salesforce Key Features and Concepts Utilized

- **Objects and Relationships**: Custom objects for Customers, Books, and Billing Process.
- **Automated Processes**: Workflow rules and Apex triggers for overdue book reminders.
- **Formula Fields**: Used to calculate total amounts, rental fees, and late fees.
- **Validation Rules**: Ensuring data accuracy, such as mandatory fields for customer information.
- **Security**: Role-based access control for different users in the system.

## Apex Code for Schedulable Class Overdue Reminder
```java
public class OverdueReminder implements Schedulable {
    // Method called by the scheduler
    public void execute(SchedulableContext sc) {
        sendOverdueReminders();
    }

    // Method to send overdue reminders
    public static void sendOverdueReminders() {
        List<Book_Rental__c> overdueRentals = [
            SELECT Customer__r.Email__c, Customer__r.Name, Book__r.Name, Due_Date__c
            FROM Book_Rental__c
            WHERE Due_Date__c < :Date.today() 
            AND Return_Date__c = NULL
        ];

        for (Book_Rental__c rental : overdueRentals) {
            // Build the detailed email message
            String customerName = rental.Customer__r.Name;
            String bookName = rental.Book__r.Name;
            Date dueDate = rental.Due_Date__c;
            
            String messageBody = 'Dear ' + customerName + ',\n\n' +
                                 'This is a reminder that the following book you rented is overdue:\n\n' +
                                 'Book Title: ' + bookName + '\n' +
                                 'Due Date: ' + String.valueOf(dueDate) + '\n\n' +
                                 'Please return the book as soon as possible to avoid further additional charges.\n\n' +
                                 'Best regards,\nYour Book Rental CRM';

            // Create the email message
            Messaging.SingleEmailMessage mail = new Messaging.SingleEmailMessage();
            mail.setToAddresses(new String[] { rental.Customer__r.Email__c });
            mail.setSubject('Overdue Book Reminder for ' + bookName);
            mail.setPlainTextBody(messageBody);
            Messaging.sendEmail(new Messaging.SingleEmailMessage[] { mail });
        }
    }
}
```
## Apex Trigger for sending welcome mail for new registrations
```java
trigger SendWelcomeEmailBook on Customer__c (after insert) {
  for (Customer__c customer : Trigger.new) {
        Messaging.SingleEmailMessage mail = new Messaging.SingleEmailMessage();
        mail.setToAddresses(new String[] { customer.Email__c });
        mail.setSubject('Welcome to Our Book Rental Service');
        mail.setPlainTextBody('Dear ' + customer.Name + ',\n\nThank you for joining our book rental service!');
        Messaging.sendEmail(new Messaging.SingleEmailMessage[] { mail });
    }
}
```


## Testing and Validation

The project has been thoroughly tested through:
- **Unit Testing**: Apex classes and triggers have been unit tested to ensure functionality.
- **User Interface Testing**: Manual and automated tests have been conducted on the UI to ensure smooth customer interactions.

## Key Scenarios Addressed by Salesforce

- Efficient handling of customer data through Lookup fields and custom objects.
- Automated billing calculation based on rental durations and late fees.
- Notifications sent automatically to customers when a book is overdue.
- Streamlined process for renting and returning books, reducing administrative effort.

## Documentation

For a detailed description of the project, including objectives, key features, and testing, refer to the project documentation:
📝 [Project Documentation](https://docs.google.com/document/d/1sHh2-G45r3pR_DyL6oZAXfwJl6cvM_WvzE6aE9HIu9k/edit?usp=sharing)

## Video Link
For a detailed demonstration video click here: 🎥 [Youtube Link](https://www.youtube.com/watch?v=bxU8NOcw5Tc)

---

Feel free to explore the project and contribute if you'd like to enhance its features or improve the functionality!
