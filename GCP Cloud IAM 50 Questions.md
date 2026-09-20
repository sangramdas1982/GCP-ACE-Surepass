# Google Cloud Engineer Exam - Cloud IAM (50 Practice Questions)

---

## BASIC CONCEPTS & FUNDAMENTALS (Questions 1-10)

**1. What are the three key components of Cloud IAM?**
- A) Roles, Policies, and Permissions
- B) Users, Groups, and Service Accounts
- C) Projects, Folders, and Organization
- D) Compute, Storage, and Networking

**2. Which of the following is NOT an IAM member type in Google Cloud?**
- A) Google Account
- B) Service Account
- C) Google Group
- D) Cloud Account

**3. What is the purpose of IAM policies?**
- A) To encrypt sensitive data
- B) To define who has access to which resources and what actions they can perform
- C) To monitor resource usage
- D) To manage billing accounts

**4. At which levels can IAM policies be granted in Google Cloud? (Select all that apply)**
- A) Organization
- B) Folder
- C) Project
- D) Resource
- E) All of the above

**5. What is the principle of least privilege in IAM?**
- A) Granting maximum permissions to all users
- B) Granting only the minimum permissions necessary for users to perform their jobs
- C) Granting permissions only to administrators
- D) Granting permissions based on geographic location

**6. Which of the following best describes IAM inheritance?**
- A) Child resources automatically inherit IAM policies from parent resources
- B) Parent resources inherit policies from child resources
- C) Policies are never inherited; they must be set at each level
- D) Inheritance only works for Custom roles

**7. What happens when you grant a role at the Organization level?**
- A) It applies only to that specific project
- B) It applies to all projects and resources within the organization
- C) It applies to the organization and all its folders and projects
- D) It requires additional confirmation at the project level

**8. What is an IAM binding?**
- A) A security group that manages access
- B) An association between members and roles for a specific resource
- C) A contract between Google Cloud and users
- D) A type of encryption method

**9. Can you remove inherited IAM roles from a child resource?**
- A) Yes, by deleting the child resource
- B) No, inherited roles cannot be removed
- C) Yes, you can override inherited policies at the child level
- D) Only if you have Organization Admin role

**10. What is the difference between a Policy and a Role in IAM?**
- A) There is no difference; the terms are interchangeable
- B) A Role defines permissions; a Policy grants roles to members
- C) A Policy is for resources; a Role is for users only
- D) A Role is temporary; a Policy is permanent

---

## ROLES & PERMISSIONS (Questions 11-20)

**11. Which type of IAM role is managed by Google Cloud and updated automatically?**
- A) Custom Role
- B) Basic Role
- C) Predefined Role
- D) Service Role

**12. How many Basic roles are available in Google Cloud IAM?**
- A) 2
- B) 3
- C) 5
- D) Unlimited

**13. Which Basic role should be used when you want to grant full control over all Google Cloud resources?**
- A) Editor
- B) Owner
- C) Viewer
- D) Contributor

**14. What is a major disadvantage of using Basic roles in production environments?**
- A) They are too expensive
- B) They don't support multiple users
- C) They grant too many permissions and don't follow the principle of least privilege
- D) They are deprecated

**15. What is a Predefined Role?**
- A) A role created by users for specific requirements
- B) A role managed by Google Cloud with a fixed set of permissions
- C) A temporary role that expires after 30 days
- D) A role that can only be used by administrators

**16. Which of the following is a Compute-related Predefined role?**
- A) roles/compute.admin
- B) roles/storage.admin
- C) roles/resourcemanager.admin
- D) roles/iam.admin

**17. How many permissions does a single IAM role contain?**
- A) Exactly 1
- B) Exactly 5
- C) Varies; can be 1 to many
- D) Maximum of 10

**18. What is the naming convention for Predefined roles?**
- A) roles/service-rolename
- B) roles/serviceName.roleName
- C) role-service-name
- D) roles/SERVICE/ROLENAME

**19. Can you modify a Predefined role?**
- A) Yes, directly
- B) No, you must create a Custom role if you need different permissions
- C) Yes, but only the description
- D) Only Google Cloud can modify them

**20. What is the maximum number of Custom roles you can create per project?**
- A) 10
- B) 50
- C) 300
- D) Unlimited

---

## SERVICE ACCOUNTS (Questions 21-30)

**21. What is a Service Account?**
- A) An account for billing purposes only
- B) A special account that represents an application or service, not a person
- C) An account with special administrative privileges
- D) A temporary account for contractors

**22. How many service accounts can be created per project?**
- A) 1
- B) 10
- C) 100
- D) Hundreds

**23. What is a Service Account Key?**
- A) A password for the service account
- B) A credential that allows applications to authenticate as the service account
- C) A token that expires every hour
- D) A type of encryption key

**24. Which authentication method is recommended for service accounts running on Google Cloud resources?**
- A) Service Account Keys
- B) Workload Identity
- C) Username and password
- D) API tokens

**25. What is the maximum number of keys a service account can have?**
- A) 1
- B) 2
- C) 5
- D) Unlimited

**26. What is the difference between a User-managed and Google-managed service account key?**
- A) User-managed keys never expire; Google-managed keys expire every 90 days
- B) Google-managed keys expire every 90 days; user-managed keys expire every 30 days
- C) User-managed keys are created and rotated by users; Google-managed keys are created and rotated by Google Cloud
- D) There is no difference

**27. Which of the following is a default service account automatically created in every Google Cloud project?**
- A) admin@project-id.iam.gserviceaccount.com
- B) PROJECT_ID@appspot.gserviceaccount.com
- C) default@project-id.iam.gserviceaccount.com
- D) service@project-id.gserviceaccount.com

**28. What is Workload Identity?**
- A) A way to identify users based on their email
- B) A mechanism that allows Kubernetes pods to impersonate service accounts
- C) A permission for workloads to access resources
- D) A feature for managing multiple identities

**29. Can a service account impersonate another service account?**
- A) No, this is not possible
- B) Yes, if the first service account has the appropriate IAM role
- C) Yes, but only Google can do it
- D) Only between projects

**30. What role would you assign to a service account that needs to read data from Cloud Storage?**
- A) roles/storage.admin
- B) roles/storage.objectViewer
- C) roles/viewer
- D) roles/iam.securityReviewer

---

## POLICY MANAGEMENT & BEST PRACTICES (Questions 31-40)

**31. Where can you view the IAM policy for a resource?**
- A) IAM & Admin section in Console
- B) Resource settings page
- C) Activity logs
- D) Billing section

**32. What is an IAM Condition?**
- A) A requirement that must be met before granting access
- B) A time-based or resource-based rule for limiting when a policy applies
- C) A condition in a contract
- D) A type of role

**33. Which of the following is a valid IAM Condition?**
- A) Access granted only during business hours
- B) Access granted only from specific IP addresses
- C) Access granted only for resources with specific labels
- D) All of the above

**34. What is an Organization Policy (formerly called Constraints)?**
- A) A type of IAM role
- B) A policy that restricts what members can do with resources, regardless of IAM roles
- C) A policy for organizing resources
- D) A backup policy

**35. Can an Organization Policy override an IAM role?**
- A) No, IAM roles always take precedence
- B) Yes, Organization Policies restrict access even if IAM roles grant it
- C) It depends on the resource type
- D) They cannot be used together

**36. What is the purpose of Service Account Impersonation?**
- A) To pretend to be an administrator
- B) To allow one service account or user to act as another service account
- C) To hide the actual user performing actions
- D) To bypass IAM policies

**37. Which role would you assign to a user who needs to manage IAM policies in a project?**
- A) roles/editor
- B) roles/resourcemanager.projectIamAdmin
- C) roles/iam.admin
- D) roles/compute.admin

**38. What is the best practice for managing permissions for a large team?**
- A) Grant individual permissions to each team member
- B) Use Google Groups to manage collective access
- C) Grant Owner role to all team members
- D) Use only service accounts

**39. How can you audit who has what permissions in your Google Cloud organization?**
- A) IAM Policy Analyzer
- B) Cloud Audit Logs
- C) Security Command Center
- D) All of the above

**40. When should you use Custom roles instead of Predefined roles?**
- A) Always use Custom roles for security
- B) Only when no Predefined role meets your requirements
- C) When you want to follow Google's recommendations
- D) Never; always use Predefined roles

---

## ADVANCED SCENARIOS & TROUBLESHOOTING (Questions 41-50)

**41. A user has the compute.instances.get permission but cannot view an instance. What could be the reason?**
- A) The permission is wrong
- B) An Organization Policy might be restricting access
- C) The user doesn't have the compute.instances.list permission
- D) All of the above

**42. What is the difference between roles/viewer and roles/monitoring.viewer?**
- A) They are identical
- B) roles/viewer grants read access to all resources; roles/monitoring.viewer grants read access only to monitoring resources
- C) roles/monitoring.viewer is deprecated
- D) They have different expiration dates

**43. Can you grant IAM roles at the resource level (e.g., on a specific Cloud Storage bucket)?**
- A) No, only at the project level
- B) No, only at the organization level
- C) Yes, for some resources like Cloud Storage buckets and Compute instances
- D) Yes, for all resources

**44. What happens if you delete an Organization that has projects?**
- A) All projects are automatically deleted
- B) All projects are moved to a default organization
- C) Projects are retained but no longer managed under an organization
- D) You cannot delete an organization with projects

**45. How can you prevent accidental deletion of resources by users with admin roles?**
- A) Remove their admin role
- B) Use Organization Policies with delete restrictions
- C) Use IAM Conditions to restrict delete operations
- D) Both B and C are valid approaches

**46. What is the purpose of the Cloud IAM Policy Analyzer?**
- A) To analyze the performance of your services
- B) To identify which members have access to specific resources
- C) To monitor API calls
- D) To encrypt data

**47. Can you grant cross-project IAM roles?**
- A) No, roles only work within the same project
- B) Yes, you can grant access to resources in one project to members from another project
- C) Only if both projects are in the same organization
- D) Only for service accounts

**48. What should you do before granting a sensitive role like Organization Admin?**
- A) Nothing; just grant it
- B) Verify the user's identity and document the reason
- C) Require two-factor authentication
- D) Both B and C

**49. How often should you audit IAM permissions?**
- A) Never; once set, permissions don't need review
- B) Only when there's a security incident
- C) Regularly (e.g., quarterly) to ensure least privilege is maintained
- D) Only annually

**50. What is the difference between granting roles at the Folder level vs. the Project level?**
- A) Folder level applies to all projects within the folder; Project level applies only to that project
- B) They are identical
- C) Folder level is deprecated
- D) Project level has more restrictions

---

## ANSWER KEY

| Q | Answer | Q | Answer | Q | Answer | Q | Answer | Q | Answer |
|---|--------|---|--------|---|--------|---|--------|---|--------|
| 1 | B | 11 | C | 21 | B | 31 | A | 41 | D |
| 2 | D | 12 | B | 22 | D | 32 | B | 42 | B |
| 3 | B | 13 | B | 23 | B | 33 | D | 43 | C |
| 4 | E | 14 | C | 24 | B | 34 | B | 44 | C |
| 5 | B | 15 | B | 25 | D | 35 | B | 45 | D |
| 6 | A | 16 | A | 26 | C | 36 | B | 46 | B |
| 7 | C | 17 | C | 27 | C | 37 | B | 47 | B |
| 8 | B | 18 | A | 28 | B | 38 | B | 48 | D |
| 9 | C | 19 | B | 29 | B | 39 | D | 49 | C |
| 10 | B | 20 | C | 30 | B | 40 | B | 50 | A |

---

## STUDY TIPS

- **Focus Areas**: Service Accounts, Predefined vs Custom Roles, IAM inheritance
- **Hands-on Practice**: Create and manage IAM policies in a test project
- **Official Resources**: Review Google Cloud IAM documentation and best practices guide
- **Key Concepts to Master**:
  - IAM hierarchy and policy inheritance
  - Service account authentication methods
  - Principle of least privilege
  - Organization Policies vs IAM roles
  - Cross-project access patterns
