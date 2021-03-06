# Exercise 1 - Configure SAP Edge Services

In Exercise 1, we will create a SAP Edge Services project.  A project is an aggregation of entities such as sensor models, rules, rule data sources, actions, connectors, and runtime settings where you can define and manage each entity and publish the project to create a configuration for the Streaming Service.      After you complete Exercise 1, you will then move to Exercise 2 to deploy such streaming service configuation on the gateway to automatically trigger the cretion of a work order based on live IoT sensor data.

## Step 1.1 Create an Edge Designer Project

After completing these steps you will have created an edge designer project.

1. Log onto SAP Edge Services - Policy Service 
   
   Open the following URL and log in using the __user name__ (email address) and __password you've written down from Exercise 0__
   
   URL: https://iot-iottrainingdev-709f.edge-demo.cfapps.eu10.hana.ondemand.com/
<br>![](/exercises/ex1/images/Ex1_Step1_1.png)

   Note: After logon you will land on the main SAP Edge services screen.

2. Click the __Edge Designer__ tile.
<br>![](/exercises/ex1/images/Ex1_Step1_2.png)

3. Click __+__ to add a new project.
<br>![](/exercises/ex1/images/Ex1_Step1_3.png)

   Note: A project is an aggregation of entities such as sensor models, rules, rule data sources, actions, connectors, and runtime settings where you can define and manage each entity and publish the project, which creates a configuration for the Streaming Service.

4. Enter the following details in the next screen, and click Create.
   
   Note: Throughout the exercise, __YOU MUST REPLACE XX__ with __the two-digit Participant ID__ you get from Exercise 0<br>
   - Name: __TechEdXX__
   - Description: __TechEdXX__
   - Profile Delimiter: __>>>__

<br>![](/exercises/ex1/images/Ex1_Step1_4.png)

## Step 1.2 Add data model to project

After completing these steps you will have added data model to the project you just created.

1.	Click __Data Model__, and then press __+__.
<br>![](/exercises/ex1/images/Ex1_Step2_1.png)
In SAP IoT, we have already defined a sensor type __Boiler__, which you are going to use in this project. The Boiler sensor type has two capabilities: __Temperature__ and __Pressure__

2.	Select the following values from the dropdown, and then click __Create__:
   
   - Sensor Type: __Boiler__
   - Capability Name: __BoilerTraining__
   - Temperature: __Check Box Checked__
 
<br>![](/exercises/ex1/images/Ex1_Step2_2.png)

3. Fidelity enables you to configure the SAP Edge Services to selectively send IoT data to cloud (or other target applications) or save at the edge in the configured time interval.

   To set fidelity, select your data model and click __Fidelity__.
<br>![](/exercises/ex1/images/Ex1_Step2_3_1.png)

   Set the __Edge Fidelity__ and the __Outbound Fidelity__ to __Enabled__, and enable __IoT Service Cloud Connector__.

   Click __Save__.
<br>![](/exercises/ex1/images/Ex1_Step2_3_2.png)   

4. Click __Validate__ at the top-right.
   <br>After a successful validation your screen should look like this:
<br>![](/exercises/ex1/images/Ex1_Step2_4.png)   

## Step 1.3 Define action

After completing these steps you will have defined an action that will trigger the creation of the work order. The action will be triggered by a streaming rule that will be created later.

1. Select __Actions__, and then click __+__ to the right.
<br>![](/exercises/ex1/images/Ex1_Step3_1.png)   

2. Enter the following values for your action, and then click __Create__.
   
   - Action Name: __Create Work Order__
   - Description: __Create Work Order__
   - Action Type: Select from Drop Down __Create Work Order__
   - Device ID: __${deviceId}__
   - Subject: __${deviceId} : Temperature too high__
   - Remarks: __${deviceId} : Temperature too high__
   - Main Work Center: __RES-0100__
<br>![](/exercises/ex1/images/Ex1_Step3_2.png) 

## Step 1.4 Define rule

After completing these steps you will have defined a rule that will trigger the action we created in the previous step.

1. Click __Rules__, and then click __+__.
<br>![](/exercises/ex1/images/Ex1_Step4_1.png)

2. Add the following values to the rule definition, and then click __Create__
   - Name: __Check Temperature__
   - Description: __Check Temperature rule__
   - Send Edge Event: __enabled__
<br>![](/exercises/ex1/images/Ex1_Step4_2.png)

3. Select the rule by clicking its name.
<br>![](/exercises/ex1/images/Ex1_Step4_3.png)

4. Click __Conditions__.
<br>![](/exercises/ex1/images/Ex1_Step4_4.png)

5. Click __+__ to add a condition.
<br>![](/exercises/ex1/images/Ex1_Step4_5.png)

6. Add the following values to the condition, and then click __Create__.
   - Name: __Temperature over 80__
   - Filter: __Any__
   - Condition Type: __Value monitoring__
   - Data Model Name: __Boiler>>>BoilerTraining>>>Temperature__
   - Operator: __>__
   - Threshold Value: __80__   
<br>![](/exercises/ex1/images/Ex1_Step4_6.png)

7. Click __Outputs__, and then click __+__ to define what should be triggered if this rule is true.
<br>![](/exercises/ex1/images/Ex1_Step4_7.png)

8. For __Output Type__, select __Action__, and then enter __Create Work Order__, and then click __Create__.
<br>![](/exercises/ex1/images/Ex1_Step4_8.png)

9. Click __Enable__ on the top-right to enable the rule.
<br>![](/exercises/ex1/images/Ex1_Step4_9.png)

## Step 1.5 Define runtime settings

After completing these steps you will connect your gateway to a central edge gateway (EBF service), which is configured to create the work order in S/4HANA.

1. Select your project __TechEdXX (where XX is your two-digit participant ID)__ from the navigation.
<br>![](/exercises/ex1/images/Ex1_Step5_1.png)

2. Click __Runtime Settings__, and then click the pencil icon.
<br>![](/exercises/ex1/images/Ex1_Step5_2_1.png)

   In runtime settings, enter the following values, and then click __Save__.
   - Enable EBF Configurations: __On__
   - Host Name: __https://100.24.131.108__
   - Port: __9443__
   - Authentication (Base64): __RUJFRjpJbml0aWFs__
   - Validate Certificates	: __Unchecked__

```
   The last 4 values will only be display if you Enable EBF Configurations!
```
<br>![](/exercises/ex1/images/Ex1_Step5_2_2.png)

   Your screen should look like this now.
<br>![](/exercises/ex1/images/Ex1_Step5_2_3.png)   

## Step 1.6 Define connector settings

After completing these steps you will chave defined runtime settings.

Go to __Connectors__ and select __HTTP Inbound Connector__.
<br>![](/exercises/ex1/images/Ex1_Step6_1.png)

Go to tab __Optional Parameters__, then use the pencil icon to change the values of the following configuration parameters:
   - Authentication Type: __None__
   - Secure Only: __false__
```
In case you don't see parameter "Secure only", you need to drag the pane downwards to reveal 
the full list of optional parameters.
```
<br>![](/exercises/ex1/images/Ex1_Step6_2.png)   
<br>![](/exercises/ex1/images/Ex1_Step6_3.png) 

## Step 1.7 Publish project

After completing these steps you will chave performed a final validation of the project, and then published the project so it can be deployed to the gateways.

First go back to your project.
<br>![](/exercises/ex1/images/Ex1_Step7_1.png) 

Then click __Validate__ on the top-right.
<br>![](/exercises/ex1/images/Ex1_Step7_2.png)   

Once successfully validated, click __Publish__ on top right to publish the project with a configuration name that you can later find, for instance __TechEdXX-V1__, __where XX is your assigned two-digit participant ID__.
<br>![](/exercises/ex1/images/Ex1_Step7_3.png)     

## Summary

You've now defined and validated an edge designer project.

Continue to - [Exercise 2 - Deploy and Test SAP Edge Services to Automatically Create a Work Order based on IoT Sensor Data](../ex2/README.md)

