### SalesforceDynamicPicklists-VF-
A kit for doing dynamic picklists in Visualforce.  Will update for Lightning later.

We will achieve this functionality using visualforce. You can then extend the code provided here to as many Visualforce pages as you want.


Step 1:

First of all, we will have to create an object to hold all the picklist values. So, go ahead and create the following..

Object name : DynamicPicklist
Field Names:
Name (Datatype : Autonumber)
PicklistValue (Datatype: text)

Step 2:

Add picklist values to the object created.. (ie) Add records to the newly created object...

For example..

Record 1: PicklistValue= 'Delhi'
Record 2: PicklistValue = 'Mumbai'

and so on...........

Now also add a value called '--Other--'. This value will be used for the user to enter a new value.

Step 3:

Creation of the Visualforce page and the Apex Class.

Apex Code.. Save this first


public class dynamicpicklist
{
public String city{get; set;}

public List<SelectOption> getcitynames()
{
  List<SelectOption> options = new List<SelectOption>();
  List<DynamicPicklist__c> citylist = new List<DynamicPicklist__c>();
  citylist = [Select Id, PicklistValue__c FROM DynamicPicklist__c ];
  options.add(new SelectOption('--None--','--None--'));
  for (Integer j=0;j<citylist.size();j++)
  {
      options.add(new SelectOption(citylist[j].PicklistValue__c,citylist[j].PicklistValue__c));
  }
  return options;
}
public String newpicklistvalue{get; set;}

public void saverec()
{
  DynamicPicklist__c newrec = new DynamicPicklist__c(PicklistValue__c=newpicklistvalue);
  insert newrec;
  newpicklistvalue=NULL;
}

}


Visualforce code...

<apex:page controller="dynamicpicklist" sidebar="false" >
<apex:form >
<apex:sectionHeader title="Dynamic Picklist" subtitle="Reusable code"/>
<apex:pageblock >
<apex:pageBlockSection title="Dynamic picklist" columns="1">
      <apex:pageblocksectionItem >
          <apex:outputlabel value="City" for="values" />
          <apex:selectList value="{!city}" size="1" id="values">
              <apex:actionSupport event="onchange" reRender="newvalue" />
              <apex:selectOptions value="{!citynames}"/>
          </apex:selectList>
      </apex:pageblocksectionItem>                                        
          <apex:outputpanel id="newvalue">
             <apex:outputpanel rendered="{!city == '--Other--'}">
             <div style="position:relative;left:75px;">             
                  <apex:outputlabel value="New value" for="newval" />
                  <apex:inputText value="{!newpicklistvalue}" id="newval"/>
                  <apex:commandbutton action="{!saverec}" value="Add!"/>
             </div>
             </apex:outputpanel>
          </apex:outputpanel>             
  </apex:pageblocksection>
</apex:pageblock>
</apex:form>
</apex:page>

How to extend this code???

Are you wondering if you will have to create one object for one picklist and suppose you have ten picklists like these, then ten objects... That is really a mess...

So, why not create ten fields in the same object.. Field1 represents picklist1, field2 picklist2 and so on...

But, be clear on the way you retrieve values and insert values into this object... For ex: I have not performed the validation to check if the newly entered value already exists..

Comments and Queries most welcome!!!!

http://www.forcetree.com/2009/06/dynamically-add-values-to-picklist.html
