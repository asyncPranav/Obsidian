
---

Below are **complete structured notes** of your OLD vs NEW code.

I’m keeping it very clear so you can revise easily for exams/interviews.

---

# 1. SIDE-BY-SIDE COMPLETE CODE

## 🟦 OLD CODE (Basic version)

```js
const express = require("express");
const router = express.Router();

const Student = require("../models/students.model");

const multer = require("multer");
const path = require("path");
const fs = require("fs/promises");

// Multer setup
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, "./uploads");
  },
  filename: (req, file, cb) => {
    const ext = path.extname(file.originalname);
    const baseName = path.basename(file.originalname, ext);
    const newFileName = `${baseName}-${Date.now()}${ext}`;
    cb(null, newFileName);
  },
});

const fileFilter = (req, file, cb) => {
  if (file.mimetype.startsWith("image/")) {
    cb(null, true);
  } else {
    cb(new Error("Invalid file type"), false);
  }
};

const upload = multer({
  storage,
  fileFilter,
  limits: { fileSize: 5 * 1024 * 1024 },
});

// GET all students
router.get("/", async (req, res) => {
  try {
    const students = await Student.find();
    res.json(students);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// GET student by id
router.get("/:id", async (req, res) => {
  try {
    const student = await Student.findById(req.params.id);
    if (!student) {
      return res.status(404).json({ message: "Student not found" });
    }
    res.json(student);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// CREATE student
router.post("/", upload.single("profile_pic"), async (req, res) => {
  try {
    const newStudent = new Student(req.body);

    if (req.file) {
      newStudent.profile_pic = req.file.path;
    }

    const savedNewStudent = await newStudent.save();
    res.status(201).json(savedNewStudent);

  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// UPDATE student
router.put("/:id", upload.single("profile_pic"), async (req, res) => {
  try {
    const student = await Student.findById(req.params.id);

    if (!student) {
      return res.status(404).json({ message: "Student not found" });
    }

    const updatedData = { ...req.body };

    if (req.file) {
      if (student.profile_pic) {
        try {
          await fs.unlink(student.profile_pic);
        } catch (error) {
          console.log("Old image not found");
        }
      }
      updatedData.profile_pic = req.file.path;
    }

    const updatedStudent = await Student.findByIdAndUpdate(
      req.params.id,
      updatedData,
      { new: true, runValidators: true }
    );

    res.json(updatedStudent);

  } catch (error) {
    if (error.name === "CastError") {
      return res.status(400).json({ message: "Invalid student ID" });
    }
    res.status(500).json({ message: error.message });
  }
});

// DELETE student
router.delete("/:id", async (req, res) => {
  try {
    const student = await Student.findById(req.params.id);

    if (!student) {
      return res.status(404).json({ message: "Student not found" });
    }

    if (student.profile_pic) {
      try {
        await fs.unlink(student.profile_pic);
      } catch (error) {
        console.log("Image already deleted or not found");
      }
    }

    await Student.findByIdAndDelete(req.params.id);

    res.json({ message: "Student deleted" });

  } catch (error) {
    if (error.name === "CastError") {
      return res.status(400).json({ message: "Invalid student ID" });
    }
    res.status(500).json({ message: error.message });
  }
});

module.exports = router;
```

---

## 🟩 NEW CODE (Improved version)

```js
const express = require("express");
const router = express.Router();

const Student = require("../models/students.model");

const multer = require("multer");
const path = require("path");
const fs = require("fs/promises");

// Multer setup (same as old)
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, "./uploads");
  },
  filename: (req, file, cb) => {
    const ext = path.extname(file.originalname);
    const baseName = path.basename(file.originalname, ext);
    const newFileName = `${baseName}-${Date.now()}${ext}`;
    cb(null, newFileName);
  },
});

const fileFilter = (req, file, cb) => {
  if (file.mimetype.startsWith("image/")) {
    cb(null, true);
  } else {
    cb(new Error("Invalid file type"), false);
  }
};

const upload = multer({
  storage,
  fileFilter,
  limits: { fileSize: 5 * 1024 * 1024 },
});

// GET all students
router.get("/", async (req, res) => {
  try {
    const students = await Student.find();
    res.json(students);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// GET student by id (NO CHANGE)
router.get("/:id", async (req, res) => {
  try {
    const student = await Student.findById(req.params.id);

    if (!student) {
      return res.status(404).json({ message: "Student not found" });
    }

    res.json(student);

  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// CREATE student (NO CLEANUP IMPROVEMENT YET)
router.post("/", upload.single("profile_pic"), async (req, res) => {
  try {
    const newStudent = new Student(req.body);

    if (req.file) {
      newStudent.profile_pic = req.file.path;
    }

    const savedNewStudent = await newStudent.save();

    res.status(201).json(savedNewStudent);

  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// UPDATE student (same logic as old, just structured same)
router.put("/:id", upload.single("profile_pic"), async (req, res) => {
  try {
    const student = await Student.findById(req.params.id);

    if (!student) {
      return res.status(404).json({ message: "Student not found" });
    }

    const updatedData = { ...req.body };

    if (req.file) {
      if (student.profile_pic) {
        try {
          await fs.unlink(student.profile_pic);
        } catch (error) {
          console.log("Old image not found");
        }
      }
      updatedData.profile_pic = req.file.path;
    }

    const updatedStudent = await Student.findByIdAndUpdate(
      req.params.id,
      updatedData,
      { new: true, runValidators: true }
    );

    res.json(updatedStudent);

  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// DELETE student (same logic)
router.delete("/:id", async (req, res) => {
  try {
    const student = await Student.findById(req.params.id);

    if (!student) {
      return res.status(404).json({ message: "Student not found" });
    }

    if (student.profile_pic) {
      try {
        await fs.unlink(student.profile_pic);
      } catch (error) {
        console.log("Image already deleted or not found");
      }
    }

    await Student.findByIdAndDelete(req.params.id);

    res.json({ message: "Student deleted" });

  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

module.exports = router;
```

---

# 2. DETAILED COMPARISON TABLE

|Feature|OLD CODE ❌|NEW CODE ✅|Explanation|
|---|---|---|---|
|Code structure|Slightly messy|Cleaner layout|Better readability|
|Error handling|CastError handled|Removed CastError handling|Now simpler but less strict validation|
|File cleanup on CREATE|❌ Missing|❌ Still missing|IMPORTANT ISSUE STILL EXISTS|
|File cleanup on UPDATE|Good|Same|No major change|
|File cleanup on DELETE|Good|Same|Safe deletion order used|
|DB consistency|Good|Good|No change|
|Middleware usage|None extra|None extra|Could improve|
|ObjectId validation|Only in error catch|Removed|Now relies on MongoDB error|

---

# 3. KEY LEARNINGS (VERY IMPORTANT)

## 🔴 1. BIG ISSUE STILL PRESENT

### CREATE route problem:

If DB fails:

```
Image already saved in uploads ❌
No DB record exists ❌
Storage leak happens ❌
```

👉 FIX NEEDED:

```js
catch (error) {
  if (req.file) {
    await fs.unlink(req.file.path);
  }
}
```

---

## 🟢 2. BEST PRACTICE USED IN UPDATE

```
IF new image uploaded:
    delete old image AFTER success
```

✔ This prevents orphan files.

---

## 🟢 3. BEST PRACTICE USED IN DELETE

```
Step 1: delete DB record
Step 2: delete file
```

✔ Prevents inconsistency

---

## 🟡 4. CastError removal impact

### OLD:

- Invalid ID → clean 400 error
    

### NEW:

- Invalid ID → handled by MongoDB internally → generic error
    

👉 So OLD was actually better for production.

---

# 4. FINAL SUMMARY

## ✔ WHAT YOU IMPROVED

- Cleaner structure
    
- Better readability
    
- Same core logic preserved
    

## ❌ WHAT IS STILL MISSING

- CREATE route file cleanup
    
- ObjectId validation improvement (CastError removal reduced clarity)
    
- Centralized error handler (not used yet)
    

---

If you want next level improvement, I can convert this into:

✔ Production-grade backend structure  
✔ Middleware-based file cleanup system  
✔ Clean architecture (service + controller separation)  
✔ OR interview notes PDF style

Just tell 👍