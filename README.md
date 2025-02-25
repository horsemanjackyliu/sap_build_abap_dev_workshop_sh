# Hands-On Guide: Integrating ABAP with SAP Build

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Environment Setup](#environment-setup)
4. [Hands-On Exercises](#hands-on-exercises)
5. [Key Takeaways](#key-takeaways)
6. [Troubleshooting](#troubleshooting)
7. [Appendix](#appendix)

## Introduction

Welcome to this hands-on session on integrating ABAP with SAP Build. This guide will walk you through step-by-step instructions to understand and implement the integration effectively.

**Objectives:**

- Understand the role of SAP Build in ABAP development.
- Set up the environment for integration.
- Create and extend ABAP packages via SAP Build.

## Prerequisites

**Knowledge Requirements:** Familiarity with SAP Build, ABAP, and SAP BTP.

**Access Requirements:** Ensure access to the following:

- SAP BTP Account with necessary entitlements.
- SAP Build subscription.
- ABAP system (BTP or S/4HANA Public Cloud).

**Tools:**

- [Eclipse](https://www.eclipse.org/downloads/packages/release/2024-12/m3) with ADT installed ([Download ADT](https://tools.hana.ondemand.com/latest)). Additional help guide [here](./ADTSetup.md).
- Updated browser supporting SAP Build UI.

## Environment Setup



<!-- Note: Can be skipped

**Step 1: Subscribing to SAP Build**

1. Log into your SAP BTP Cockpit.
2. Navigate to Subscriptions and select SAP Build.
3. Follow the steps to activate the subscription.

**Step 2: Setting Up Destinations**

1. Navigate to Connectivity > Destinations in SAP BTP.
2. Create a destination with the following properties:

- Name: ABAP_System
- Type: HTTP
- URL: <Your ABAP System URL>
- Authentication: SAMLAssertion

3. Test the connection to ensure it's active.

**Step 3: Configuring Communication Systems**

1. Download your BTP subaccount's trust certificate.
2. Configure the communication system in the ABAP environment using the Communication Systems app. -->

**SAP Build system with access to Build Code, Build Apps and Build Process Automation is already available:** [Link](https://spa-us10-nwjsondh.us10.build.cloud.sap)

## Hands-On Exercises

**Note:** The system is for testing purposes only. After the session, all data will be lost, so keep that in mind when creating new TRs or objects.


### Exercise 0: Create Package Tabel and data generater class in Eclipse ADT

1. **Create Package**
   In ADT, go to the Project Explorer, right-click on the package ZLOCAL, and select New > ABAP Package from the context menu.
   ![alt text](image.png)
   
   Maintain the required information (### is your group ID):

   ![alt text](image-1.png)

   Name: zshbuild_###
   Description: SH build day Package ###
   Select the box Add to favorites package
   Click Next >.

   Create a new transport request, maintained a description (e.g.SH build Package ###), and click Finish.
![alt text](image-2.png)



2. **Create database table**
   - Right-click on your ABAP package zshbuild_### and select New > Other ABAP Repository Object from the context menu.
    ![alt text](image-3.png)

   - Search for database table, select it, and click Next >.
  ![alt text](image-4.png)

   - Maintain the required information (### is your group ID) and click Next >.
![alt text](image-5.png)
   - Select a transport request, and click **Finish** to create the database table.
   - Replace the default code with the code snippet provided below and replace all occurrences of the placeholder ### with your group ID using the Replace All function (CTRL+F).


      ```
         @EndUserText.label : 'Database table ###'
         @AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
         @AbapCatalog.tableCategory : #TRANSPARENT
         @AbapCatalog.deliveryClass : #A
         @AbapCatalog.dataMaintenance : #RESTRICTED
         define table zsh_travel### {

         key client    : abap.clnt not null;
         key travel_id : /dmo/travel_id not null;
         agency_id     : /dmo/agency_id;
         customer_id   : /dmo/customer_id;
         begin_date    : /dmo/begin_date;
         end_date      : /dmo/end_date;
         createdby     : syuname;
         createdat     : abp_creation_tstmpl;
         lastchangedby : abp_locinst_lastchange_user;
         lastchangedat : abp_lastchange_tstmpl;

         }

      ```
   - Save and activate the changes.
3. **Create data generator class**
   Create an ABAP classclass to generate demo zsh_travel### data.
      - Right-click your ABAP package zshbuild_### and select New > ABAP Class from the context menu.
         ![alt text](image-6.png)

      - Maintain the required information (### is your group ID) and click Next >.

         Name: ZCL_SHBUILD_GEN_DATA_###
         Description: Generate demo data
         ![alt text](image-7.png)


      - Select a transport request and click Finish to create the class.
         ![alt text](image-8.png)
      - Replace the default code with the code snippet provided in the source code document ZCL_shbuild_GEN_DATA_### linked below and replace all occurrences of the placeholder ### with your group ID using the Replace All function (CTRL+F).  
         ```

            CLASS zcl_shbuild_gen_data_### DEFINITION
            PUBLIC
            FINAL
            CREATE PUBLIC .

            PUBLIC SECTION.
               INTERFACES if_oo_adt_classrun.
            PROTECTED SECTION.
            PRIVATE SECTION.
            ENDCLASS.



            CLASS zcl_shbuild_gen_data_### IMPLEMENTATION.
            METHOD if_oo_adt_classrun~main.
               DATA:
               group_id   TYPE string VALUE '###'.
            DELETE FROM ZSH_TRAVEL### .
            INSERT ZSH_TRAVEL### FROM (
            SELECT
               FROM /dmo/travel AS travel
               FIELDS
                  travel~travel_id        AS  travel_id,
                  travel~agency_id       AS agency_id,
                  travel~customer_id     AS customer_id,
                  travel~begin_date      as begin_date,
                  travel~begin_date as end_date,
                  travel~createdby        AS createdby,
                  travel~createdat        AS createdat,
                  travel~lastchangedby    AS lastchangedby,
                  travel~lastchangedat    AS lastchangedat
                  ORDER BY travel_id UP TO 10 ROWS
                  ).
               COMMIT WORK.
               out->write( |[SH build] Demo data generated for table ZSH_TRAVVEL{ group_id }. | ).
            ENDMETHOD.
            ENDCLASS.

         ```  
      - Save and activate the changes.
      - Run your console application. For that, select your ABAP class class zcl_shbuild_gen_data_###, select the run button > Run As > ABAP Application (Console) F9 or press F9. A message will be displayed ABAP Console.
         ![alt text](image-9.png)
         ![alt text](image-10.png)
   


### Exercise 1: Creating an ABAP Project in SAP Build

1. **Access the SAP Build Lobby:**

   Log in to the [SAP Build Lobby](https://bdb6dc3dtrial.us10.build.cloud.sap/lobby) and click `Create` > `Build an Application` > `ABAP Cloud`.

   ![image](./assets/project-1.png)

   ![image](./assets/project-2.png)

   ![image](./assets/project-3.png)

2. **Select System:**

   From the System dropdown, select **TRL**, which is the mapped system for the session. This system has been pre-configured for the session.

3. **Package Selection:**

   You can use the existing packages **ZSHBUILD_###** … 

   ![alt text](image-11.png)



4. **Proceed to ABAP Project Creation:**
   ![alt text](image-12.png)

5. **Post successful creation open the project in Eclipse.**

   i. Allow browser to open eclipse.

   ![image](./assets/project-8.png)

   ii. Create a new project from the link.

   ![image](./assets/project-9.png)

   iii. Click on Next.

   ![image](./assets/project-10.png)

   iv. Logon using test user through browser and close the new tab once done.

   ![image](./assets/project-11.png)

   ![image](./assets/project-12.png)

   v. Finish with the creation

   ![image](./assets/project-13.png)

### Exercise 2: Generating and Using Service Bindings

1. **Create a New ABAP Object:** Use the Repository Object Generator Wizard to create a new OData UI service for your ABAP object.

   i. Browse existing objects.

   ![image](./assets/fiori-1.png)

   ii. Search for “zsh” and then chose the object accordingly as per your user, example for  select Zsh_TRAVEL### and press OK.

    ![alt text](image-13.png)

   iii. Press Next.

   ![alt text](image-14.png)

   iv. Once the packages are validated press Next on the screen.

   v. Validate the general details and press Next again.

   vi. Review the list of repository objects that are going to be generated and press Next again.

   vii. Select a transport request from the list or browse for existing requests from under the “Enter a request number” and select the “CEI Workshop" request. You can also create a new request.

   viii. Click Finish and wait for artifacts to generate.

   ix. Publish the artifacts.

   ![alt text](image-15.png)

   x. Preview the Fiori Application.

   ![alt text](image-16.png)

2. **Create a Fiori Project:** Link Business Application Studio to your project

   i. Create a Fiori dev space for yourself [here](https://bdb6dc3dtrial.us10cf.trial.applicationstudio.cloud.sap/index.html?externalRedirect=true).

   ![image](./assets/fiori-4.png)

   ii. You would need the dev space id of the newly created dev space to complete the next steps.

   ![image](./assets/fiori-5.png)

   iii. Go back to eclipse and open project properties.

   ![image](./assets/fiori-6.png)

   iv. Use the below details and link the dev space under “ABAP Development Tools” -> “External IDE Configuration” -> “Configure External IDE” -> “SAP Business Application Studio”

   - URL: <https://bdb6dc3dtrial.us10cf.trial.applicationstudio.cloud.sap/index.html>
   - Dev Space Id: `<dev space id from previous step>`
   - Destination Name:'TRl`

   v. Click on Apply and Close.

   vi. Select the newly published entity and select Create Fiori Project.

   ![image](./assets/fiori-7b.png)

   vii. Select the “List Report Page” as the template.

   ![image](./assets/fiori-8.png)

   viii. Review the Entity Selection step and press Next.

   ix. Review the project attributes and press Finish. This will generate a Fiori Application. Please wait for the files to be load.

   x. Click on Preview application and select start-mock. This will setup the dependencies and start a mock application.

   ![image](./assets/fiori-9.png)

   ![image](./assets/fiori-10.png)

   xi. In the preview app press Go to view mock data.

   ![image](./assets/fiori-11.png)

## Key Takeaways

- SAP Build simplifies ABAP extensibility.
- Integration allows seamless development across SAP BTP and S/4HANA.
- Service bindings can be reused across multiple SAP Build tools, enhancing collaboration and flexibility.

## Troubleshooting

### Common Issues

| Issue                        | Possible Solution                                         |
| ---------------------------- | --------------------------------------------------------- |
| Destination not reachable    | Verify URL and authentication settings.                   |
| Transport request not listed | Check ABAP system connection and permissions.             |
| Service binding not found    | Ensure the package is created in the correct ABAP system. |

## Appendix

### References

- SAP Build Documentation
- SAP BTP ABAP Environment Guide

### Glossary

- BTP: Business Technology Platform.
- ADT: ABAP Development Tools.

### Contact Information

For assistance during the session, reach out to:
Swarnava Chatterjee (<swarnava.chatterjee@sap.com>) (Product Manager, SAP Build)
