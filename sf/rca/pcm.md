 

### 📦 Product Catalog Management

#### Standard Objects Overview

*Manage products, rules, and catalogs effectively in Salesforce Revenue Cloud*
- [PCM Mdoel](https://mchinnappan100.github.io/pages2/clouds/revenue/model/pcm.html)

---

### 📚 What is Product Catalog Management?

* A structured data model in Revenue Cloud
* Manages:

  * Products
  * Attribute rules
  * Qualification/disqualification logic
  * Bundles and configurations
* Based on standard and customizable objects

---

### 🧩 AttributeCategory

* Logical grouping of attributes
* Reusable in product definitions
* Aids in **searching & managing attributes**

🔹 **Example**:
**"Mobile Handset Properties"** category → `Color`, `Storage`, `Model`, `Size`

🧪 API Version: **60.0+**

---

### 🔗 AttributeCategoryAttribute

* Links an **Attribute Category** to an **Attribute Definition**
* Represents the mapping/relationship

🧪 API Version: **60.0+**

---

### 🚫 AttrPicklistExcludedValue

* Represents **excluded picklist values** for:

  * Product classification attributes
  * Product attribute definitions

🔍 Ensures invalid combinations are not shown

🧪 API Version: **61.0+**

---

### 🧬 ProductAttributeDefinition

* Maps attributes to specific products
* Foundation for defining **dynamic and flexible products**

🧪 API Version: **60.0+**

---

### ❌ ProductCategoryDisqual

* Disqualification **rules for product categories**
* Hides categories based on business logic

🧪 API Version: **60.0+**

---

### ✅ ProductCategoryQualification

* Qualification **rules for product categories**
* Displays categories when criteria match

🧪 API Version: **60.0+**

---

### 🧱 ProductClassification

* Template holding a collection of **dynamic attributes**
* Used to define & generate **similar product variants quickly**

🧪 API Version: **60.0+**

---

### 🔁 ProductClassificationAttr

* Defines default **attribute configuration**
* Based on product classification template

🧪 API Version: **60.0+**

---

### 🎛️ ProductComponentGrpOverride

* Allows override of **component group rules**
* Used in **product bundles**
* Supports **custom cardinality** settings

🧪 API Version: **60.0+**

---

### 🚫 ProductDisqualification

* Disqualification **rules for products**
* Evaluated at runtime based on **user context**

🧪 API Version: **60.0+**

---

### ✅ ProductQualification

* Qualification **rules for products**
* Determines **eligibility to show** products dynamically

🧪 API Version: **60.0+**

---

### ⏳ ProductRampSegment

* Represents a **ramp period** in ramp deals
* Captures **volume, term, pricing changes over time**

🧪 API Version: **62.0+**

---

### ♻️ ProductRelComponentOverride

* Cardinality overrides for **components in a bundle**
* Fine-tunes bundle structure

🧪 API Version: **60.0+**

---

### 🧾 ProductSpecificationRecType

* Links industry-specific **product specs** with **record types**

🧪 API Version: **60.0+**

---

### 🏷️ ProductSpecificationType

* Defines **terminology** for product specs by industry
* Linked to `ProductSpecificationRecType`

🧪 API Version: **60.0+**

---

### 📘 Summary

* 15+ Standard Objects for flexible catalog design
* Support for:

  * Attribute management
  * Classification
  * Rules engine
  * Ramp deals
  * Industry-specific customization

✅ Powered by Salesforce Revenue Cloud!

---

 