# 🚀 Team-24DIGIBP1
This project was completed during the Spring semester of 2024 as part of the course "Digitalization of Business Processes." The objective of this project was to analyze and model an existing process, identify problems and potential for digitalization, and digitalize the process where it adds value.

## Table of contents
- [👩‍💻 Team Members](#team-members)
- [📚 Background](#background)
- [📝 Short Description of Process](#short-description-of-process)
- [🔍 As-is Process](#as-is-process)
    - [🚩Pain Points / Digitalization Potential](#pain-points--digitalization-potential)
- [🔄 To-be Process](#to-be-process)
    - [🤔 Assumptions](#assumptions)
    - [📄 Description To-Be Process](#description-to-be-process)
  - [🛠️ Technical Implementation](#technical-implementation)
    - [Section 1](#section-1)
    - [Section 2](#section-2)
    - [Section 3](#section-3)
- [💻 Technologies Used](#technologies-used)
- [▶️ Running the Process](#running-the-process)
- [🙏 Acknowledgements](#acknowledgements)




### 👩‍💻 Team Members 
| Team Members | E-Mail |
|--------------|--------|
|Claudia D'Aniello| claudia.daniello@students.fhnw.ch |
|Samira Nathalie Moser | samiranathalie.moser@students.fhnw.ch|
| Nadia Jakob | nadia.jakob@students.fhnw.ch|

### 📚 Background
The FeuerWächter AG is a SME with 177 employees and is part of the international acting FlameShield Co. FeuerWächter AG is in the field of fire protection and offers many products and services from small extinguishing devices to planning and installation of pressurised smoke protection systems. Their customers include private individuals, companies, architects and construction managers. Since 2018 the FeuerWächter AG is part of the FlameShield Co, who operates in the construction sector. With the takeover, FeuerWächter AG had to adapt to the IT systems of FlameShield Co. The system used by FlameShield Co does not offer the same functionality as the previous ERP system so they were able to only move the financial accounting and controlling parts to the new ERP. The whole management of the construction project stayed in the old system. The invoice data is transferred to SAP via an interface, where it is then processed. The focus of the ERP change was to implement the processes in SAP. Now that these processes are working, the focus is on improving and digitalizing the processes.

### 📝 Short description of Process 
This project focuses on the dunning process, which will be executed every two weeks on Tuesday. The process is mainly located in SAP. Other tools used in the process are an advanced PDF tool, Excel, and Outlook. The modelling is done based on the Dunning run instructions document and further feedback from the finance department. A dunning run includes about 300 reminders, the majority of which are emailed. Around 50 dunning blocks will be set per dunning run. Setting a dunning block does not trigger a dunning notice during the dunning block. A valid reason is required to set a dunning block. The finance department will decide whether the reason is valid or not based on the description.

# 🔎 As-is Process
The BPMN models for all processes mentioned in this readme can be found in the "Models" 

![Overview As-is Process](./05_Images/As_is_Process_Overview.jpg)

We have divided the AS-IS process into three sections, in the screenshot above, the entire process can be seen with the sections highlighted. The explanation of the process follows these three sections.

![Section 1 As-is Process](05_Images\As_is_Section_1.jpg)

**Start Event: Every second Tuesday**

The process starts every 2nd Tuesday, as the dunning run is to take place every two weeks.  

**User Task: Open dunning list**

For this task, the Finance Department opens the dunning list from SAP with all overdue receivables on it that require a follow-up. The list shows the customer concerned, the project concerned, the outstanding amount and other information required to process the outstanding payment.

**User Task: Payment reconciliation**

During payment reconciliation, the Finance Department carries out various checks to see whether the amount is actually still outstanding. It checks any advance payments or double payments, as well as recently received payments, and also compares the items with the bank accounts. This check is carried out on a regular basis, every 2nd or 3rd day, but must also be carried out directly before the dunning run to ensure that no unjustified reminders are issued.

**Exclusive Gateway: Third reminder?**

This gateway checks if this is the third reminder for payment.  
Yes: The process moves to "Transfer case to Creditreform”. This is done from a User directly via Email. Once this has been done, the item is removed from the dunning list as it no longer needs to be taken into account because Creditreform takes care of it.  
No: The process goes on with checking if the dunning block is applicable.

**Exclusive Gateway: Dunning block applicable?**

The gateway decides whether a Dunning block is applicable or not. This is done, for example, in cases such as an error by Feuerwächter AG or a delay in delivery. In these cases, a dunning would be unjustified, as no delivery has yet been made. If a dunning block may be set, this is done directly by the Finance Department (User Task: Set dunning block) and then this item is also removed from the dunning list (Task: Remove instance from dunning list).
If the Dunning block may not be set, the process is continued with the following task.  

![Section 2 As-is Process](05_Images\As_is_Section_2.png)

**User Task: Export dunning list**

The current Dunning list is exported from SAP as an Excel file.  

**User Task: Send e-mail to Department contacts**

Once the list is available as an Excel file, it is sent by email to the department heads so that they can check the list and add any comments, such as errors by Feuerwächter AG or delays in delivery. This is done via Excel file because the department heads don’t have access to the dunning list on SAP. After all, the department heads are the ones who know best about the current status of their project.

**Timer Event: Two days**

The department heads then have two days to process the Excel reminder list received. If no feedback has been received after two days, the Finance Department assumes that the items as they appear on the list sent are correct and no adjustments are requested by the department head.

**Catch Event: Updated e-mail list received**

If the department heads wish to make adjustments, they must return the corrected and updated email list to the Finance Department before the two-day deadline expires.

**Exclusive Gateway: Dunning block applicable?**

After receiving the list, the Finance Department checks the Dunning blocks again. This time, those requested by the department heads and noted on the Excel list are checked. The Finance Department checks whether these are justified or not. If they are justified, the Dunning block is set directly in SAP (User Task: Set dunning block) and the dunning item is removed from the list (Task: Remove instance from dunning list). If the dunning block is not justified, the item is still taken into account in the process.

**Exclusive Gateway: Dunning address correct?**

This gateway checks if the dunning address mentioned in the dunning list is correct. It may be that a customer does not want invoices and reminders to be sent to the usual main address, but to a branch office or the relevant department with a different address.   
Yes: The process continues to "Execute dunning run."  
No: The process moves to "Correct dunning address." In this step, the Dunning address is corrected manually by a user so that the reminder is sent to the address requested by the customer.

![Section 3 As-is Process](05_Images\As_is_Section_3.jpg)

**Execute dunning run**

This task executes the dunning run, preparing the notices for dispatch. This is triggered by a click from a user in SAP.

**Exclusive Gateway: Dunning notice per Mail?**

In this step, the system checks whether an email address is stored for the customer. If this is the case, the reminder is sent by email to the corresponding email address (Service Task: Send Email). If no email address is stored, the reminder is sent by post, for which the following steps are necessary.

**User Task: Download dunning notice**

This task involves downloading the prepared dunning notice for mailing purposes. This is done by a User manually via PDF out of SAP.

**User Task: Separate dunning to send per post notices into individual PDF’s**

This task is performed by an employee of the Finance Department. The employee must sort the PDF, which is generated as a complete document with all reminders in it, so that all reminders that have already been sent by email are no longer included. He can then print out the reminders that still need to be sent by post.

**User Task: Prepare letter**

This task involves preparing the physical letter to be sent out as part of the dunning process.

**User Task: Send letter**

In this step, the prepared letters are taken to the post office and sent to the recipients accordingly.  

**End Event**

The process ends after the letter is sent or the appropriate steps have been completed based on the conditions checked in the gateways.

### 🚩Pain Points / Digitalization Potential
The current process has some points that need improvement.

**Duration of the process**

Specifically, it takes a long time to complete as the initial checks and the export of the dunning list Excel are executed on the first day. This is followed by a two-day waiting period during which the department heads can check their dunning positions and set dunning blocks. The active part of the process starts again after the two-day waiting period. This waiting period unnecessarily prolongs the process and thus needs to be addressed.

**Setting dunning blocks**

The finance department is not directly involved in the projects and therefore lacks awareness about their progress. To address this, the finance department exports a dunning list that contains all the due invoices from the upcoming dunning run. They share this list with the department heads, who then check whether there is a valid reason that the invoices are unpaid. Valid reasons could be any mistakes from the company’s side like delay in delivery of the products or delivering the wrong products. After checking their position on the dunning list for two days, they send the comments back to the finance department. The finance department manually reviews all the comments and validates whether they warrant setting a dunning block. If necessary, they enter the dunning block manually in SAP. This process is prone to errors as everything is done manually, and it involves redundant work as the comments need to be copied into the system.

**Correction of the dunning address**

The company distinguishes between two different billing addresses the “Konto” (Account) and the “Rechnungsempfänger” (invoice recipient). Every customer has one account but can have several different invoice recipients. One Example of that would be a supermarket with several different branches. The company headquarters would be stored as the account and the individual shops as invoice recipients. The invoice goes to the appropriate invoice recipients. However, the dunning notice are sent directly to the account according to the internal business guidelines. In some cases, e.g. smaller companies, the account and the invoice recipient are the same. But if this isn’t the case the financial department has to manually correct the dunning address. This is done in the dunning run and they just copy the account number and replace the invoice recipient with the account number. This is done manually by a user who has to go through every position of the dunning list. This makes it prune to errors and could be automized. 

**Export of the reminders send by post**

When the dunning run is executed, an email with a dunning notice as a pdf in the attachment is automatically sent to customers who have opted to receive dunning notice via email. However, some customers still prefer to receive dunning notice via post. To send these dunning notice via post, the finance department needs to extract a PDF containing all the dunning notice from SAP, save it onto their computer, and manually extract the corresponding PDF from the large PDF file to print it.

# 🔄 To-Be Process
This section addresses the to-be process.

## 🤔 Assumptions
We assumed that a new role in SAP could be created for our to-be process. This role would have limited access. It would have access to open the dunning list and add comments to the corresponding dunning position. However, the role can not make any other changes to the dunning positions or execute a dunning run. The creation of this role would make the process leaner. 

## 📄 Description To-Be Process

## 💡 Benefits
-	Less manual work 
-	More efficiency due to faster processing
-	Fewer errors due to automation
-	Bundling the finance department process into one day instead of two


## 🛠️ Technical Implementation
In this section, we elaborate the technical implementation. For this purpose, we have divided the process into three sections to make it easier to follow. Within these sections, we have highlighted and numbered parts, which we will elaborate in the following chapters in detail.

![Technical Implementation Overview](05_Images\To_be_process_overview.jpg)

### Section 1
In the first section we will discuss the following implementations:
1.	Reminder Dunning Block
2.	Decision Dunning Block

**Reminder Dunning Block**

We have changed the beginning of the process slightly, by moving the start of the entire process from Tuesday to Thursday. Every other week, an e-mail with the reminder to set the dunning block on the overdue invoices is automatically sent to the heads of department (HoD). With the new user roles as discussed in our assumptions above, they are asked to add dunning block were deemed necessary by adding comments directly on SAP (in our case the Excel file).

![Reminder Dunning Block](05_Images\Reminder_set_dunning_block.jpg)
Figure 1 shows the Camunda diagram, we start the process with a timer start event (Figure 1, A), which triggers on Thursday morning every other week (Figure 2 shows the settings). The task “Send reminder e-mail” then triggers a make scenario (Figure 3), which sends the reminder e-mail to the HoD.

![To-be Section 1 Figure 2 & 3](05_Images\TI_S1_F2.jpg)
The make scenario is simple, it consists of a webhook (Figure 3, A), which is triggered by the system task. To send the e-mail, we use the Gmail module (Figure 3, B) which is connected to our project e-mail address which then sends out the predefined e-mail to the HoD (Figure 4). The e-mail addresses are static, as we assume that the HoD do not change very frequently, and thus changing them manually if they do, seems a reasonable approach. The e-mail body is written in HTML, to ensure proper formatting of the e-mail. At the end, we have a webhook response module (Figure 3, C), which sends the variable “Reminder_status=successful” back to Camunda (Figure 5). This variable is then needed in Camunda to confirm successful completion of the reminder task. Figure 6 shows the e-mail received by the recipient defined in make.

![To-be Section 1 Figure 4 & 5](05_Images\TI_S1_F4_F5.jpg)
![To-be Section 1 Figure 6](05_Images\TI_S1_F6.jpg)
In Camunda, we added an expression on the flow (Figure 1, B) in between the first timer event and the system task, as “Reminder_status=unsuccessful”. This expression is needed for the exclusive gateway to confirm the reminder was sent successfully by the make scenario. To achieve this, we put an execution listener on the “unsuccessful” arrow (Figure 1, C), looking for the variable “Reminder_status=unsuccessful”. However, if the reminder was sent successfully, this variable will change to “successful” and as a result follow the default path. If there was an issue with the make scenario, an employee will have to check manually what the error might have been, as a user task (Figure 1, D). Finally, the process arrives at the second timer event(Figure 1, E), which will continue the process the following Tuesday.

**Decision Table**

The process for deciding which reminder code to set starts with the service task "Count rows in Spreadsheet." This is needed as preparation for the following subprocess. 

![To-be Section 1 Decision Table](05_Images\TI_S1_DT.jpg)

The Make scenario below is initiated by the mentioned service task. The service task triggers the webhook (1), which then consults the dunning run list in SAP (in our case, a Google Spreadsheet) (2).

![To-be Section 1 Decision Table F1-3](05_Images\TI_S1_DT_123.jpg)

### Section 2
### Section 3

## 💻 Technologies used

## ▶️ Running the Process

Follow these instructions to run the process:
- ..
- ...



## 🙏 Acknowledgements
We would like to thank our coaches, Andreas Martin and Charuta Pande, for their invaluable guidance and unwavering support throughout this project. 





