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

**SAP Build system with access to Build Code, Build Apps and Build Process Automation is already available:** [Link](https://build-day-gc-wk1s5i5m.us10.build.cloud.sap/lobby)

## Hands-On Exercises

**Note:** The system is for testing purposes only. After the session, all data will be lost, so keep that in mind when creating new TRs or objects.




### Exercise 0: Create ABAP Project,Package, Table and data generater class in Eclipse ADT

1. **Create ABAP Cloud Project in Eclipse ADT**
   

   ![alt text](image-17.png)
   ![alt text](image-18.png)

   Fill the field **ABAP instance URL** with the value : https://051ed91d-aa61-4ac5-a825-fbbbc8f45121.abap.us10.hana.ondemand.com. And click on **Next** .

   ![alt text](image-19.png)
   ![alt text](image-20.png)
   ![alt text](image-23.png)

   Input you username with **builduser###@sap.com** and password **W3lcome!###** .  ### is your account ID. For example, if your user account number is 181, then your username is **builduser181@sap.com** and your password is **W3lcome!181** . If you not sure, please check [UserAccounts](users.md) .

   ![alt text](image-24.png)

   ![alt text](image-21.png)


   ![alt text](image-22.png)

   



2. **Create Package**
   

   In ADT, go to the Project Explorer, right-click on the package ZLOCAL, and select New > ABAP Package from the context menu.
   ![alt text](image.png)
   
   Maintain the required information (### is your account ID):

   ![alt text](image-1.png)

   Name: zshbuild_###
   Description: SH build day Package ###
   Select the box Add to favorites package
   Click Next >.

   Create a new transport request, maintained a description (e.g.SH build Package ###), and click Finish.
![alt text](image-2.png)



3. **Create database table**
   
   - Right-click on your ABAP package zshbuild_### and select New > Other ABAP Repository Object from the context menu.
    ![alt text](image-3.png)

   - Search for database table, select it, and click Next >.
  ![alt text](image-4.png)

   - Maintain the required information (### is your account ID) and click Next >.
  
      Name: ZSH_TRAVEL###

      Description : DataBase Tabel ###

![alt text](image-5.png)
   - Select a transport request, and click **Finish** to create the database table.
   - Replace the default code with the code snippet provided below and replace all occurrences of the placeholder ### with your account ID using the Replace All function (CTRL+F).


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
  

1. **Create data generator class**

   Create an ABAP classclass to generate demo zsh_travel### data.
      - Right-click your ABAP package zshbuild_### and select New > ABAP Class from the context menu.
         ![alt text](image-6.png)

      - Maintain the required information (### is your account ID) and click Next >.

         Name: ZCL_SHBUILD_GEN_DATA_###
         Description: Generate demo data
         ![alt text](image-7.png)


      - Select a transport request and click Finish to create the class.
         ![alt text](image-8.png)
      - Replace the default code with the code snippet provided in the source code document ZCL_shbuild_GEN_DATA_### linked below and replace all occurrences of the placeholder ### with your account ID using the Replace All function (CTRL+F).  
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

   Log in to the [SAP Build Lobby](https://build-day-gc-wk1s5i5m.us10.build.cloud.sap/).

   In the next screen, click **aobocnbd7.accounts.ondemand.com**, then fill you username **builduser###@sap.com** and password **W3lcome!###**. Please replace **###** with **your user ID** . 

   Then click `Create` > `Build an Application` > `ABAP Cloud`.


   ![image](./assets/project-1.png)

   ![image](./assets/project-2.png)

   ![image](./assets/project-3.png)

2. **Select System:**

   From the System dropdown, select **H01**, which is the mapped system for the session. This system has been pre-configured for the session.

3. **Package Selection:**

   You can use the existing packages **ZSHBUILD_###** Which is created by yourself in **Exercise 0** .
   
   Please replace **###** with **your user ID** . 

   ![alt text](image-11.png)



4. **Proceed to ABAP Project Creation:**
   
   Project Name: ZSHBUILD_###

   Description: ZSHBUILD_###

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

   1. Browse existing objects.

   ![image](./assets/fiori-1.png)

   2. Search for “zsh” and then chose the object accordingly as per your user, example for  select ZSH_TRAVEL### and press OK.
   
   **NOTE: only select the table of your user ID which is created by yourself .**

    ![alt text](image-13.png)

   3. Press Next.

   ![alt text](image-14.png)

   4. Once the packages are validated press Next on the screen.

   5. Validate the general details and press Next again.

   6. Review the list of repository objects that are going to be generated and press Next again.

   7. Select a transport request from the list or browse for existing requests with description **SH build Package ###** which has been created yourown user account in **Excercise 0**. 

   Please replace **###** with **your user ID** . 

   8. Click Finish and wait for artifacts to generate.

   9. Publish the artifacts.

   ![alt text](image-15.png)

   10. Preview the Fiori Application.

   ![alt text](image-16.png)

2. **Create a Fiori Project:** Link Business Application Studio to your project

  <!-- 1. Create a Fiori dev space for yourself [here](https://build-day-gc-wk1s5i5m.us10cf.applicationstudio.cloud.sap/index.html?externalRedirect=truee).

   ![image](./assets/fiori-4.png)

   2. You would need the dev space id of the newly created dev space to complete the next steps.

   ![image](./assets/fiori-5.png) -->

   1. Go back to eclipse and open project properties.

   ![image](./assets/fiori-6.png)

   2. Use the below details and link the dev space under “ABAP Development Tools” -> “External IDE Configuration” -> “Configure External IDE” -> “SAP Business Application Studio”

   - URL: <https://build-day-gc-wk1s5i5m.us10cf.applicationstudio.cloud.sap/index.html>
   - Dev Space Id: `ws-jb8lx`  or `ws-l62dz`

      Fr builduser180@sap.com to builduser190@sap.com, use `ws-jb8lx`

      Fr builduser191@sap.com to builduser200@sap.com, use `ws-l62dz`


   - Destination Name:'H01`
      ![alt text](image-25.png)

   1. Click on Apply and Close.

   2. Select the newly published entity and select Create Fiori Project.

   ![alt text](image-26.png)

   3. Select the “List Report Page” as the template.

   ![image](./assets/fiori-8.png)

   4. Review the Entity Selection step and press Next.

      ![alt text](image-27.png)

   5. Review the project attributes and press Finish. This will generate a Fiori Application. Please wait for the files to be load.

      Module name: zshbuild_ui5_###

      Application title:  UI5 PROJECT ###

      **Note** Please replace **###** with **your user ID** . 
      ![alt text](image-28.png)




   10. Click on Preview application and select start-mock. This will setup the dependencies and start a mock application.

      ![alt text](image-29.png)

      ![alt text](image-30.png)

   11.  In the preview app press Go to view mock data.
      ![alt text](image-31.png)
 

   12. Stop the preview in SAP Business Application Studio.
   ![alt text](image-32.png)  

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
