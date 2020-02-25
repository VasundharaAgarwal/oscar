# oscar

## What is Oscar?

More than 15 million GP appointments are missed annually, costing the NHS hundreds of millions of pounds.

We have attempted to reduce this wastage with Oscar, a natural language email assistant to confirm attendence to GP appointments. We aim for Oscar to learn over time how to handle conversations more effectively and become personalised based on patient's input. 

## How does Oscar Work?

Oscar stores all appointment data and emails in two remote databases, one managed by us and one managed by Gmail. Every 30 seconds Oscar's `EmailReceiver` polls the Gmail Server to see if there are any new emails. 

If there are new emails, `EmailSender` parses the email and passes it Oscar's `Kernel`. `Kernel` then checks with our remote database whether this is an appointment we recognise. If it is, the email is sent to Oscar's `EmailClassifier`.

The `EmailClassifier` uses the [OpenNLP](https://opennlp.apache.org/) Max Entropy classifer to understand whether the patient is confirming attendance, cancelling, rescheduling, or whether it is another type of response Oscar is not designed to handle.

Oscar's `Kernel` then uses this information to decide what type of repsonse to send to the patient. Appropriate modifications are made to the database at this time to reflect the confirmation, cancellation, or rescheduling of the appoinment.

The `Kernel` then asks Oscar's `EmailSender` to send an email, passing the relevant information to be included.

## How do you use Oscar?

Oscar is written in Java 1.8, and can be compilied and ran on compatible versions of the JVM. The main class is `Kernel.java`. 

When first starting, a command line prompt will ask for two usernames and passwords. The first username and password pair is for logging into [SRCF](https://srcf.net) where our database is hosted. The second username-password pair is the login details for the database.