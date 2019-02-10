# MS-SC-TS316 Script Notes

## Intro/Story Setup

### Mavi

- CISO for Contoso
- Contoso not ready for challenges of protecting information
- GDPR and other Regulatory pressures
- Needs solution before presenting to CEO/CIO 
- Assembled team (Shubha, Kartik, Enrique, Kevin, Denis)
    - Need all sensitive information protected today, what do you have for me?

## Discovery

### Shubha

- Start with Discovery/Understanding current Data Risk/Exposure - Make risk/prioritization decisions based on real data
- I have heard the AIP scanner may be able to help
- Denis, do you have details on that?

### Denis

- Intro to AIP scanner MVP
    - _Log Into Client01_
    - _Walkthrough Profile/Settings/Repos_
    - _Switch to Scanner01_
    - _Run Install-AIPScannerPreview.ps1_
        - Admin@AIPDemo.com
        - AIP4life!

## Taxonomy Considerations

### Mavi

- _To Shubha:_ We need to have a clear view of how our users will classify all data. What have you heard about best practices for setting up our labels and classification taxonomy?

### Shubha

- I just read a new post about the AIP Deployment Aceleration Guide on the AIP team blog and they recommend using the default Microsoft Classification Taxonomy. Since we don't have a taxonomy at Contoso yet, let's start with that.
    - _Switch to Client01_
    - _Reviews Taxonomy in AIP portal_

## Infrastructure Considerations

### Enrique

- So does AIP only work on Windows?

### Kartik

- Will work in Office on all platforms plus 3rd parties that integrate with the MIP SDK
- MIP SDK background info??
- Enable labels for Unified Labeling
    - _In AIP portal, go to Unified Labeling_
    - Explain pre activated labels
    - _Click link to go to SCC_
    - _Show labels matching AIP portal_
    - _Show label policy_
- Mention that labels will now show on Mac Office and other Unified Labeling clients
    - _Switch to Mac VM connected to AIPDemo tenant_
    - _Show Sensitivity Labeling in Word_

## User Classification Guidance 

### Mavi

- Do we have some way to help our users classify data correctly? There are a lot of options and we want to make sure they have a seamless experience if possible.

### Shubha

- Actually, yes. Because we have AIP P2, we can create conditions to recommend or automatically classify well defined sensitive data types
- I spoke to our CxE and got us added to the private preview of the recommendations dashboard. Based on the Discovery data collected by the scanner, we can see Recommendations in the AIP console and directly add conditions to labels.
    - _Click on Recommendations (Preview)_
    - _Click on US SSN_
    - Should we create recommended or automatic conditions?

### Enrique

- We should do both. We can have recommendations for users while fine tuning conditions to ensure low false positive rate, and automatic for well defined conditions to use with AIP scanner.

### Shubha

- OK, I will add those.
    - _Create a recommended condition for SSN tied to Confidential \ Extended_
    - _Create an automatic condition for Credit Card tied to Confidential \ All Employees_
- As a side note, we configured additional automatic conditions in this tenant to show full results from the AIP scanner

## Reviewing AIP Analytics

### Mavi

- On that note, what have we gotten back from the AIP scanner discovery scan?

### Denis

- We can go to the Data Discovery (Preview) dashboard in the AIP console to see our results
    - _Switch to Data Discovery_

### Mavi

- Wow, that is a lot of sensitive data!, what can we do about all of this risk?

### Denis

- That is no problem. Based on the conditions we defined earlier, I can simply use the AIP P2 features of the AIP scanner to enforce labeling and protection on all of the documents we classified during the discovery scan.
    - _Go to Scanner Profile_
    - _Switch to Always, Policy Only, Enforce_
- Now we have set the scanner to run continuously on all defined repositories and automatically label and protect any documents it finds in those repositories.  It can even discover new files placed in repositories in less than a minute!

## MIP SDK Integrations/Applications

### Kartik

- Now that we have protected our files using the AIP scanner and enabled and deployed the MIP labels in the Security and compliance center, we can take a look at some of the new functionality that is made possible via the MIP SDK.
- First, we are going to review a PDF that was protected using the new ISO compliant PDF standard in use in the AIP client and scanner
    - _Browse to ```\\Scanner01\Documents``` and open be-id.pdf using Edge_

- As you can see, because the PDF is encrypted, we are not able to see the contents in Edge.  Now, we open it in the Adobe PDF Reader and we can see that it open and displays the classification across the top of the window.
    - _Open protected pdf in Adobe Reader_

- We can also open Office applications on the Mac and see the native MIP sensitivity labels in the GA Office client.
    - _Switch to the Mac VM_
    - _Launch Word and demo Sensitivity labels_

- More details about MIP integrations (depending on time)

## WD ATP/WIP Integration

### Mavi

- What about protecting sensitive documents on client systems?
- Can we protect these files from being copied to USB drives and unsanctioned locations?

### Kevin

- We absolutely can! We have deployed Windows Defender ATP and a Windows Information Protection policy to our clients so with the new integration between WD ATP and the MIP lables, we can ensure that our sensitive documents are being secured.
    - _Show block copying HC document to personal onedrive and gmail_

## Cloud Service Protection

### Shubha

- We also have some of our data migrated to SharePoint Online and other cloud services.  Doesn't Office 365 use the same Labels, Rules, and Protections we configured in AIP?

### Enrique

- Well, because we have EMS E5, we also own Microsoft Cloud App Security.  I can add a rule in MCAS to discover and protect content as it is downloaded from cloud services.
    - _Show configured policy in MCAS portal_
    - Now when we go to our SPO site, we can do co-authoring and search while the files are in the cloud, but as soon as we download one...
    - _Download sensitive file_
    - ...we see that it is now protected as Highly Confidential \ All Employees

## Exchange Online Rules

### Mavi

- WOW! That is awesome! The CEO and CIO will LOVE this! 
- They have also voiced some concerns of accidental leakage of sensitive data via Email.  Is there anything we can do to prevent sensitive content from being emailed out in the clear?

### Enrique

- You bet. We can go into the Exchange Online control panel and configure mail flow rules to catch sensitive data on egress. Let's take a quick look at that.
    - _Open ECP at ```https://outlook.office365.com/ecp```_
    - _Create a new MFR for protection of sensitive content (predefined rule already in place)_
    - _Send email with sensitive content to external account kemckinnmsft@gmail.com_
    - _Open mail on Mac VM to show OME experience_

## Conditional Access

### Mavi

- So with credential theft being so prevalent today, is there anything we can do to ensure that our users are actually who they say they are before they open protected content?

### Shubha

- Absolutely! AIP is one of the applications available to be secured via Conditional Access.  That way, when users open protected content they will be prompted to authenticate.
    - _Log into AAD Conditional Access panel and configure CA policy_
    - Now that we have that in place, we can clear our AIP and Windows Auth tokens and attempt to open the protected content
    - _On Client01 Reset AIP client and delete ADAL credentials_
    - _Open Test HC.docx on desktop_
    - _Log in as adams@aipdemo.com/pass@word1 and enter code from Kevin's text_

## Review Analytics

### Denis

- Let's take a look at the new AIP Analytics dashboards and see what information we have gathered
    - _Switch to Client01 and Open dashboards and explain features_



### Mavi

- This is perfect! With this, I am ready to present the CEO and CIO with an all-up Information Protection story that should dramatically reduce our risk of sensitive data leakage. They are going to love it.

## Action Items

### Mavi

- So, in conclusion we would like for each of you to go out and tell this story to your customers.
- We have just posted about our AIP Deployment Acceleration Guide at https://aka.ms/AIPBlog and we recommend you use internalize that information to help you accelerate customer's information protection journey.

## Wrap-up/Questions