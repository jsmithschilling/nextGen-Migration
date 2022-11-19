# eMDs Solution Series data collection for NextGen Migration

  * Please note that this should serve as a starting point of being able to get some of the information that NextGen is requesting.
  * eMDs Support did not assist in data exporting and left me on my own to-do so. This is made to hopefully help others.
  * This may differ from your eMDs environment
  
  Exported from version eMDs Solution Series 9.1.0 (June 2, 2022)
  
  
  
  ***
  
  ### Demographics Custom View
  
  `nextGenDemographicView`
  
  Create a Custom View for Demographics using this statement:
  
 ``` 
 SELECT        dbo.VIEW_Patient.Entity_ID AS entityID, dbo.VIEW_Patient.Person_SSN AS ssn, dbo.VIEW_Patient.Person_ConfidentialEmail AS email, dbo.VIEW_Phone.Phone_Number AS phoneNumber,
                         dbo.VIEW_Phone.Phone_Extension AS phoneExt, dbo.VIEW_Patient.EmployeeStatus_ID, dbo.VIEW_Address.Address_Line1, dbo.VIEW_Address.Address_Line2, dbo.VIEW_Address.Address_City,
                         dbo.VIEW_Address.Address_ZipCode, dbo.VIEW_Address.State_Abbreviation, dbo.INSR_Insurance.Insurance_PolicyNum, dbo.VIEW_Patient.Person_FirstName AS firstName,
                         dbo.VIEW_Patient.Person_MiddleName AS middleName, dbo.VIEW_Patient.Person_LastName AS lastName, dbo.VIEW_Patient.Person_DateOfBirth AS dateofBirth, dbo.VIEW_Patient.Person_Sex AS sex,
                         cdar21.VIEW_Demographics.maritalStatusDisplayName AS maritalStatus, cdar21.VIEW_Demographics.raceDisplayName AS race, cdar21.VIEW_Demographics.patientRelationshipToGuarantor,
                         dbo.VIEW_Insurance.Organization_Name, dbo.INSR_Insurance.Insurance_ID, dbo.VIEW_Insurance.InsuranceCompany_MDDPayorID, dbo.VIEW_Insurance.InsuranceCompany_MDCPayorID,
                         dbo.VIEW_Patient.Patient_ID AS patientID
FROM            dbo.VIEW_Patient INNER JOIN
                         dbo.VIEW_Phone ON dbo.VIEW_Patient.Entity_ID = dbo.VIEW_Phone.Entity_ID INNER JOIN
                         dbo.VIEW_Address ON dbo.VIEW_Patient.Entity_ID = dbo.VIEW_Address.Entity_ID INNER JOIN
                         dbo.INSR_Insurance ON dbo.VIEW_Patient.Entity_ID = dbo.INSR_Insurance.Entity_ID INNER JOIN
                         cdar21.VIEW_Demographics ON dbo.VIEW_Patient.Entity_ID = cdar21.VIEW_Demographics.entityId INNER JOIN
                         dbo.VIEW_Insurance ON dbo.INSR_Insurance.Insurance_ID = dbo.VIEW_Insurance.Insurance_ID
```

***

### Appointments Custom View

`nextGenAppointmentsView`

Create a Custom View for Appointments using this statement:

```
SELECT        dbo.VIEW_Appointment.Appointment_AppStartTime, dbo.VIEW_Appointment.Appointment_AppEndTime, dbo.VIEW_Appointment.Patient_ID, dbo.VIEW_Appointment.Appointment_Notes, dbo.VIEW_Patient.Entity_ID,
                          dbo.VIEW_Physician.Person_FirstName, dbo.VIEW_Physician.Person_LastName, dbo.APP_AppointmentType.AppointmentType_Description
FROM            dbo.VIEW_Appointment INNER JOIN
                         dbo.VIEW_Patient ON dbo.VIEW_Appointment.Patient_ID = dbo.VIEW_Patient.Patient_ID INNER JOIN
                         dbo.VIEW_Physician ON dbo.VIEW_Appointment.Provider_ID = dbo.VIEW_Physician.Provider_ID INNER JOIN
                         dbo.APP_AppointmentType ON dbo.VIEW_Appointment.AppointmentType_ID = dbo.APP_AppointmentType.AppointmentType_ID
```

eMDs User's may have put commas within their `Appointment_Notes` which will cause issues with columns being further down and mess up the data alignment. 

### Notepad++

This Pattern Match will replace any commas within the `Appointment_Notes` column between `,"NOTES, ARE, HERE",`

Find what: `("[^",]+),([^"]+")`

Replace with: `$1$2`

You will have to hit `Replace All` multiple times until it does not replace any more.

![alt text](https://github.com/jsmithschilling/nextGen-Migration/blob/main/notepadregex.png "Notepad++ RegEx")

*Example of a row with commas in the `Appointment_Notes`

```
Appointment_AppStartTime,Appointment_AppEndTime,Patient_ID,Appointment_Notes,Entity_ID,Person_FirstName,Person_LastName,AppointmentType_Description

2008-10-22 10:20:00.000,2008-10-22 11:00:00.000,1234567,"PHYSICAL, FINDING, LUMPS ON BODY NP  LS",7654321,JOHN,DOE,NEW PATIENT
```

### Multimedia Custom View

`nextGenMultimediaView`

This custom view is, so we can tie a Patient to their documents that were uploaded within eMDs as eMDs Image storage does not place a single person's documents in their own unique folder (in my case)

Create a Custom View for Multimedia using this statement:

```
SELECT        dbo.IMG_ImageDoc.ImageDoc_Description AS imageDesc, dbo.TRK_TrackChart.Patient_ID AS patientID, dbo.IMG_Image.Image_FileName AS fileName, dbo.VIEW_Patient.Person_LastName AS lastName, 
                         dbo.VIEW_Patient.Person_FirstName AS firstName, dbo.VIEW_Patient.Entity_ID AS entityID
FROM            dbo.TRK_TrackChart INNER JOIN
                         dbo.IMG_ImageDoc ON dbo.TRK_TrackChart.ImageDoc_ID = dbo.IMG_ImageDoc.ImageDoc_ID INNER JOIN
                         dbo.IMG_Image ON dbo.IMG_ImageDoc.ImageDoc_ID = dbo.IMG_Image.ImageDoc_ID INNER JOIN
                         dbo.VIEW_Patient ON dbo.TRK_TrackChart.Patient_ID = dbo.VIEW_Patient.Patient_ID
```

***

### Consulting Information

If you are need of assistance for your eMDs to NextGen migration then we do offer "Service"

Reach out to us at "email" for a quote!
