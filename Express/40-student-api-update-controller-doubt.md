
---


  
bhai mera ek doubt hai  
suppose ek simple student api hai  
jiske schema me name rollno profilepic hai  
  
suppose user ne patch request dala  
update student controller me maine existing Student nikala id lekar  
  

const existingStudent = await Student.findById(req.params.id);

  
  
then aisa bolte hai phle student object ko update kr do uske baad old profile pic delete kro  
  

const updatedData = {};

    // update only those fields which are present in the request body
    // if (name) updatedData.name = name;
    // if (newRollNo) updatedData.rollNo = newRollNo;

    
    if (name !== undefined) updatedData.name = name;
    if (newRollNo !== undefined) updatedData.rollNo = newRollNo;
    if (req.file) updatedData.profile = req.file.path;

    const updatedStudent = await Student.findByIdAndUpdate(
      req.params.id,
      updatedData,
      { new: true, runValidators: true },
    );

  
  
ab iske successfully student object update hone ke baad old image ko delete krenge for cleanup  
  

if (req.file && existingStudent.profile) {
      await deleteFile(existingStudent.profile);
    }

  
  
  
  
mera doubt ye hai ki student update krne ke baad delete kr rhe hai to existingStudent.profile to new profile ko point krne lagega na ??