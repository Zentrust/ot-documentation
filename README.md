# Integrating with ForgeRock for Consent
You can integrate a ForgeRock account with OneTrust to manage consent preferences for users. This integration can achieved by building two workflows: one to update consent in ForgeRock and one to sync ForgeRock consent with the OneTrust CMP.

## To create a new user
1. Log in to your ForgeRock account. 
2. From the **Quick Start Dashboard**, click **Manage Users**. The **User List** screen appears. 
3. Click the *New User* button. 
4. Complete the fields, then click the *Save* button. The new user details will appear. 
> **Note:** Notice the default fields and the preferences fields are set by default. 

## To configure a Preference Center in OneTrust 
With **Universal Consent & Preference Management**, you can create a Preference Center that data subjects can use to define whether to give or withdraw their consent on a deeper level. When you create a Preference Center, you'll need to give a name, select whether to use the Original or Enhanced template, and define additional basic details. You can then begin adding purposes that you want to make available for users to specify their consent, customize the branding, and finalize any additional configurations that you want to make.
<br></br>
For this integration, there are **two** preferences created in ForgeRock. You will need to create a purpose and two topics, then associate that purpose to a Preference Center in OneTrust. You'll also need to record the Preference Center ID and Purpose ID to use later in the workflow configuration.

## To set up credentials in OneTrust
Once you have performed the above steps for creating the Preference Center, you will need to setup credentials prior to creating the integration workflow that will be triggered when the OneTrust Preference center is updated. For the purposes of this documentation, we are using Basic Authentication credentials.
1. On the ​**Integrations​** menu, click ​**Credentials​​**. The Credentials​​ screen appears.
2. Click the ​*Add New​* button. The ​Select System​​ screen appears.
3. Enter **Forgerock** in the ​Select System​ field, then click the ​*Next​* button. The ​Enter Credential Details​​ screen appears.
4. Select ​**HTTP​** from the ​Connector Type​​ field. The screen will populate suggested fields based on this connector type.
5. Select ​**Basic**​ from the ​Auth Type​​ field. The screen will populate suggested fields based on this authentication type.
6. Create a new ​Basic​​ credential using your ForgeRock instance information using the provided fields.
<img src="https://cdn.onetrust.com/images/my-ot/enter-credential-details.png" width="500"/>

7. After you've provided the fields, click the *Save* button. 



|Field |Description |
|----|----|
|Credential Name | Required: Enter a name to identify the credential |
|Description |Enter a description of the credential. | 
|Connector Type |Required: Select HTTP as the credential connector type. | 
|Auth Type |Required: Select ​Basic​​ as the authorization type. | 
|Protocol |Required: Enter HTTP as the protocol for the credential. | 
|Hostname |Enter the hostname for the credential. | 
|Test URL |Enter a test URL for the new credential. | 
|Default Headers: Key & Value |Select the type of key which should be used in the header of the call made by the connection and its respective value.<br></br>You can add additional keys if your credential requires more than one header. | 
|Username |Required: Enter the username for the credential. | 
|Password |Enter the password for the credential. | 
|mTLS enabled | Select to enable mutual Transport Layer Security (mTLS) for the credential.<br></br>Enable mutual transport layer security (mTLS) for this credential to establish a secure and trusted connections with external servers. This requires the provision of a Client Certificate, Privacy Key, and an optional Private Key Passphrase.| 
|Serve Certificate Chain | Post an optional, custom server certificate to this credential to establish a secure and trusted connection with external systems not using a certificate issued by a trusted authority|

## To create the OneTrust Consent Updates ForgeRock workflow 
You will need to create the initial workflow that will sync consent preferences between Onetrust and ForgeRock.
1. On the ​**Integrations**​ menu, select ​**Gallery​​**. The ​Integrations Gallery​​ screen appears.
2. Search for **Forgerock** in the search bar, then select the tile.

    <img src="https://cdn.onetrust.com/images/my-ot/integrations-gallery-search.png" width="500"/>

3. Click the ​*Add*​ button on the ​**Consent: OneTrust Consent Updates ForgeRock​** tile. The ​Add Workflow Name​​ modal appears.

    <img src="https://cdn.onetrust.com/images/my-ot/consent_OT-consent-updates-FR.png" width="500"/>

4. Provide a name and select the existing credential for this workflow, then click the ​*Create​* button. The ​Workflow Builder​​ screen appears.

    <img src="https://cdn.onetrust.com/images/my-ot/add_workflow1.png" width="400"/>

5. The initial workflow for the ForgeRock integration will appear in the ​Workflow Builder​​ screen.

    <img src="https://cdn.onetrust.com/images/my-ot/workflow1.png" width="550"/>

6. Click the ​*Save*​​ button.
7. Click the *Activate* button to enable the workflow. 

## To configure the OneTrust Consent Updates ForgeRock workflow 
### Preference Center Trigger
1. On the ​Workflow Builder​ screen, click the ​*Edit​​* icon on the initial trigger action.
2. Enter the ​**Preference Center ID**​ for the Collection Point payload value. The Preference Center ID can be obtained from your Preference Center URL or by navigating to the ​Integrations​ tab of the ​Preference Center Details​​ screen.
   >**Example:** https://<your-URL>/​​2a0fcf53-e712-4744-a05b-417b5d6ccb6d​​
3. Enter the ​**Purpose ID**​ for the Purpose ID payload value. The Purpose ID can be obtained from the ​Purposes Details​​ screen.

    <img src="https://cdn.onetrust.com/images/my-ot/pref-purpose-id.png" width="500"/>

4. Click the *Done* button. 

### Conditional Logic - True or False
When the user opts in or out to marketing, the selection is sent via an API call with either ​True​ or ​False​​. You will need to modify the first conditional logic sequence for ​ACTIVE​ for opt ins and the second for ​WITHDRAWN​​ for opt outs.

#### **ACTIVE**

<img src="https://cdn.onetrust.com/images/my-ot/conditional-logic_ACTIVE.png" width="500"/>

#### **WITHDRAWN**


<img src="https://cdn.onetrust.com/images/my-ot/conditional-logic_WITHDRAWN.png" width="500"/>

## To create the Consent: Update Consent from ForgeRock workflow 
You will need to create a secondary integration workflow that will pull in consent preferences from ForgeRock to Onetrust.
1. On the ​**Integrations​** menu, select **​Gallery​​**. The ​Integrations Gallery​​ screen appears. 
2. Search for Forgerock in the Gallery screen search bar, then select the tile.
    <img src="https://cdn.onetrust.com/images/my-ot/integrations-gallery-search.png" width="500"/>

3. Click the ​*Add​* button on the ​**Consent: Update Consent from ForgeRock**​ tile. The ​Add Workflow Name​​ modal appears.

    <img src="https://cdn.onetrust.com/images/my-ot/consent_update-consent-from-FR.png" width="500"/>

4. Provide a name and select the existing credential for this workflow, then click the *​Create​* button. The ​Workflow Builder​​ screen appears.

    <img src="https://cdn.onetrust.com/images/my-ot/add_workflow2.png" width="400"/>

5. The update consent workflow for the ForgeRock integration will appear in the ​Workflow Builder​​ screen.

    <img src="https://cdn.onetrust.com/images/my-ot/workflow2.png" width="600"/>

6. Click the *Save* button. 
7. Click the *Activate* button to enable the workflow. 

## To configure the Consent: Update Consent from ForgeRock workflow 
### Preference Center ID
1. On the ​Workflow Builder​ screen, click the ​*Edit​* icon on the ​**PUT Update**​ action under the ​**Apply to Each**​​ sequence.
2. Enter the ​**Preference Center ID​​** to update the consent from ForgeRock back to the respective Preference Center.

    <img src="https://cdn.onetrust.com/images/my-ot/second-workflow-pref-center.png" width="500"/>