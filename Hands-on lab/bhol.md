# Before the hands-on lab

## Estimated Duration: 150 minutes

## Overview

In this lab, you'll set up a comprehensive data integration workflow in Azure Synapse Analytics, enabling seamless data transfer between Azure Data Lake Storage Gen2 and Azure Cosmos DB for NoSQL. The process begins with the creation of linked services that establish secure connections to both Azure Data Lake Storage Gen2, where the source CSV file resides, and Azure Cosmos DB, which will store the migrated data. After the connections are set up, you'll define integration datasets, representing the structure of the source CSV data and the destination Cosmos DB collection. These datasets will serve as the foundation for building a data pipeline that facilitates the transfer of records from the CSV file into Cosmos DB. Once the pipeline is fully configured, you'll publish and trigger it to execute the migration. Following the pipeline execution, you'll use the Azure Cosmos DB Data Explorer to verify the success of the data transfer, ensuring the records have been accurately moved and are available in the target database for further querying and analysis. This lab provides a hands-on experience in setting up an end-to-end data integration workflow in Azure Synapse Analytics.

## Lab Objectives

After completing this lab, you will be able to:

- Task 1: Obtain the desired Azure Subscription ID value
- Task 2: Create an SAP Cloud Appliance
- Task 3: Deploy the Azure Resources
- Task 4: Prepare sales data in SAP
- Task 5: Prepare the business partner service in SAP
- Task 6: Prepare payment data in Cosmos DB

### Task 1: Obtain the desired Azure Subscription ID value

In this task, you will obtain the Azure Subscription ID value needed to configure and deploy resources in your Azure environment.

1. In the top toolbar, search for and select **Subscriptions**.

    ![The Azure portal displays with Subscriptions entered in the search box and the Subscriptions item selected from the search results.](media/subscription.png "Search for Azure Subscriptions")

1. On the **Subscriptions** screen, locate the desired Azure Subscription to deploy lab resources and copy the **Subscription ID** value. Save this value in a text editor for later use.

    ![The Subscriptions screen displays with the Subscription ID value copied from a list of subscriptions.](media/subscriptions(1).png "Azure Subscriptions")

### Task 2: Create an SAP Cloud Appliance

In this task, you will create an SAP Cloud Appliance by registering a new account on the SAP Cloud Appliance Library, configuring the necessary settings, and deploying an SAP S/4HANA 2023 fully-activated appliance on Microsoft Azure.

1. Open a new tab, and open the [SAP Cloud Appliance Library](https://cal.sap.com/) website.

1. Select the **Log On** button in the header of the website.

    ![](media/ex0.task2.2.png)

1. Select **Register** for creating a new account using the following details: 

    ![](media/ex0.task2.3.png)

    - **First Name**: odluser **(1)**
    - **Last Name**: <inject key="DeploymentID" enableCopy="false"></inject> **(2)**
    - **Email**: <inject key="AzureAdUserEmail"></inject> **(3)**
    - **Country**: United States **(4)**
    - Select **Continue** **(5)**

      ![](media/ex0.task2.5.png)

    - **Company**: Microsoft **(6)**
    - **Street Address**: 123 Main Street **(7)**
    - **City**: Anytown **(8)**
    - **ZIP/Postal Code**: 12345 **(9)**
    - **Country/Region**: United States **(10)**
    - Select **Continue** **(11)**
      ![](media/ex0.task2.6.png)
    - **Password**: <inject key="AzureAdUserPassword"></inject> **(12)**
    - **Re-Enter Password**: <inject key="AzureAdUserPassword"></inject> **(13)**
    - Select **Register** **(14)**
      ![](media/ex0.task2.7.png)
     

1. For activating the account, open **[Outlook](https://outlook.office365.com/mail/inbox/id/AAQkADljM2VkMzEwLTI2ZmUtNDlmNC1iYjA5LTBmNzlkYTY5NzJmYgAQABkSpmAaulhIiEXU2F3Yr90%3D)** on the same browser, and log in using the same credentials which you have used for registering, you will recieve an email from **SAP ID Service**, open that email, and select **Click here to activate your account**.
  ![](media/ex0.task2.10.png)

1. It will open the new page, on the **Account Successfully Activated** page, select **Continue**. 
   ![](media/ex0.task2.11.png)

1. On the **Terms and Conditions** screen, read the conditions of the 30-day trial license, and select the **I Accept** button to continue.

    ![](./media/updatedimg1.png)

1. On the SAP Cloud Appliance Library from the left navigation pane select **Appliance Templates (1)** screen, search for and locate the **SAP S/4HANA 2023, Fully-Activated Appliance (2)** item, and select the **Create Appliance (3)** button in the search results.

    ![The Appliance Templates screen displays with SAP S/4Hana 2023, Fully-Activated Appliance entered in the search box, and the Create Appliance button highlighted in the search results listing.](media/ex0.task2.12.png "Create SAP S4/HANA instance")

1. On the **Terms and Conditions** screen, read the conditions, and select the **I Accept** button to continue.

1. On the **Basic Mode: Create Appliance - Account Details** screen, enter the following values, then select the **Authorize (6)** button:

    | Field | Value |
    |-------|-------|
    | Name | **SAP plus extend and innovate (1)** |
    | Description | **SAP instance for the Microsoft Cloud Workshop (2)** |
    | Cloud Provider | Select **Microsoft Azure (3)** |
    | Subscription ID | Paste the **Subscription ID (4)** value from Task 1 |
    | Authorization Type | Select **Standard Authorization (5)** |

    ![The Basic Mode: Create Instance Account Details screen displays with the form filled with the preceding values. The Authorize button is highlighted.](media/sap-plus.png "SAP Create Instance")

1. When prompted, authenticate to Azure. If prompted, select to **Consent on behalf of your organization** and select **Accept**.

    ![A Permissions requested modal dialog displays with the Consent on behalf of your organization checkbox checked and the Accept button highlighted.](media/accept-sap.png "SAP Cloud Appliance Library Permissions Requested")

1. Returning to the **Basic Mode: Create Appliance** screen fill the **Appliance Details** form as follows and select **Create (5)**:

    | Field | Value |
    |-------|-------|
    | Name  | MS-SAP **(1)** |
    | Region | Select the nearest location **(2)** |
    | Password | Choose a strong password **(3)** |
    | Retype Password | Enter the chosen password **(4)**| 

    ![](media/ex0.task2.13.png)

    >**Note**: If difficulty arises using the **Basic** mode, an alternative is to use **Authorization with Application Type** using a [service principal](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

    >**Note:** Select **Proceed** on the **Warning** pop-up.

1. On the **Private Key** modal, select to **Store (1)** the private key in the SAP Cloud Appliance Library. Check the **Encrypt the private key with a password (2)** and enter a **Password (3)**: <inject key="AzureAdUserPassword"></inject>. Type the password once more in the **Retype Password (4)** textbox. Select the **Download (5)** button to download the encrypted key.

    ![The Private Key dialog displays with the password fields filled in and the Store and Download buttons highlighted.](media/ex0.task2.14.png "Private Key dialog")

    >**Note:** Close the **Warning** pop-up.

1. The deployment  will take approximately 90 minutes. The status will update on the Instances screen. Once complete, the status will indicate **Active**.

    ![The SAP instance displays with a status of Active.](media/active.png "SAP CAL Instance listing")

### Task 3: Deploy the Azure Resources

In this task, you will deploy the required Azure resources using Terraform Infrastructure as Code by running commands in Azure Cloud Shell with the PowerShell environment.

This lab utilizes Terraform Infrastructure as Code to deploy the necessary Azure resources.

1. Navigate back to the **Azure Portal**.

1. Use the **[\>_]** button to the right of the search bar at the top of the page to create a new Cloud Shell in the Azure portal, and select ***PowerShell*** environment.
    
    ![Azure portal with a cloud shell pane](./media/cloud-shell1.png)

    ![Azure portal with a cloud shell pane](./media/cl2.png)
   
1. In the **Getting Started** menu,choose **No storage account required (1)**,select your default **Subscription (2)** from the dropdown and click on **Apply (3)**

   ![Azure portal with a cloud shell pane](./media/cl3.png)

1. Note that Cloud Shell can be resized by dragging the separator bar at the top of the pane, or by using the—, **&#9723;**, and **X** icons at the top right of the pane to minimize, maximize, and close the pane. For more information about using the Azure Cloud Shell, see the [Azure Cloud Shell documentation](https://docs.microsoft.com/azure/cloud-shell/overview).
  
1. In the Cloud Shell pane, ensure the PowerShell language is selected. Clone the source code repository by issuing the following command.

    ```PowerShell
    git clone --branch MS-Innovation --single-branch https://github.com/CloudLabsAI-Azure/SAP-plus-extend-and-innovate-with-Data-and-AI
    ```

1. Navigate to the Terraform directory by executing the following command:

    ```PowerShell
    cd 'SAP-plus-extend-and-innovate-with-Data-and-AI/Hands-on lab/Resources/terraform'
    ```

1. Set the desired subscription by executing the following code, replace **SUBSCRIPTION_ID** with the value you recorded earlier in the lab setup.

    ```PowerShell
    az account set --subscription SUBSCRIPTION_ID
    ```

1. Establish a user context by executing the following command. Follow the prompts to authenticate to the Azure Cloud Shell.

    ```PowerShell
    az login
    ```

1. Initialize the Terraform code using the following command:

    ```PowerShell
    terraform init
    ```

1. Deploy the lab resources by executing the following command. When prompted to perform the actions, type `yes` and press <kbd>Enter</kbd>. It will take approximately 15 minutes for the deployment to complete.

    ```PowerShell
    terraform apply
    ```

    >**Note:** Wait for the script to finish; it will take approximately 10-15 minutes. If there are any errors, please ignore them and proceed with the lab.

1. Close the Cloud Shell panel if desired.

### Task 4: Prepare sales data in SAP

In this task, you will create a sales view in SAP, expose it as an OData service, and enable consumption by external services. The process includes connecting to your SAP instance via the SAP Cloud Appliance Library, using SAP Development Tools for Eclipse to define and activate the sales view, and configuring the OData service in SAP GUI.

This task demonstrates creating a sales view in SAP and exposing it as an OData service for consumption by external services.

1. On the SAP Cloud Appliance Library Instances page, select the **Connect** button on the **SAP** row.

    ![The SAP CAL instances listing displays with the Connect button highlighted next to the SAP item.](media/ex0.task4.1.png "Connect to SAP Instance")

1. On the **Connect to the instance** dialog, select the **Connect** link on the RDP row. This will download an RDP file.

    ![The Connect to the instance dialog displays with the Connect link highlighted on the RDP row.](media/ex0.task4.2.png "Connect via RDP")

1. Open the downloaded RDP file and log into the instance using the username `Administrator` and the password used when deploying the instance which you had specified in step 11 of Task2. 
    ![](media/ex0.task4.3.png)

    ![](media/ex0.task4.4.png) 

    ![](media/ex0.task4.5.png)

    >**Note:** Minimize the **Welcome** page.

1. From the desktop double-click the **SAP Dev Tools for Eclipse** PowerShell icon to install Eclipse. Accept the Terms and Conditions agreement. Installation can take up to 10 minutes, please be patient.

    >**Note**: The Eclipse IDE will open while the SAP tools are installed, let it run to completion.

   ![The SAP Dev Tools for Eclipse icon.](media/sapvm_pstoolsinstallicon.png "SAP Dev Tools for Eclipse icon")

   >**Note:** Once the installation complete, select **OK**.

1. Once the installation has completed, double-click the **SAP Dev Tools for Eclipse** icon. This will open the Eclipse development environment.

    ![SAP Dev Tools for Eclipse icon.](media/sapvm_eclipseicon.png "SAP Dev Tools for Eclipse icon")

    >**Note:** If you see a dialog box for the secure storage indicates Password hint required, select **No**.

    ![SAP Dev Tools for Eclipse icon.](media/securestorage.png "SAP Dev Tools for Eclipse icon")

1. Change the perspective to development by expanding the **Window (1)** menu, **Perspective (2)**, **Open Perspective (3)**, then selecting the **SAP HANA Development (4)** item.

    ![HANA Studio displays with the Window, Perspective, Open Perspective menu items expanded and the SAP HANA Development item is selected.](media/sapvm_changeperspective.png "Change to development perspective")

1. In the left panel, select **Resource**, select the **Project Explorer** tab then double-click the **S4H_100_s4h_ext_en** folder.
    ![](media/ex0.task4.6.png)

    ![The left panel of HANA Studio displays with the Project Explorer tab and S4H_100_s4h_ext_en folder selected.](media/sapvm_projectexplorer.png "Project Explorer")

1. When prompted for a password, enter `Welcome1` and select **OK**.

    ![A logon dialog displays with the Password text box and OK button highlighted.](media/sapvm_logins4h_ext.png "Logon dialog")

1. Expand the **File (1)** menu, then **New (2)** and select the **Other (3)** item.

    ![The File and New menus are expanded with the Other item selected.](media/sapvm_file_new_other.png "Create new file")

1. In the **Select a wizard** dialog, search for `Data Definition`. Select the **Data Definition (1)** item beneath the **ABAP / Core Data Services (2)** folders. Select **Next (3)**.

    ![The Select a wizard dialog displays with Data Definition entered in the search box and the Data Definition item selected from the search results. The Next button is highlighted.](media/sapvm_newdatadefinition.png "New Data Definition")

1. Fill in the **New Data Definition** dialog as follows, then select **Finish**.

    | Field | Value |
    |-------|-------|
    | Project | Retain the default **S4H_100_s4h_ext_en**. |
    | Package | Enter `$TMP` |
    | Name | Enter `ZBD_ISalesDocument_E` |
    | Description | Enter `ZBD_ISalesDocument_E` |

    ![The New Data Definition dialog displays populated with the preceding values.](media/sapvm_datadefinitionform.png "New Data Definition dialog")

1. Replace the code listing for **ZBD_ISALESDOCUMENT_E** with the following. Save the file.

    ```SAP
    @AbapCatalog.sqlViewName: 'ZBD_ISALESDOC_E'
    @AbapCatalog.compiler.compareFilter: true
    @AbapCatalog.preserveKey: true
    @AccessControl.authorizationCheck: #CHECK
    @EndUserText.label: 'Expanded CDS for Extraction I_Salesdocument'
    define view ZBD_I_Salesdocument_E as select from I_SalesDocument {
    key SalesDocument,
        //Category
        SDDocumentCategory,
        SalesDocumentType,
        SalesDocumentProcessingType,

        CreationDate,
        CreationTime,
        LastChangeDate,
        //@Semantics.systemDate.lastChangedAt: true
        LastChangeDateTime,

        //Organization
        SalesOrganization,
        DistributionChannel,
        OrganizationDivision,
        SalesGroup,
        SalesOffice,
        
        //SoldTo
        SoldToParty,
        _SoldToParty.CustomerName,
        _SoldToParty.Country,
        _SoldToParty.CityName,
        _SoldToParty.PostalCode,
        _SoldToParty.CustomerAccountGroup,
        
        //SalesDistrict
        SalesDistrict,
        
        CustomerGroup,
        CreditControlArea,
        PurchaseOrderByCustomer,
        
        //Pricing
        TotalNetAmount,
        TransactionCurrency,
        PricingDate,
        //RetailPromotion,
        //PriceDetnExchangeRate,
        //SalesDocumentCondition,
        
        //Billing
        BillingDocumentDate,
        BillingCompanyCode
    } where SDDocumentCategory = 'C'
    ```

1. Right-click in the whitespace of the **ZBD_ISALESDOCUMENT_E** view code window and select **Activate**.

    ![The ZBD_ISALESDOCUMENT_E view displays a context menu with the activate item selected.](media/sapvm_activatezbd_isalesdocument_e.png "Activate the ZBD_ISALESDOCUMENT_E view")

1. Right-click in the whitespace of the **ZBD_ISALESDOCUMENT_E** view once more, this time select **Open With (1)** and choose **Data Preview (2)**. This will display the raw data of the view. After reviewing the data, you can close this preview.

    ![The ZBD_ISALESDOCUMENT_E view displays a context menu with the Open With and Data Preview items selected.](media/sapvm_preview_menuitem_zbd_isalesdocument_e.png "Preview ZBD_ISALESDOCUMENT_E view")

    ![Raw data for the ZBD_ISALESDOCUMENT_E view displays in tabular format.](media/sapvm_previewsalesdocuments.png "Raw data preview of ZBD_ISALESDOCUMENT_E view")

1. Next, expose SAP sales data as an OData service. Add the following code immediately preceding the `define view` line of code of the **ZBD_ISALESDOCUMENT_E** file and save the file.

    ```ABAL
    @OData.publish: true
    ```

    ![A portion of a code window displays with the preceding line of code highlighted.](media/sapvm_addodataannotation.png "Add OData publish annotation")

1. Right-click in the whitespace of the **ZBD_ISALESDOCUMENT_E** file and select **Activate**.

1. Minimize the Eclipse development environment and double-click the **SAP Logon** icon located on the desktop of the virtual machine. This will open the SAP GUI application.

    ![The SAP Logon icon located on the virtual machine desktop displays.](media/sapvm_saplogonicon.png "SAP Logon icon")

1. From the top toolbar menu, select the **Log On** button.

    ![A portion of the SAP GUI toolbar displays with the Log On button highlighted.](media/sapvm_sapguilogonbutton.png "Logon")

1. Log in with the username `S4H_EXT` and the password `Welcome1`, press <kbd>Enter</kbd> to submit the form.

    ![The SAP GUI logon form displays populated with the preceding values.](media/sapvm_sapguilogonform.png "SAP GUI Logon form")

1. Once logged on, type `/n/IWFND/MAINT_SERVICE` in the toolbar menu transaction combo box and press <kbd>Enter</kbd>. This opens the **Activate and Maintain Services** window.

    ![The SAP GUI toolbar displays with the transaction combo box highlighted. The n/IFWND/MAINT_SERVICE transaction is entered in the transaction combo box.](media/sapvm_sapgui_transactioncombobox.png "SAP GUI transaction combo box")  

1. From the toolbar menu of the **Activate and Maintain Services** window, select the **Add Service** button.

    ![A portion of the Activate and Maintain Services toolbar displays with the Add service button highlighted.](media/sapvm_sapgui_maintsvcs_addservicebutton.png "Add Service")

1. Populate the **Add Selected Services** filter form as follows, and press <kbd>Enter</kbd>.

    | Field | Value |
    |-------|-------|
    | System Alias | Enter `Local` |
    | Technical Service Name | Enter `ZBD_*` |

    ![The filter form displays populated with the preceding values.](media/sapvm_sapgui_svcfilterform.png "Filter form")

1. From the list of results, select the **ZBD_I_SALESDOCUMENT_E_CDS** item.

    ![The filter results display with the ZBD_I_SALESDOCUMENT_E_CDS item highlighted.](media/sapvm_sapgui_svcresults.png "Filter results list")

1. In the **Add Service** dialog, select the **Local Object** button located in the **Creation Information** section. This will populate the **$TMP** value, and press <kbd>Enter</kbd>. An information dialog indicating success will display, dismiss this dialog.

    ![The Add Service dialog displays with the Local Object button highlighted.](media/sapvm_sapgui_addservicedialog.png "Add Service dialog")

1. On the **Add Selected Services** screen, select the **Back** button on the toolbar menu. This will open the **Activate and Maintain Services** window once more.

    ![A portion of the Add Selected Services toolbar displays with the Back button highlighted.](media/sapvm_sapgui_backbuttonaddservices.png "Back button")

1. On the **Activate and Maintain Services** screen, select the **Filter** button from the toolbar menu.

    ![A portion of the Activate and Maintain Services toolbar menu displays with the Filter button highlighted.](media/sapvm_sapgui_activateandmaintainfilterbutton.png "Filter services")

1. In the **Filter for Service Catalog** dialog, type `ZBD_*` in the **Technical Service Name** field and press <kbd>Enter</kbd>.

    ![The Filter for Service Catalog displays with ZBD_* entered in the Technical Service Name field.](media/sapvm_sapgui_svccatalogfilterdialog.png "Filter for Service Catalog dialog")

1. This action filters the **Activate and Maintain Services** screen to a single service. In the **ICF Nodes** pane, select the **SAP Gateway Client** button. If the **SAP GUI Security** dialog displays, check the **Remember My Decision** checkbox and select **Allow**.

    ![The ICF Nodes pane displays with the SAP Gateway Client button highlighted on the toolbar menu.](media/sapvm_sapgui_icfnodessapgatewayclientbutton.png "ICF Nodes SAP Gateway Client")

1. On the **SAP Gateway Client** window, select the **Execute** button from the toolbar menu. This tests the OData service, check the **Remember My Decision** checkbox and select **Allow**. Verify in the **HTTP Response** pane that the status code indicates **200**.

    ![The SAP Gateway Client window displays with the Execute button highlighted on the toolbar menu and the HTTP Response status code indicating 200.](media/sapvm_sapgui_sapgatewayclientexecution.png "SAP Gateway Client")

1. On the **SAP Gateway Client** window, select the **Entity Set** button on the toolbar menu.

    ![The SAP Gateway window displays with the EntitySets button highlighted.](media/sapvm_sapgui_sapgatewaycliententitysetsbutton.png "EntitySets")

1. On the **EntitySets** dialog, double-click the **ZBD_I_Salesdocument_E** item, check the **Remember My Decision** checkbox and select **Allow**.

    ![The Entity Sets dialog displays with teh ZBD_I_Salesdocument_E item highlighted.](media/sapvm_sapgui_entitysetsdialog.png "EntitySets dialog")

1. On the **SAP Gateway Client** window, select **Execute**. This service retrieves the sales documents via the OData endpoint. Verify the HTTP Response status code value is **200**.

    ![The SAP Gateway Client window displays with the Execute button highlighted. The HTTP Response status code is 200.](media/sapvm_sapgui_entitysetodataresult.png "EntitySet OData service execution results")

1. On the **SAP Gateway Client** window, select the **Back** button to return to the **Activate and Maintain Services** screen.

1. On the **ICF Node** pane, select the **Call Browser** button. This will bring up the **Security GUI** dialog once more. Copy the URL value for future use in the lab. After recording the value, close the dialog by selecting the **X** button on the upper right corner of the dialog window. This URL is the service endpoint for the sales document OData service.

    ![The SAP GUI Security dialog displays with the URL value highlighted.](media/sapvm_sapgui_sapguisecuritydialog.png "Service endpoint")

### Task 5: Prepare the business partner service in SAP

In this task, you'll update a Business Partner record in SAP to ensure the company name matches the imported Sales data. This involves activating and maintaining the relevant service in SAP, accessing the SAP Gateway Client to retrieve data, and using Postman to update the company name of a Business Partner record.

A service is available that allows for the update of a Business Partner record. A Business Partner record is where non-paying entities are flagged in the system. This service does not have company names that match what is imported from the Sales data. This task updates the company name of a record in SAP for use in the lab.

1. In the SAP UI, access the Activate and Maintain Service transaction by typing `/n/IWFND/MAINT_SERVICE` in the toolbar menu transaction combo box and press <kbd>Enter</kbd>. This opens the **Activate and Maintain Services** window.

    ![The SAP GUI toolbar displays with the transaction combo box highlighted. The n/IFWND/MAINT_SERVICE transaction is entered in the transaction combo box.](media/sapvm_sapgui_transactioncombobox.png "SAP GUI transaction combo box")  

<!-- 2. From the toolbar menu of the **Activate and Maintain Services** window, select the **Add Service** button.

    ![A portion of the Activate and Maintain Services toolbar displays with the Add service button highlighted.](media/sapvm_sapgui_maintsvcs_addservicebutton.png "Add Service")

3. Populate the **Add Selected Services** filter form as follows, and press <kbd>Enter</kbd>. Close the **Information** pop-up.

    | Field | Value |
    |-------|-------|
    | System Alias | Enter `Local` |
    | Technical Service Name | Enter `*GWSAMPLE*` |

    ![The filter pane displays with the preceding values populated.](media/sapui_addsvcfilter_gwsample.png "Add Service Filter Pane") -->

<!-- 4. From the list of results, select the **/IWBEP/GWSAMPLE_BASIC** item.

    ![The Select Backend Services pane displays with the /IWBEP/GWSAMPLE_BASIC item highlighted.](media/sapui_selectbackendsvc_gwsample.png "Select Backend Services pane")

5. In the **Add Service** dialog, select the **Local Object** button located in the **Creation Information** section. This will populate the **$TMP** value, and press <kbd>Enter</kbd>. An information dialog indicating success will display, dismiss this dialog.

    ![The Add Service dialog displays with the Local Object button highlighted.](media/sapvm_sapgui_addservicedialog_gwsample.png "Add Service dialog")

6. On the **Add Selected Services** screen, select the **Back** button on the toolbar menu. This will open the **Activate and Maintain Services** window once more.

    ![A portion of the Add Selected Services toolbar displays with the Back button highlighted.](media/sapvm_sapgui_backbuttonaddservices_gwsample.png "Back button") -->

1. On the **Activate and Maintain Services** screen, select the **Filter** button from the toolbar menu.

    ![A portion of the Activate and Maintain Services toolbar menu displays with the Filter button highlighted](media/sapvm_sapgui_activateandmaintainfilterbutton.png "Filter services")

1. In the **Filter for Service Catalog** dialog, type `*GWSAMPLE*` in the **Technical Service Name** field and press <kbd>Enter</kbd>.

    ![The Filter for Service Catalog displays with *GWSAMPLE* entered in the Technical Service Name field.](media/sapvm_sapgui_svccatalogfilterdialog_gwsample.png "Filter for Service Catalog dialog")

1. This action filters the **Activate and Maintain Services** screen to a single service. In the **ICF Nodes** pane, select the **SAP Gateway Client** button. If the **SAP GUI Security** dialog displays, check the **Remember My Decision** checkbox and select **Allow**.

    ![The ICF Nodes pane displays with the SAP Gateway Client button highlighted on the toolbar menu.](media/sapvm_sapgui_icfnodessapgatewayclientbutton.png "ICF Nodes SAP Gateway Client")

1. On the **SAP Gateway Client** window, select the **Entity Set** button on the toolbar menu.

    ![The SAP Gateway window displays with the EntitySets button highlighted.](media/sapvm_sapgui_sapgatewaycliententitysetsbutton_gwsample.png "EntitySets")

1. On the **EntitySets** dialog, double-click the **BusinessPartnerSet** item.

    ![The Entity Sets dialog displays with the BusinessPartnerSet item highlighted.](media/sapvm_sapgui_entitysetsdialog_gwsample.png "EntitySets dialog")

1. On the **SAP Gateway Client** window, select **Execute**. This service retrieves the sales documents via the OData endpoint. Verify the HTTP Response status code value is **200**.

    ![The SAP Gateway Client window displays with the Execute button highlighted. The HTTP Response status code is 200.](media/sapvm_sapgui_entitysetodataresult_gwsample.png "EntitySet OData service execution results")

1. On the **SAP Gateway Client** window, select the **Back** button to return to the **Activate and Maintain Services** screen.

1. On the **ICF Node** pane, select the **Call Browser** button. This will bring up the **Security GUI** dialog once more. Copy the URL value for future use in the lab. After recording the value, close the dialog by selecting the **X** button on the upper right corner of the dialog window. This URL is the service endpoint for the Business Partner OData service. 

    ![The SAP GUI Security dialog displays with the URL value highlighted.](media/sapvm_sapgui_sapguisecuritydialog_gwsample.png "Service endpoint")

    >**Note:** Minimize the RDP window.

1. Next, obtain the IP Address for the MSSAP-SAP1 virtual machine. In the [Azure Portal](https://portal.azure.com), enter `MSSAP-SAP1` (1) in the search box located in the top toolbar and select the **MSSAP-SAP1 (2)** virtual machine from the filtered list of resources.

    ![The Azure Portal toolbar search box displays with MSSAP-SAP1 text and the MSSAP-SAP1 virtual machine resource selected.](media/virtualmachinesap.png "Search for VM")

1. On the **MSSAP-SAP1** virtual machine Overview screen, copy the IP address and record it for future use.

    ![The MSSAP-SAP1 Virtual machine screen displays with the Public IP address highlighted.](media/publicip.png "MSSAP-SAP1 Virtual Machine Overview")

    >**Note**: This IP address can change, it does not have a static IP. Please obtain the current IP address.

1. Open a new tab in the browser, download [Postman](https://www.postman.com/downloads/) for **Windows 64-bit**. Open the downloaded the file. It will start the installation process.

1. Enter the following details:

    - **Work email**: <inject key="AzureAdUserEmail"></inject>
    - Select **Create Free account**
    - **Username**: **odluser<inject key="DeploymentID" enableCopy="false"/>**
    - **Password**: <inject key="AzureAdUserPassword"></inject>
    - **Verify you are human**: Select the checkbox
    - Select **Create Free Account**

        >**Note:** Select **Open** on the pop-up.

1. Open Postman on your local machine and provide **Your name** as **odluser<inject key="DeploymentID" enableCopy="false"/>** **(1)** and select any of the role from the list (2). Click on **Continue (3)**.

    ![](./media/updatedimg2.png)

1. In the main pane select **Import** from the Workspace toolbar to import a collection.

    ![The Postman interface displays with the Import button highlighted.](media/pm_importcollection.png "Import Collection")

1. On the Import dialog, then enter the following URL.

    ```text
    https://raw.githubusercontent.com/CloudLabsAI-Azure/SAP-plus-extend-and-innovate-with-Data-and-AI/MS-Innovation/Hands-on%20lab/Resources/postman/SAP.postman_collection.json
    ```


1. In the Postman Collections list, select the **SAP(1)** collection. Select the **Variables (2)** tab and enter the MSSAP-SAP1 IP address in the **INITIAL VALUE (3)** and **CURRENT VALUE (4)** column. Select **Save (5)** on the collection.

    ![The Variables for the SAP collection displays with the initial value and current value set to an IP address. The Save button is highlighted.](media/mssap.png "Set ip-address values")

1. Expand the SAP Collection and select the **GET Company** request. Select **Send**. Notice how the CompanyName is currently **Office Line Prag**.

    ![The SAP Collection is expanded with the GET Company request selected. The Send button is highlighted. The CompanyName of Office Line Prag is highlighted in the response.](media/effect.png "Get Company Request - Office Line Prag Company Name")

1. In the SAP Collection, select the **Patch Company** request and select **Send**. The response should show the status of **204 No Content**.

    ![The Patch Company request displays with the Send button highlighted. The status indicates 204 No Content.](media/patchcompany.png "PATCH company")

1. Return to the **GET Company** request and select **Send**. Notice how the CompanyName now shows **Bigmart**.

    ![The GET Company request displays with the Send button highlighted and the CompanyName value set to Bigmart in the response"](media/bigmart.png "CompanyName updated to Bigmart")

1. Retain this collection in Postman for use during the hands-on lab.

### Task 6: Prepare payment data in Cosmos DB

Raw payment data is available in Azure Data Lake storage. This task walks through loading raw payment data into Cosmos DB by leveraging Azure Synapse Analytics. First, linked services are created, these act as the connection strings to external compute resources. Next, integration datasets are created, these indicate the location and shape of the data being used in the migration. Finally, a pipeline is created to orchestrate moving data from Azure Data Lake Storage Gen2 to Cosmos DB.

In this task, you will create linked services in Azure Synapse Analytics for Azure Data Lake Storage Gen2 and Azure Cosmos DB for NoSQL. You'll then define integration datasets to represent your source (CSV data) and sink (Cosmos DB collection). Finally, you'll build a pipeline to transfer data from the CSV file to Cosmos DB, publish the pipeline, and trigger it to run.

#### Step 1: Create linked services in Azure Synapse Analytics

1. Navigate to the [Azure Portal](https://portal.azure.com), open the **sap_plus_extend_and_innovate** resource group.

2. From the list of resources, locate and select the Synapse Workspace item named **sapdatasynws{SUFFIX}**.

3. On the Synapse workspace screen, select **Open Synapse Studio**.

    ![The Synapse workspace screen displays with the Open Synapse Studio card highlighted.](media/ap_opensynapsestudiocard.png "Open Synapse Studio")

4. In Synapse Studio, select the **Manage** hub from the left menu.

    ![Synapse Studio displays with the Manage hub selected in the left menu.](media/ss_managehub_menu.png "Manage hub")

5. Beneath the **External connections** section, select **Linked services**. Then select **+ New** from the Linked services toolbar menu.

    ![The Manage Hub displays with the Linked services item selected. The + New button is highlighted on the Linked services screen toolbar.](media/ss_newlinkedservicemenu.png "New Linked service")

6. In the New linked service blade, search for and select **Azure Data Lake Storage Gen2**. Select **Continue**.

    ![The New linked service blade displays with the Azure Data Lake Storage Gen2 item highlighted.](media/ss_newlinkedsvc_adlsgen2search.png "New ADLS Gen2 Linked service")  

7. In the New linked service - Azure Data Lake Storage Gen2 form, fill it in as follows and select **Create (5)**. Unspecified fields retain their default values.

    | Field | Value |
    |-------|-------|
    | Name | Enter `datalake` **(1)** |
    | Authentication type | Select **System-assigned managed identity** **(2)** |
    | Azure subscription | Select your Azure subscription **(3)** |
    | Storage account name | Select **sapadls{SUFFIX}** **(4)** |

    ![The New Linked service form for ADLS Gen2 displays populated with the preceding values.](media/updatedimg3.png "New ADLS Gen2 Linked service form")

8. On the Linked services screen, select **+ New** from the toolbar menu. On the New Linked service blade search for and select **Azure Cosmos DB for NoSQL**. Select **Continue**.

    ![The New linked service blade displays with Azure Cosmos DB for NoSQL selected. The Continue button is highlighted.](media/ss_linkedservice_cosmosdbsql.png "New Cosmos DB Linked Service")

9. In the New linked service - Azure Cosmos DB for NoSQL form, fill it in as follows, then select the **Create** button. Unspecified fields retain their default values.

    | Field | Value |
    |-------|-------|
    | Name | Enter `payment_data_cosmosdb` |
    | Authentication type | Select **Account key** |
    | Azure subscription | Select your Azure subscription |
    | Azure Cosmos DB account name | Select **sap-cosmos-{SUFFIX}** |
    | Database name | Select **SAPS4D** |

    ![The new Linked service form for Cosmos DB displays populated with the preceding values.](media/ss_newlinkedservice_cosmosdb_form(1).png "New Cosmos DB Linked service form")

10. On the Synapse Studio toolbar menu, select **Publish all**. Select **Publish** on the verification blade.

    ![The Synapse Studio toolbar menu displays with the Publish all button highlighted.](media/ss_publishall.png "Publish all")

#### Step 2: Create source and sink integration datasets

The source data is payment data that is located in Azure Data Lake Storage Gen2 in CSV format. The sink is the paymentData Cosmos DB collection.

1. In Synapse Studio, select the **Data** hub.

   ![The left menu of Synapse Studio displays with the Data hub highlighted.](media/ss_datahub_menu.png "Data hub")

2. From the center pane, select the **Linked (1)** tab. Expand the **Azure Data Lake Storage Gen2 (2)** item, followed by the **datalake (3)** item, and select the **payment-data-csv (4)** container. From the **payment-data-csv** tab, select the **paymentData_CAL2021.csv (5)** file. Next, select **New integration dataset (6)** from the toolbar menu.

    >**Note**: If you do not see **datalake** in the **Azure Data Lake Storage Gen2** items, you may need to refresh.

    ![The Data hub displays with the Linked tab selected and the Azure Datalake Storage Gen2 item expanded along with the datalake item. The payment-data-csv container is selected. The paymentData_CAL2021.csv file is selected with the New integration dataset menu item highlighted in the toolbar menu.](media/payment-datacsv.png "Data hub new integration dataset")

3. On the New integration dataset blade, enter `payment_data_csv` for the name and select **DelimitedText** for the format. Select **Create**.

    ![The New integration dataset blade displays populated with the preceding values.](media/paymentdata.png "New integration dataset for payment data")

4. On the **payment-data-csv** integration dataset tab, change the **Column delimiter** value to **Semicolon (;) (1)** and check the checkbox for the **First row as header (2)** field.

   ![The payment-data-csv integration dataset form displays with the column delimiter value changed to semicolon and the first row as header field checked.](media/ss_paymentdata_dataset_form.png)

5. If desired, select the **Preview data** button to view a sample of the data.

6. Remaining in the Data hub, expand the **+** menu in the center pane and select **Integration dataset** beneath the **Linked** header.

    ![The Data hub displays with the + menu expanded and the Integration dataset option highlighted.](media/ss_datahub_newintegrationdataset.png "New Integration dataset")

7. On the New integration dataset blade, search for and select **Azure Cosmos DB for NoSQL**. Select **Continue**.

    ![The New integration dataset blade displays with the Azure Cosmos DB for NoSQL item selected.](media/ss_newintegrationdataset_comsosdbsearch.png "New Azure Cosmos DB integration dataset")

8. On the **Set properties** blade, fill the form as follows, then select **OK**.

    | Field | Value |
    |-------|-------|
    | Name | Enter `payments_cosmosdb` |
    | Linked service | Select **payment_data_cosmosdb** |
    | Container | Select **paymentData** |
    | Import schema | Select **None** |

    ![The Set properties blade displays populated with the preceding values.](media/ss_cosmosdbintegrationdataset_setpropertiesform.png "Set Cosmos DB integration dataset properties")

9. On the Synapse Studio toolbar menu, select **Publish all**. Select **Publish** on the verification blade.

#### Step 3: Create pipeline to ingest payment data into Cosmos DB

1. In Synapse Studio, select the **Integrate** hub from the left menu. Then from the center pane menu, expand the **+** button and choose **Pipeline**.

    ![Synapse Studio displays with the Integrate hub selected in the left menu and the + menu expanded in the center panel. The pipeline item is selected.](media/ss_newpipelinemenu.png "New pipeline")

2. In the **Properties** blade of the new pipeline, set the name field to **UploadPaymentsToCosmosDB**. If desired, press the **Properties** button in the pipeline toolbar to collapse this blade.

    ![The Properties blade displays with the name set to UploadPaymentsToCosmosDB and the Properties button is highlighted.](media/ss_paymentspipeline_propertiespane.png "Pipeline properties")

3. In the Activities pane, expand the **Move & Transform** item then drag and drop a **Copy data** activity to the pipeline canvas.

    ![The pipeline editor displays with the Move & Transform item expanded in the Activities panel and an arrow indicates the drag and drop operation of the Copy data activity to the pipeline canvas.](media/ss_paymentspipeline_copydata_movetocanvas.png "Add Copy data activity to pipeline")

4. With the Copy data activity selected in the pipeline editor, select the **Source** tab. Select **payment_data_csv** as the **Source dataset**.

    ![The Copy data activity Source tab displays with payment_data_csv selected as the Source dataset.](media/ss_paymentpipeline_copysource.png "Source dataset")

5. With the Copy data activity selected in the pipeline editor, select the **Sink** tab. Select **payments_cosmosdb** as the Sink dataset.

    ![The Copy data activity Sink tab displays with the payments_cosmosdb selected as the Sink dataset.](media/ss_paymentpipeline_copysink.png "Sink dataset")

6. On the Synapse Studio toolbar menu, select **Publish all**. Select **Publish** on the verification blade.

    ![The Synapse Studio toolbar menu displays with the Publish all button highlighted.](media/ss_publishall.png "Publish all")

7. Once published, expand the **Add trigger** toolbar item and choose **Trigger now**. Select **OK** on the Pipeline run blade. This will run the pipeline to copy the payments data to Cosmos DB.

    ![The payments pipeline Add trigger item is expanded with the Trigger now item selected.](media/ss_paymentpipeline_triggernow.png "Trigger payments pipeline")

8. In Synapse Studio, select the **Monitor** hub. Beneath the Integration header of the center pane, select **Pipeline runs**. Monitor the payments pipeline run until it has completed. The Refresh button in the toolbar may be used to update the monitoring table.

    ![Synapse Studio displays with the Monitor hub selected from the left menu, the Pipeline runs selected in the center panel, and the payment pipeline listed as in progress. The Refresh button is highlighted.](media/ss_monitorpaymentspipeline.png "Monitor hub")

9. Verify the data by returning to the Azure Portal, opening the **sap_plus_extend_and_innovate** resource group, then locating and opening the **sap-cosmos-{SUFFIX}** Cosmos DB resource.

10. On the Azure Cosmos DB account screen, select **Data Explorer** from the left menu. In the SQL API panel, expand the **SAPS4D** and **paymentData**. Select **Items** tab. This will display the contents of the selected document for review.

    ![The Azure Cosmos DB account screen displays with the Data Explorer item highlighted in the left menu. In the SQL API pane, the SAPS4D database is expanded along with the paymentData collection. The items option is selected beneath the collection. One document is highlighted in the paymentData tab and the screen displays the selected document's content.](media/portal_cosmosdb_paymentdatapreview.png "View document in Cosmos DB")

You should follow all steps provided *before* performing the Hands-on lab.

## Summary

In this lab, you configured data integration in Azure Synapse Analytics by creating linked services for Azure Data Lake Storage Gen2 and Azure Cosmos DB. You set up integration datasets for the source CSV file and the Cosmos DB target collection. A pipeline was created to move data from the CSV file into Cosmos DB. After setting up and publishing the pipeline, you triggered it to run and monitored the data transfer, verifying the results in Azure Cosmos DB.

## You have successfully completed this exercise. Select **Next >>** to proceed to the next one.
