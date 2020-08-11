# Trigger-to-check-duplicate-name-to-custom-object-in-Salesforce
we are going to learn about How can we prevent duplicate name insert on custom object in Salesforce<br/><br/>
<b>Real time scenarios:-</b> Write a trigger on custom object When a Object (childObjTrigger__c) is being created, if a record exists with the same EmployeeName (EmployeeName__c), the trigger should throw an error.<br/><br/>

<a href="https://www.w3web.net/trigger-to-check-duplicate-name/" rel="nofollow">Go to more details about this article__ <b><i>www.w3web.net</i></b></a><br/><br/>

   public class childObjHandler {
    
    //Do not allow duplicate Employee name
    public static void childDuplicate(List<childObjTrigger__c> dupChildRec){        
        Set<String> empExtStr = new Set<String>();
        List<childObjTrigger__c> empList = [Select Id, Name, EmployeeName__c, childLookup__c, Status__c From childObjTrigger__c where EmployeeName__c !=null];
        
        for(childObjTrigger__c empStr:empList){
            empExtStr.add(empStr.EmployeeName__c);
        }  
        
        for(childObjTrigger__c childTr:dupChildRec){
            if(empExtStr.contains(childTr.EmployeeName__c)){
                childTr.EmployeeName__c.addError('Do not allow duplicate');
            }
        }
    }
}
   trigger childObjTriggerHandler on childObjTrigger__c (before insert, after insert) {
    
    if(trigger.isBefore){
        if(trigger.isInsert){
            system.debug('I am inside before insert');  
            childObjHandler.childDuplicate(trigger.new);
        }
    }
    else if(trigger.isAfter){
        if(trigger.isInsert){
            //system.debug('I am inside after insert');  
            system.debug('Record inserted successfully');              
        }        
    }    
}
