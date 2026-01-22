
# 🔐 Salesforce Platform Encryption (Shield)

**Protect sensitive data while preserving business functionality**

---

## 📌 What is Platform Encryption?

Salesforce Platform Encryption allows you to encrypt sensitive data at rest while maintaining core platform functionality such as search, validation, and automation.

* Industry-standard **AES 256-bit encryption**
* Field-level, file, and attachment encryption
* Customer-controlled encryption keys

---

## 🔒 AES 256-bit Encryption

* **AES 256-bit** is the highest level of encryption available in Salesforce
* Meets strict compliance and regulatory requirements
* Used consistently across:

  * Fields
  * Files
  * Attachments

---

## 🧱 At-Rest Encryption

* Encrypts data **when stored in Salesforce data centers**
* Plain text is converted into **cipher text**
* Prevents database-level access to readable data
* Salesforce infrastructure agents cannot view real data

---

## 🧩 Field-Level Encryption

* Encrypts individual fields containing sensitive information
* Salesforce **does not provide field-level encryption out of the box**
* Platform Encryption enables encryption while preserving:

  * Validation rules
  * Workflow automation
  * Apex logic (with limitations)

---

## 📁 Files & Attachments Encryption Policy

* Encryption is **all-or-nothing**
* Cannot selectively encrypt individual files or attachments
* Applies uniformly across the org

---

## 🔑 Encryption Key Management

Salesforce offers **three key management options**:

* Salesforce-managed keys
* Customer-managed tenant secrets
* **Customer-Driven Key Management (CDKM)**

Key lifecycle control includes:

* Key rotation
* Key revocation
* Key expiration policies

---

## ✅ Business Value

* Prevents unauthorized data access
* Customer-controlled key ownership and lifecycle
* Encrypts **fields, files, and attachments** while maintaining functionality
* Seamlessly upgraded with every Salesforce release
* Helps meet compliance and regulatory requirements

---

## ⚠️ Risks & Challenges

* Requires **extensive testing** to avoid functional regressions
* Some platform features are restricted with encrypted fields
* Dependency on **AWS Key Management Service (KMS)**

➡️ Mitigation planning is required before enablement

---

## ⚖️ Probabilistic vs Deterministic Encryption

### 🔀 Probabilistic Encryption

* Fully randomized Initialization Vector (IV)
* Same input produces **different cipher text**
* **More secure**
* Limited filtering and sorting capabilities

### 🎯 Deterministic Encryption

* Static Initialization Vector per field
* Same input always produces **same cipher text**
* Supports **exact-match filtering**
* Slightly less secure than probabilistic

---

## 🚫 Key Considerations & Limitations

* **Currency fields** are not supported
* Not all managed packages support encryption (e.g., ZoomInfo)
* Encrypted fields:

  * Can’t be used in **criteria-based sharing rules**
  * Can’t be sorted in **list views**
* Login Discovery Handlers relying on encrypted email fields may fail

---

## 🚫 SOQL & SOSL Restrictions

Fields encrypted with **probabilistic encryption** cannot be used in:

* `WHERE` clause
* `GROUP BY`
* `ORDER BY`
* Aggregate functions:

  * `MAX()`
  * `MIN()`
  * `COUNT_DISTINCT()`

---

## 🔄 Encryption Lifecycle & Data Sync

* Newly created or updated data is encrypted immediately
* Existing data is encrypted via **background encryption service**
* Key rotation:

  * Old data remains encrypted with the previous key
  * Re-encryption is optional via background service
* No impact on:

  * Triggers
  * Workflow rules
  * Validation rules
* **SystemModStamp is updated**

---

## 🧾 Supported Field Types

Platform Encryption supports the following custom field types:

* Text
* Text Area
* Email
* Phone
* URL
* External ID
* Date
* Date/DateTime

⚠️ Only these data types are supported for encryption

---

## 🏁 Summary

* Platform Encryption secures sensitive data at rest
* AES 256-bit encryption with customer-controlled keys
* Enables compliance while preserving business processes
* Requires careful planning, testing, and stakeholder alignment

